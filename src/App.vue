<template>
  <div id="app">
    <div class="container" :style="{transform: 'rotateY(' + degValue + 'deg)'}">
      <div class="flex-wrap front">
        <div class="increment">
          <IncrementCount/>
        </div>
        <div class="show-front">{{ fontCount }}</div>
        <div class="decrement">
          <DecreaseCount/>
        </div>
      </div>
      <div class="flex-wrap back">
        <div class="increment">
          <IncrementCount/>
        </div>
        <div class="show-back">{{ backCount }}</div>
        <div class="decrement">
          <DecreaseCount/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import IncrementCount from "./components/IncrementCount";
import DecreaseCount from "./components/DecreaseCount";
import { EventBus } from "./event-bus.js";

export default {
  name: "App",
  components: {
    IncrementCount,
    DecreaseCount
  },
  data() {
    return {
      degValue: 0,
      fontCount: 0,
      backCount: 0
    };
  },
  mounted() {
    EventBus.$on("incremented", ({ num, deg }) => {
      this.fontCount += num;
      this.$nextTick(() => {
        this.backCount += num;
        this.degValue += deg;
      });
    });
    EventBus.$on("decreased", ({ num, deg }) => {
      this.fontCount -= num;
      this.$nextTick(() => {
        this.backCount -= num;
        this.degValue -= deg;
      });
    });
    // EventBus.$off("incremented");
    // EventBus.$off("decreased");
  }
};
</script>

<style>
.container {
  width: 100px;
  margin: 100px auto;
}
.flex-wrap {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
}
</style>
