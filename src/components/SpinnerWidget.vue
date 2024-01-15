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
        :style="{
          zIndex: !isItemAdded ? '-1' : '1',
          opacity: !isItemAdded ? '0' : '1',
        }"
        class="filled-spinner-main"
      >
        <div class="arrow-img">
          <img :src="arrow" :style="{ width: '55px', height: '40px' }" />
        </div>

        <div :key="num" id="wheelOfFortune">
          <canvas ref="wheelCanvas" width="300" height="300"></canvas>
          <div id="spin" @click="startSpin">
            <div class="start-spin-img">
              <div v-if="!this.isSpinning" class="play-spin-img">
                <img
                  :src="play_icn"
                  :style="{ width: '20px', height: '20px' }"
                />
              </div>
              <img :src="play_bg" :style="{ width: '60px', height: '60px' }" />
            </div>
          </div>
        </div>

        <div
          class="spinner-complition-animation"
          ref="animationContainer"
          v-if="this.animationStarted"
        ></div>

        <div class="winner-name" v-if="this.winnerName !== ''">
          <div class="tag-main">
            <span class="tag-inner">
              {{ this.winnerName }}
              <img
                :style="{ width: '15px', height: '15px' }"
                loading="lazy"
                class="pointer"
                :src="cancelImg"
                @click="handleClickWinTag()"
              />
            </span>
          </div>
        </div>
      </div>

      <div v-if="!isItemAdded" class="empty-spinner-main">
        <div class="empty-circle">
          <div class="col-9">
            <button
              class="btn-common save"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              @click="
                () => {
                  showAddModal = true;
                }
              "
            >
              Create New Spinner
            </button>
          </div>
          <!-- <div class="col-9">
            <button class="btn-common save mt-5" @click="handleSaveSound()">
              Choose Spinner
            </button>
          </div> -->
        </div>
      </div>

      <!-- bottom buttons -->

      <div
        v-if="this.isItemAdded"
        :class="{
          'bottom-buttons': !rotation,
          'bottom-buttons-1': rotation,
        }"
        class="d-flex justify-content-between pe-2"
      >
        <div>
          <img
            data-bs-toggle="modal"
            data-bs-target="#exampleModal"
            :src="editImg"
            class="pointer"
            alt="reset timer"
            :style="{ width: '36px', height: '36px' }"
            @click="handleClickEdit"
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

  <audio ref="tickAudio" :src="celebration_bell"></audio>

  <!-- Modal -->
  <div>
    <div class="modal fade" id="exampleModal" data-bs-backdrop="static">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <div class="form-main">
              <span class="form-inner">
                <div class="name-label">Tag Name</div>
                <span class="input-main">
                  <input v-model="message" placeholder="Enter Tag Name" />

                  <div
                    class="enter-btn-main pointer"
                    @click="handleClickTagEnter"
                  >
                    <img loading="lazy" :src="EnterImg" class="enter-img" />
                  </div>
                </span>

                <div class="tag-main">
                  <span
                    class="tag-inner"
                    v-for="(tagNames, index) in tagList"
                    :key="index"
                  >
                    {{ tagNames.label }}
                    <img
                      :style="{ width: '15px', height: '15px' }"
                      loading="lazy"
                      class="pointer"
                      :src="cancelImg"
                      @click="handleClickTagRemove(index)"
                    />
                  </span>
                </div>
                <hr />
                <div class="cancle-save-btn">
                  <span
                    class="btn-common cancle col-6 me-2 pointer"
                    @click="handleClickCancle"
                    :data-bs-dismiss="dismissAttributeCancel"
                    >Cancel</span
                  >
                  <span
                    class="btn-common save col-6 pointer"
                    @click="handleClickSaveItems"
                    :data-bs-dismiss="dismissAttributeSave"
                    >Save</span
                  >
                </div>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
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
import EnterImg from "@/assets/images/SpinnerWidget/EnterImg.png";
import cancelImg from "@/assets/images/SpinnerWidget/cancelImg.png";
import play_bg from "@/assets/images/SpinnerWidget/play_bg.png";
import celebration_bell from "@/assets/sound/SpinnerWidget/celebration_bell.mp3";
import { colorsList } from "./constants/ConstData.js";
import { ref } from "vue";

