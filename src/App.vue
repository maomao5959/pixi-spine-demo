<template>
  <div id="app">
    <div class="hero">
      <!-- <hero
        :heroCode="value.unit"
        :headCode="value.head"
        :handCode="value.hand"
        :weaponCode="value.weapon"
        :bodyCode="value.body"
      /> -->
      <hero :heroCode="value.unit" />
    </div>
  </div>
</template>

<script>
import hero from "./components/hero.vue";

export default {
  name: "App",
  components: {
    hero,
  },
  data() {
    return {
      value: {
        unit: "101",
        head: "11",
        hand: "26",
        weapon: "46",
        body: "32",
      },
    };
  },
};
</script>

<style lang="less">
* {
  margin: 0;
  padding: 0;
}
#app {
  // background: black;
  width: 900px;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  .hero {
    width: 500px;
    height: 300px;
  }
}
</style>
