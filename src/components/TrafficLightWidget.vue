<template>
  <div
    class="trafic-bg"
    @click="handleClick"
    @mouseleave="handleMouseLeave"
    :class="{ enlarged: isEnlarged }"
    :style="{
      'background-color': IsLightMode
        ? colorTypes?.LIGHTBACKGROUND
        : colorTypes?.DARKBACKGROUND,
    }"
  >
    <transition name="fade" mode="out-in">
      <div
        :key="isEnlarged"
        :class="{ 'traffic-light': !rotation, 'traffic-light-1': rotation }"
      >
        <div
          v-for="light in [1, 2, 3]"
          :key="light"
          class="light"
          :style="{
            width: isEnlarged ? '86px' : '80px',
            height: isEnlarged ? '86px' : '80px',
          }"
          @click="changeColor(light)"
          :class="{
            redactive: light === 1 && currentLight === 1,
            yellowactive: light === 2 && currentLight === 2,
            greenactive: light === 3 && currentLight === 3,
          }"
        ></div>
      </div>
    </transition>

    <!-- bottom buttons -->
    <transition name="fade" mode="out-in">
      <div
        :key="this.isEnlarged"
        v-if="!this.isEnlarged"
        :class="{
          'bottom-buttons-traffic': !rotation,
          'bottom-buttons-1': rotation,
        }"
        class="d-flex justify-content-between pe-2"
      >
        <div>
          <img
            :src="RottetImg"
            class="pointer"
            alt="reset timer"
            :style="{ width: '36px', height: '36px' }"
            @click="RottetLights"
          />
        </div>
        <div>
          <img
            v-if="IsSoundOn"
            :src="VolumOn"
            class="pointer"
            alt="Volume On"
            :style="{ width: '36px', height: '36px' }"
            @click="stopSound"
          />
          <img
            v-else
            :src="VolumOff"
            class="pointer"
            alt="Volume Off"
            :style="{ width: '36px', height: '36px' }"
            @click="startSound"
          />
        </div>
      </div>
    </transition>
  </div>
  <audio ref="tickAudio" :src="ClickSound"></audio>
</template>

<script>
import lottie from "lottie-web";
import animationData from "../assets/images/celebration.json";
import ClickSound from "@/assets/sound/TrafficLightWidget/ClickSound.mp3";
import { colorsList } from "./constants/ConstData.js";

export default {
  data() {
    return {
      RottetImg: require("@/assets/images/TrafficLightWidget/RottetImg.png"),
      Sun: require("@/assets/images/TimerWidget/Sun.png"),
      Moon: require("@/assets/images/TimerWidget/Moon.png"),
      VolumOn: require("@/assets/images/TimerWidget/VolumOn.png"),
      VolumOff: require("@/assets/images/TimerWidget/VolumOff.png"),
      isEnlarged: false,
      timer: null,
      currentLight: 0,
      ClickSound: ClickSound,
      selectedSound: null,
      IsSoundOn: true,
      IsLightMode: true,
      colorTypes: colorsList,
      rotation: false,
    };
  },

  methods: {
    handleClick() {
      console.log("handleMouseLeave");
      clearTimeout(this.timer);
      this.isEnlarged = false;
    },
    handleMouseLeave() {
      this.timer = setTimeout(() => {
        // if (this.timerData.isTimeAdded && this.completedProgress !== 0) {
        this.isEnlarged = true;
        // }
        // this.isEnlarged = true;
      }, 5000);
    },

    changeColor(lightNumber) {
      this.currentLight = lightNumber;
      if (this.IsSoundOn) {
        const audio = this.$refs.tickAudio;
        audio.currentTime = 0;
        audio.play();
      }
    },

    initAnimation() {
      const container = this.$refs.animationContainer;
      const animation = lottie.loadAnimation({
        container,
        renderer: "svg",
        loop: true,
        autoplay: true,
        animationData,
      });
      this.animation = animation;
    },

    RottetLights() {
      this.rotation = !this.rotation;
    },

    startSound() {
      this.IsSoundOn = true;
    },

    stopSound() {
      this.IsSoundOn = false;
    },

    startLightMode() {
      this.IsLightMode = true;
    },

    stopLightMode() {
      this.IsLightMode = false;
    },
  },
};
</script>

<style scoped>
@import "../assets/style.css";
@import "~bootstrap-icons/font/bootstrap-icons.css";
</style>