let message = ref("");
export default {
  data() {
    return {
      tot: 0,
      elSpin: null,
      ctx: null,
      dia: 0,
      rad: 0,
      PI: Math.PI,
      TAU: 2 * Math.PI,
      arc: 0,
      friction: 0.991,
      angVelMin: 0.009,
      angVelMax: 0,
      angVel: 0,
      ang: 0,
      isSpinning: false,
      isAccelerating: false,
      animFrame: null,
      prizes: [],

      tagList: [],
      message: message,
      canvasVerify: true,
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
      cancelImg: cancelImg,
      play_icn: play_icn,
      VolumOn: VolumOn,
      VolumOff: VolumOff,
      IsSoundOn: true,
      IsLightMode: true,
      colorTypes: colorsList,
      spinnerStart: false,
      isItemAdded: false,
      showAddModal: false,
      showEditModal: false,
      animationStarted: false,
      winnerName: "",
      prevPrizes: [],
      tempTagList: [],
      num: 0,
    };
  },

  computed: {
    isVerifiedCanvas: function () {
      return this.canvasVerify;
    },

    dismissAttributeSave() {
      return this.tagList.length > 1 ? "modal" : null;
    },

    dismissAttributeCancel() {
      return "modal";
    },
  },
  methods: {
    getIndex() {
      return Math.floor(this.tot - (this.ang / this.TAU) * this.tot) % this.tot;
    },
    drawSector(sector, i) {
      const ang = this.arc * i;
      this.ctx.save();
      // COLOR
      this.ctx.beginPath();
      this.ctx.fillStyle = sector.color;
      this.ctx.moveTo(this.rad, this.rad);
      this.ctx.arc(this.rad, this.rad, this.rad, ang, ang + this.arc);
      this.ctx.lineTo(this.rad, this.rad);
      this.ctx.fill();
      // TEXT
      this.ctx.translate(this.rad, this.rad);
      this.ctx.rotate(ang + this.arc / 2);
      this.ctx.textAlign = "right";
      this.ctx.fillStyle = "#fff";
      this.ctx.font = "16px sans-serif";
      // this.ctx.fillText(sector.label, this.rad - 10, 10);
      this.ctx.fillText(sector.label.length > 10 ? sector.label.slice(0, 10) + "..." : sector.label, this.rad - 10, 10);
      //
      this.ctx.restore();
    },
    rotate() {
      // const currentSector = this.prizes[this.getIndex()];
      this.$refs.wheelCanvas.style.transform = `rotate(${
        this.ang - this.PI / 2
      }rad)`;
      // this.elSpin.textContent = !this.angVel ? "SPIN" : currentSector.label;
      // this.elSpin.style.background = currentSector.color;
    },

    frame() {
      if (!this.isSpinning) return;

      if (this.angVel >= this.angVelMax) this.isAccelerating = false;

      // Accelerate
      if (this.isAccelerating) {
        this.angVel ||= this.angVelMin; // Initial velocity kick
        this.angVel *= 2; // Accelerate
      }

      // Decelerate
      else {
        this.isAccelerating = false;
        this.angVel *= this.friction; // Decelerate by friction

        // SPIN END:
        if (this.angVel < this.angVelMin) {
          this.isSpinning = false;
          this.angVel = 0;
          cancelAnimationFrame(this.animFrame);
          const winningSector = this.prizes[this.getIndex()]; 
          this.animationStarted = true;
          if (this.IsSoundOn) {
            this.$refs.tickAudio.play();
          }
          this.spinnerStart = false;
          setTimeout(() => {
            this.initAnimation();
          }, 3);
          this.winnerName = winningSector.label;
        }
      }

      this.ang += this.angVel; // Update angle
      this.ang %= this.TAU; // Normalize angle
      this.rotate(); // CSS rotate!
    },

    engine() {
      this.frame();
      this.animFrame = requestAnimationFrame(this.engine);
    },
    startSpin() {
      if (this.isSpinning) return;
      this.animationStarted = false;
      this.winnerName = "";
      this.spinnerStart = true;
      this.isSpinning = true;
      this.isAccelerating = true;
      this.angVelMax = this.rand(0.25, 0.4);
      this.engine(); // Start engine!
    },

    rand(m, M) {
      return Math.random() * (M - m) + m;
    },

    handleClickTagEnter() {
      let randomColor;
      if (message.value.trim() !== "") {
        for (let i = 0; i < 10; i++) {
          const color = "#" + Math.floor(Math.random() * 16777215).toString(16);
          randomColor = color;
        }
        this.tagList.push({
          label: message.value.trim(),
          color: randomColor,
        });
        message.value = "";
      }
    },

    handleClickSaveItems() {
      if (this.tagList.length < 2) {
        alert("Please add two or more items.");
        return;
      }

      this.handleClickWinTag();

      this.prizes = [...this.tagList];

      this.num += 1;

      this.prevPrizes = this.prizes;
      this.tempTagList = [...this.prizes];
      this.showAddModal = false;
      this.isItemAdded = true;

      setTimeout(() => {
        this.tot = this.prizes.length;
        this.elSpin = document.querySelector("#spin");
        this.ctx = this.$refs.wheelCanvas.getContext("2d");
        this.dia = this.ctx.canvas.width;
        this.rad = this.dia / 2;
        this.arc = this.TAU / this.tot;

        // INIT!
        this.prizes.forEach(this.drawSector);
        this.rotate(); // Initial rotation
      }, 1);
    },

    handleClickTagRemove(index) {
      this.tagList.splice(index, 1);
    },

    handleClickCancle() {
      if (this.showEditModal) {
        message.value = "";
        this.tagList = [...this.tempTagList];
        this.prizes = this.prevPrizes;
        this.showEditModal = false;
      } else {
        message.value = "";
        this.showAddModal = false;
        this.tagList = [];
        this.tempTagList = [];
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

    handleClickWinTag() {
      this.animationStarted = false;
      this.winnerName = "";
    },

    handleClickEdit() {
      this.showEditModal = true;
      this.showAddModal = false;
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
