<template>
  <div class="heros">
    <!--  -->
    <!-- <div class="mask"></div> -->
    <canvas ref="heroCanvas"></canvas>
  </div>
</template>

<script>
import * as PIXI from "pixi.js";
import { Spine } from "pixi-spine";
export default {
  name: "PixiDemoHero1",
  props: {},
  data() {
    return {
      publicPath: process.env.VUE_APP_PUBLIC_PATH,
      $_dragonCage: null,
      $_app: null,
    };
  },

  mounted() {
    this.init();
  },
  beforeDestroy() {
    if (this.$_dragonCage) {
      this.$_dragonCage.destroy(true);
    }
    if (this.$_app) {
      this.$_app.destroy(true);
    }
  },
  methods: {
    init() {
      const that = this;
      that.$_app = new PIXI.Application({
        antialias: true,
        backgroundAlpha: 0,
        resolution: 1,
        view: this.$refs.heroCanvas,
      });
      that.$_app.stop();
      that.$_app.loader
        .add("goblins", `${this.publicPath}resourcev12/101_succbus.json`)
        .load(onAssetsLoaded);

      that.$_app.stage.interactive = true;
      // that.$_app.stage.buttonMode = true;
      function onAssetsLoaded(loader, res) {
        const hero = new Spine(res.goblins.spineData);
        // console.log(hero,'hero');
        // set current skin
        hero.skeleton.setSkinByName("default");
        console.log(hero, "hero");
        const currentSkinName = hero.skeleton.skin.constructor;
        const newSkin = new currentSkinName("newSkin");
        const newSkin1 = new currentSkinName("newSkin1");
        newSkin.addSkin(hero.spineData.findSkin("body/body_1"));
        newSkin.addSkin(hero.spineData.findSkin("weapon/weapon_1"));
        newSkin.addSkin(hero.spineData.findSkin("hand/hand_1"));
        newSkin.addSkin(hero.spineData.findSkin("head/head_1"));
        newSkin.addSkin(hero.spineData.findSkin("common/common_1"));
        newSkin1.addSkin(hero.spineData.findSkin("body/body_2"));
        newSkin1.addSkin(hero.spineData.findSkin("weapon/weapon_2"));
        newSkin1.addSkin(hero.spineData.findSkin("hand/hand_2"));
        newSkin1.addSkin(hero.spineData.findSkin("head/head_2"));
        newSkin1.addSkin(hero.spineData.findSkin("common/common_2"));
        hero.skeleton.setSkin(newSkin);
        hero.skeleton.setSlotsToSetupPose();

        // create a container for the spine animation and add the animation to it
        that.$_dragonCage = new PIXI.Container();
        that.$_dragonCage.addChild(hero);

        // measure the spine animation and position it inside its container to align it to the origin
        const localRect = hero.getLocalBounds();
        hero.position.set(-localRect.x, -localRect.y);

        // now we can scale, position and rotate the container as any other display object
        const scale = Math.min(
          that.$_app.screen.width / that.$_dragonCage.width,
          that.$_app.screen.height / that.$_dragonCage.height
        );
        that.$_dragonCage.scale.set(scale, scale);
        that.$_dragonCage.position.set(
          (that.$_app.screen.width - that.$_dragonCage.width) * 0.5,
          (that.$_app.screen.height - that.$_dragonCage.height) * 0.5
        );

        // add the container to the stage
        that.$_app.stage.addChild(that.$_dragonCage);
        hero.state.addAnimation(0, "stand", true, 0);
        that.$_app.stage.on("pointertap", () => {
          const skin = hero.skeleton.skin.name;
          if (skin === "newSkin") {
            hero.skeleton.setSkin(newSkin1);
            hero.state.addAnimation(0, "attack_1", true, 0);
            hero.skeleton.setSlotsToSetupPose();
          } else {
            hero.skeleton.setSkin(newSkin);
            hero.state.addAnimation(0, "attack_2", true, 0);
            hero.skeleton.setSlotsToSetupPose();
          }
        });
      }
      that.$_app.start();
    },
  },
};
</script>

<style lang="less" scoped>
.heros {
  width: 100%;
  position: relative;
  .mask {
    position: absolute;
    width: 100%;
    height: 100%;
    background: transparent;
    z-index: 1;
  }
  canvas {
    width: 90% !important;
  }
}
</style>