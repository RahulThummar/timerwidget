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
      <div class="spinner-main">
        <div class="arrow-img">
          <img :src="arrow" :style="{ width: '30px', height: '30px' }" />
        </div>
        <div class="start-spin-img ">
          <div v-if="!this.spinnerStart" class="play-spin-img">
            <img :src="play_icn" :style="{ width: '20px', height: '20px' }" />
          </div>
          <img :src="play_bg" :style="{ width: '60px', height: '60px' }" />
        </div>
        <FortuneWheel
          style="width: 300px"
          borderColor="#00000033"
          :borderWidth="6"
          :prizes="prizes"
          :verify="isVerifiedCanvas"
          @rotateStart="onCanvasRotateStart"
          @rotateEnd="onRotateEnd"
        />
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
import FortuneWheel from "vue-fortune-wheel";
import "../../node_modules/vue-fortune-wheel/dist/style.css";
import lottie from "lottie-web";
import animationData from "../assets/images/celebration.json";
import unFilledtimerImg from "@/assets/images/TrafficLightWidget/unFilledtimerImg.png";
import filledTrafficlightImg from "@/assets/images/TrafficLightWidget/filledTrafficlight.png";
import RottetImg from "@/assets/images/TrafficLightWidget/RottetImg.png";
import VolumOn from "@/assets/images/TimerWidget/VolumOn.png";
import VolumOff from "@/assets/images/TimerWidget/VolumOff.png";
import Sun from "@/assets/images/TimerWidget/Sun.png";
import Moon from "@/assets/images/TimerWidget/Moon.png";
import arrow from "@/assets/images/SpinnerWidget/arrow.png";
import play_icn from "@/assets/images/SpinnerWidget/play_icn.png";
import play_bg from "@/assets/images/SpinnerWidget/play_bg.png";
import ClickSound from "@/assets/sound/TrafficLightWidget/ClickSound.mp3";
import { colorsList } from "./constants/ConstData.js";

export default {
  components: {
    FortuneWheel,
  },
  mounted() {
    var element = document.getElementsByClassName("fw-btn__btn")[0];
    console.log(element);
    if (element) {
      element.classList.add("play-spinner-btn");
    }
  },
  data() {
    return {
      canvasVerify: true,
      prizes: [
        {
          id: 1,
          name: "1",
          value: "1",
          bgColor: "#45ace9",
          color: "#ffffff",
          probability: 10,
        },
        {
          id: 2,
          name: "2",
          value: "2",
          bgColor: "#dd3832",
          color: "#ffffff",
          probability: 10,
        },
        {
          id: 3,
          name: "3",
          value: "3",
          bgColor: "#fef151",
          color: "#ffffff",
          probability: 10,
        },
        {
          id: 4,
          name: "4",
          value: "4",
          bgColor: "pink",
          color: "blue",
          probability: 10,
        },
        {
          id: 5,
          name: "5",
          value: "5",
          bgColor: "#32a852",
          color: "blue",
          probability: 10,
        },
        {
          id: 6,
          name: "6",
          value: "6",
          bgColor: "#30898c",
          color: "blue",
          probability: 10,
        },
        {
          id: 7,
          name: "7",
          value: "7",
          bgColor: "#05e6ed",
          color: "blue",
          probability: 10,
        },
        {
          id: 8,
          name: "8",
          value: "8",
          bgColor: "#dbb753",
          color: "blue",
          probability: 10,
        },
        {
          id: 9,
          name: "9",
          value: "9",
          bgColor: "#c9c5de",
          color: "blue",
          probability: 10,
        },
        {
          id: 10,
          name: "10",
          value: "10",
          bgColor: "#8348b0",
          color: "blue",
          probability: 10,
        },
      ],

      ClickSound: ClickSound,
      RottetImg: RottetImg,
      unFilledtimerImg: unFilledtimerImg,
      filledTrafficlightImg: filledTrafficlightImg,
      selectedSound: null,
      Sun: Sun,
      Moon: Moon,
      play_bg: play_bg,
      arrow: arrow,
      play_icn: play_icn,
      VolumOn: VolumOn,
      VolumOff: VolumOff,
      IsSoundOn: true,
      IsLightMode: true,
      colorTypes: colorsList,
      spinnerStart: false,
    };
  },

  computed: {
    isVerifiedCanvas: function () {
      return this.canvasVerify;
    },
  },
  methods: {
    handleStartSpinner() {
      console.log("*************");
      //   this.spinnerStart = true;
      //   document.getElementsByClassName("fw-btn").click();
    },
    onImageRotateStart() {
      console.log("onRotateStart");
    },
    onCanvasRotateStart(rotate) {
      this.spinnerStart = true;
      console.log(rotate, "l");
      console.log(this.isVerifiedCanvas);
      if (this.isVerifiedCanvas) {
        const verified = true;
        this.DoServiceVerify(verified, 100).then((verifiedRes) => {
          if (verifiedRes) {
            console.log("Verification passed, start to rotate");
            rotate();
            this.canvasVerify = false;
          } else {
            alert("Failed verification");
          }
        });
        return;
      }

      console.log("onCanvasRotateStart");
    },
    onRotateEnd(prize) {
      this.spinnerStart = false;
      alert(prize.value);
    },

    DoServiceVerify(verified, duration) {
      return new Promise((resove) => {
        setTimeout(() => {
          resove(verified);
        }, duration);
      });
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
