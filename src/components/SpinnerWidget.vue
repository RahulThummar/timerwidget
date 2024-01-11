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
      <div v-if="this.isItemAdded" class="filled-spinner-main">
        <div class="arrow-img">
          <img :src="arrow" :style="{ width: '55px', height: '40px' }" />
        </div>
        <div class="start-spin-img">
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
      <div v-else class="empty-spinner-main">
        <div class="empty-circle">
          <div class="col-9">
            <button
              class="btn-common save"
              @click="
                () => {
                  showAddModal = true;
                }
              "
            >
              Create New Spinner
            </button>
          </div>
          <div class="col-9">
            <button class="btn-common save mt-5" @click="handleSaveSound()">
              Choose Spinner
            </button>
          </div>
        </div>
      </div>

      <!-- bottom buttons -->

      <!-- <div
        :class="{
          'bottom-buttons': !rotation,
          'bottom-buttons-1': rotation,
        }"
        class="d-flex justify-content-between pe-2"
      >
        <div>
          <img
            :src="editImg"
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
      </div> -->
    </div>
  </div>

  <audio ref="tickAudio" :src="celebration_bell"></audio>

  <div class="form-main mt-5" v-if="this.showAddModal">
    <span class="form-inner">
      <div class="name-label">Tag Name</div>
      <span class="input-main">
        <input v-model="message" placeholder="Enter Tag Name" />
        <div class="enter-btn-main" @click="handleClickTagEnter">
          <img loading="lazy" :src="EnterImg" class="enter-img pointer" />
        </div>
      </span>

      <div class="tag-main">
        <span
          class="tag-inner"
          v-for="(tagNames, index) in tagList"
          :key="index"
        >
          {{ tagNames.name }}
          <img
            loading="lazy"
            class="pointer"
            :src="cancelImg"
            @click="handleClickTagRemove(index)"
          />
        </span>
      </div>

      <div class="cancle-save-btn">
        <span
          class="btn-common cancle col-6 me-2 pointer"
          @click="handleClickCancle"
          >Cancel</span
        >
        <span
          class="btn-common save col-6 pointer"
          @click="handleClickSaveItems"
          >Save</span
        >
      </div>
    </span>
  </div>
</template>

<script>
import FortuneWheel from "vue-fortune-wheel";
import "../../node_modules/vue-fortune-wheel/dist/style.css";
import lottie from "lottie-web";
import animationData from "../assets/images/celebration.json";
import unFilledtimerImg from "@/assets/images/TrafficLightWidget/unFilledtimerImg.png";
import filledTrafficlightImg from "@/assets/images/TrafficLightWidget/filledTrafficlight.png";
import VolumOn from "@/assets/images/TimerWidget/VolumOn.png";
import VolumOff from "@/assets/images/TimerWidget/VolumOff.png";
import Sun from "@/assets/images/TimerWidget/Sun.png";
import Moon from "@/assets/images/TimerWidget/Moon.png";
import arrow from "@/assets/images/SpinnerWidget/arrow.png";
import editImg from "@/assets/images/SpinnerWidget/editImg.png";
import play_icn from "@/assets/images/SpinnerWidget/play_icn.png";
import Rectangle from "@/assets/images/SpinnerWidget/Rectangle.png";
import EnterImg from "@/assets/images/SpinnerWidget/EnterImg.png";
import cancelImg from "@/assets/images/SpinnerWidget/cancelImg.png";
import play_bg from "@/assets/images/SpinnerWidget/play_bg.png";
import celebration_bell from "@/assets/sound/SpinnerWidget/celebration_bell.mp3";
import { colorsList } from "./constants/ConstData.js";
import { ref } from "vue";

let message = ref("");
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
    console.log(this.tagText);
    return {
      tagList: [],
      message: message,
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
      celebration_bell: celebration_bell,
      editImg: editImg,
      unFilledtimerImg: unFilledtimerImg,
      filledTrafficlightImg: filledTrafficlightImg,
      selectedSound: null,
      Sun: Sun,
      Moon: Moon,
      play_bg: play_bg,
      arrow: arrow,
      EnterImg: EnterImg,
      Rectangle: Rectangle,
      cancelImg: cancelImg,
      play_icn: play_icn,
      VolumOn: VolumOn,
      VolumOff: VolumOff,
      IsSoundOn: true,
      IsLightMode: true,
      colorTypes: colorsList,
      spinnerStart: false,
      isItemAdded: true,
      showAddModal: false,
    };
  },

  computed: {
    isVerifiedCanvas: function () {
      return this.canvasVerify;
    },
  },
  methods: {
    handleClickTagEnter() {
      let randomColor;
      if (message.value.trim() !== "") {
        for (let i = 0; i < 10; i++) {
          const color = "#" + Math.floor(Math.random() * 16777215).toString(16);
          randomColor = color;
        }
        this.tagList.push({
          id: this.tagList.length + 1,
          name: message.value.trim(),
          bgColor: randomColor,
          color: "#fff",
          probability: 0,
        });
        message.value = "";
      }
      console.log(this.tagList);
    },

    handleClickSaveItems() {
      console.log(this.tagList.length, "length");
      if (this.tagList.length < 2) {
        alert("Please add two or more items.");
        return;
      }

      let probabilityRatio = 100 / this.tagList.length;
      console.log(probabilityRatio);
      this.tagList.map((a) => (a.probability = probabilityRatio));
      this.prizes = this.tagList;

      let arr = this.prizes;

      if (
        this.prizes.reduce((n, { probability }) => n + probability, 0) !== 100
      ) {
        let temp =
          100 - this.prizes.reduce((n, { probability }) => n + probability, 0);
        arr[0].probability = arr[0].probability + temp;
      }
      this.prizes = arr;

      if (
        this.prizes.reduce((n, { probability }) => n + probability, 0) === 100
      ) {
        console.log(this.prizes.length, "5555555588888");
        console.log(this.prizes, "55555555");
        this.showAddModal = false;
        this.isItemAdded = true;
      }
      setTimeout(() => {
        var element = document.getElementsByClassName("fw-btn__btn")[0];
        console.log(element);
        if (element) {
          element.classList.add("play-spinner-btn");
        }
      }, 1);
    },

    handleClickTagRemove(index) {
      this.tagList.splice(index, 1);
    },

    handleClickCancle() {
      this.tagList = [];
      this.prizes = [];
      this.showAddModal = false;
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
      if (this.IsSoundOn) {
        this.$refs.tickAudio.play();
      }
      this.spinnerStart = false;
      alert(prize.name);
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

<!-- id: 1,
name: "1",
value: "1",
bgColor: "#45ace9",
color: "#ffffff",
probability: 10, -->
