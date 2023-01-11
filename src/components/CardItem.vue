<template>
  <div
    class="card"
    :class="{ disable: isDisabled }"
    :style="{
      height: `${(900 - 17 * 4) / Math.sqrt(cardsContext.length) - 17}px`,
      width: `${
        (((900 - 17 * 4) / Math.sqrt(cardsContext.length) - 17) * 3) / 4
      }px`,
      perspective: `${
        (2 * (((900 - 17 * 4) / Math.sqrt(cardsContext.length) - 17) * 3)) / 4
      }px`,
    }"
  >
    <div
      class="card__inner"
      @click="onToggleRotateCard"
      :class="{ 'is-flipped': isFlipped }"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((900 - 17 * 4) / Math.sqrt(cardsContext.length) - 17) * 3) /
              4 /
              3
            }px ${
              (((900 - 17 * 4) / Math.sqrt(cardsContext.length) - 17) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [String, Number, Array, Object],
    },
    imgBackFaceUrl: {
      type: String,
      require: true,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isFlipped: false,
    };
  },
  methods: {
    onToggleRotateCard() {
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },

    onFlipBackCard() {
      this.isFlipped = false;
    },

    onEnableDisabledCard() {
      this.isDisabled = true;
    },
  },
};
</script>

<style scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disable .card__inner {
  cursor: default;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  padding: 10px;
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

.card__face--back {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--light);
  color: var(--dark);
  transform: rotateY(180deg);
}

.card__face--back .card__content {
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}
</style>
