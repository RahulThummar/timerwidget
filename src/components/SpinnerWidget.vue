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
          :key="num"
          style="width: 300px"
          borderColor="#00000033"
          :borderWidth="6"
          :prizes="prizes"
          :verify="isVerifiedCanvas"
          @rotateStart="onCanvasRotateStart"
          @rotateEnd="onRotateEnd"
        />

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

      <div v-else class="empty-spinner-main">
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
                    {{ tagNames.name }}
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
    if (element) {
      element.classList.add("play-spinner-btn");
    }
  },

  data() {
    return {
      tagList: [],
      prizes: [],
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
      tempTagList:[],
      num:0
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
    },

    handleClickSaveItems() {
      if (this.tagList.length < 2) {
        alert("Please add two or more items.");
        return;
      }

      this.handleClickWinTag();

      const probabilityRatio = parseInt(100 / this.tagList.length);
      this.tagList.map((item) => (item.probability = probabilityRatio));

      this.prizes = [...this.tagList];

      const totalProbability = this.prizes.reduce(
        (sum, { probability }) => sum + probability,
        0
      );
      
      this.num += 1 
      if (totalProbability === 100) {
        this.tempTagList = [...this.prizes]; 
        this.prevPrizes = this.prizes;
        this.showAddModal = false;
        this.isItemAdded = true;
      } else {
        const remainingProbability = 100 - totalProbability;
        this.prizes[0].probability += remainingProbability;
        this.prevPrizes = this.prizes;
        this.tempTagList = [...this.prizes]; 

        this.showAddModal = false;
        this.isItemAdded = true;
      }

      setTimeout(() => {
        const element = document.querySelector(".fw-btn__btn");
        if (element) {
          element.classList.add("play-spinner-btn");
        }
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

    onCanvasRotateStart(rotate) {
      this.animationStarted = false;
      this.winnerName = "";
      this.spinnerStart = true;
      if (this.isVerifiedCanvas) {
        const verified = true;
        this.DoServiceVerify(verified, 100).then((verifiedRes) => {
          if (verifiedRes) {
            rotate();
            this.canvasVerify = false;
          } else {
            alert("Failed verification");
          }
        });
        return;
      }
    },

    onRotateEnd(prize) {
      this.animationStarted = true;
      if (this.IsSoundOn) {
        this.$refs.tickAudio.play();
      }
      this.spinnerStart = false;
      setTimeout(() => {
        this.initAnimation();
      }, 3);
      this.winnerName = prize.name;
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
