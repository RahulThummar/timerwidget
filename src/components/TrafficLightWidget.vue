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
      <!-- <div class="top-menu">
        <img
          :src="Group244"
          alt="Group244"
          :style="{ width: '32px', height: '32px' }"
        />
        <img
          :src="unFilledtimerImg"
          alt="Group244"
          :style="{ width: '32px', height: '32px' }"
        />
        <img
          :src="Group4"
          alt="Group244"
          :style="{ width: '32px', height: '32px' }"
        />
        <img
          :src="filledTrafficlightImg"
          alt="Group244"
          :style="{ width: '32px', height: '32px' }"
        />
      </div> -->

      <div class="content">
        <div class="p-4">
          <div
            :class="[
              { 'traffic-light-without-rotation': !rotation },
              { 'traffic-light-with-rotation ': rotation },
            ]"
          >
            <input
              type="radio"
              name="traffic-light-color"
              class="red-input"
              value="color1"
              @click="handleClickRedLight()"
              :style="{
                backgroundColor: this.redLightOn
                  ? colorTypes?.REDLIGHTON
                  : colorTypes?.REDLIGHTOFF,
              }"
            />
            <input
              type="radio"
              name="traffic-light-color"
              class="yellow-input"
              value="color2"
              @click="handleClickYellowLight()"
              :style="{
                backgroundColor: this.yellowLightOn
                  ? colorTypes?.YELLOWLIGHTON
                  : colorTypes?.YELLOWLIGHTOFF,
              }"
            />
            <input
              type="radio"
              name="traffic-light-color"
              class="green-input"
              value="color3"
              @click="handleClickGreenLight()"
              :style="{
                backgroundColor: this.greenLightOn
                  ? colorTypes?.GREENLIGHTON
                  : colorTypes?.GREENLIGHTOFF,
              }"
            />
          </div>
        </div>
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
import Group244 from "@/assets/images/TimerWidget/Group244.png";
import unFilledtimerImg from "@/assets/images/TrafficLightWidget/unFilledtimerImg.png";
import Group4 from "@/assets/images/TimerWidget/Group4.png";
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
      ClickSound: ClickSound,
      RottetImg: RottetImg,
      Group244: Group244,
      unFilledtimerImg: unFilledtimerImg,
      Group4: Group4,
      filledTrafficlightImg: filledTrafficlightImg,
      selectedSound: null,
      Sun: Sun,
      Moon: Moon,
      VolumOn: VolumOn,
      VolumOff: VolumOff,
      IsSoundOn: true,
      IsLightMode: true,
      tempSound: "beep",
      colorTypes: colorsList,
      redLightOn: false,
      yellowLightOn: false,
      greenLightOn: false,
      rotation: false,
    };
  },

  methods: {
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
    handleClickRedLight() {
      if (this.IsSoundOn) {
        this.$refs.tickAudio.play();
      }
      this.redLightOn = true;
      this.yellowLightOn = false;
      this.greenLightOn = false;
    },

    handleClickYellowLight() {
      if (this.IsSoundOn) {
        this.$refs.tickAudio.play();
      }
      this.yellowLightOn = true;
      this.redLightOn = false;
      this.greenLightOn = false;
    },

    handleClickGreenLight() {
      if (this.IsSoundOn) {
        this.$refs.tickAudio.play();
      }
      this.greenLightOn = true;
      this.redLightOn = false;
      this.yellowLightOn = false;
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
