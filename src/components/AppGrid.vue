<template>
  <div class="scene -gallery" ref="gallery">
    <div
      @click="expand(item, $event)"
      v-for="item in food"
      class="item"
      :key="item"
      :data-key="item"
      ref="itemimg"
    >
      <img :src="`${item}.jpg`" :alt="item" :ref="item">
    </div>
    <app-details v-if="isShowing"></app-details>
  </div>
</template>

<script>
import AppDetails from "./AppDetails.vue";
import { TimelineMax, Sine } from "gsap";

export default {
  components: {
    AppDetails
  },
  data() {
    return {
      isShowing: false,
      food: [
        "appetizer",
        "avocado",
        "berries",
        "bowl",
        "breakfast",
        "burger2",
        "corn",
        "dumpling",
        "egg",
        "egg2",
        "fig",
        "fries",
        "oyster",
        "pancake",
        "pasta",
        "pasta2",
        "pizza",
        "plates",
        "popcicle",
        "salmon",
        "soup",
        "steak",
        "steamed",
        "toast",
        "tomato"
      ],
      rects: {
        first: null,
        last: null
      }
    };
  },
  methods: {
    expand(item, event) {
      const itemEl = event.target;
      const elImg = this.$refs[item][0];

      this.rects.first = itemEl.getBoundingClientRect();
      this.rects.last = this.$refs.gallery.getBoundingClientRect();

      if (!this.isShowing) {
        this.isShowing = true;

        let deltaW = this.rects.first.left - this.rects.last.left;
        let deltaH = this.rects.first.top - this.rects.last.top;
        let deltaS = this.rects.last.width / this.rects.first.width;

        TweenMax.to(elImg, 0.3, {
          x: -deltaW,
          y: -deltaH,
          scale: deltaS,
          transformOrigin: "0 0",
          zIndex: 1000,
          ease: Sine.easeOut
        });
      } else {
        TweenMax.to(elImg, 0.3, {
          x: 0,
          y: 0,
          scale: 1,
          transformOrigin: "0 0",
          zIndex: 1,
          ease: Sine.easeIn
        });

        this.isShowing = false;
      }
    }
  }
};
</script>

<style lang="scss">
$app-width: 600px;
$app-height: 100vh;

.scene {
  display: flex;
  position: relative;
  top: 0;
  left: 0;
  width: $app-width;
  height: $app-height;
  max-height: 100%;
  overflow-y: scroll;

  &.-gallery {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: flex-start;

    > .item {
      flex-basis: 30%;
      flex-grow: 0;
      flex-shrink: 0;
      height: auto;
      min-height: $app-width / 3;
      // overflow: hidden;
    }
  }
}

.item {
  transform-origin: top left;
  cursor: pointer;
  position: relative;
  transform-origin: 0 0;

  > img {
    height: auto;
    width: 100%;
    position: relative;
    backface-visibility: hidden;
  }
}
</style>