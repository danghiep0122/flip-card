<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((1000 - 17 * 4) / Math.sqrt(cardsContext.length) - 17) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-item-vue
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardItemVue from "./CardItem.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: () => [],
    },
  },
  components: {
    CardItemVue,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisabledCard();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisabledCard();
        this.rules = [];
        console.log("right");

        const disabledElements = document.querySelectorAll(
          ".screen .card.disable"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          console.log("game over");
          setTimeout(() => {
            this.$emit("onFinish");
          }, 900);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          console.log("Wrong");
          this.rules = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background: var(--dark);
}

.screen__inner {
  height: 90vh;
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
