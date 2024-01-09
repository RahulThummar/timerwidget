<template>
  <div
    class="ms-3"
    :style="{
      'background-color': IsLightMode
        ? colorTypes?.LIGHTBACKGROUND
        : colorTypes?.DARKBACKGROUND,
    }"
  >
    <div>
      <div
        :class="[
          { 'traffic-light': !rotation },
          { 'traffic-light-1 ': rotation },
        ]"
      >
        <div
          class="light"
          @click="changeColor(1)"
          :class="{ redactive: currentLight === 1 }"
        ></div>
        <div
          class="light"
          @click="changeColor(2)"
          :class="{ yellowactive: currentLight === 2 }"
        ></div>
        <div
          class="light"
          @click="changeColor(3)"
          :class="{ greenactive: currentLight === 3 }"
        ></div>
      </div>

      <!-- bottom buttons -->

      <div
        :class="{
          'bottom-buttons': !rotation,
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
    </div>
  </div>
  <audio ref="tickAudio" :src="ClickSound"></audio>
</template>

<script>
import lottie from "lottie-web";
import animationData from "../assets/images/celebration.json";
import unFilledtimerImg from "@/assets/images/TrafficLightWidget/unFilledtimerImg.png";
import filledTrafficlightImg from "@/assets/images/TrafficLightWidget/filledTrafficlight.png";
import RottetImg from "@/assets/images/TrafficLightWidget/RottetImg.png";
import VolumOn from "@/assets/images/TimerWidget/VolumOn.png";
import VolumOff from "@/assets/images/TimerWidget/VolumOff.png";
import Sun from "@/assets/images/TimerWidget/Sun.png";
import Moon from "@/assets/images/TimerWidget/Moon.png";
import ClickSound from "@/assets/sound/TrafficLightWidget/ClickSound.mp3";
import { colorsList } from "./constants/ConstData.js";

export default {
  data() {
    return {
      currentLight: 0,
      ClickSound: ClickSound,
      RottetImg: RottetImg,
      unFilledtimerImg: unFilledtimerImg,
      filledTrafficlightImg: filledTrafficlightImg,
      selectedSound: null,
      Sun: Sun,
      Moon: Moon,
      VolumOn: VolumOn,
      VolumOff: VolumOff,
      IsSoundOn: true,
      IsLightMode: true,
      colorTypes: colorsList,
      rotation: false,
    };
  },

  methods: {
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
