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
          <img :src="arrow" :style="{ width: '42px', height: '29px' }" />
        </div>

        <div class="outer-border">
          <div :key="num" id="wheelOfFortune">
            <canvas ref="wheelCanvas" width="300" height="300"></canvas>
            <div id="spin">
              <div class="start-spin-img" @click="startSpin">
                <div v-if="!this.isSpinning" class="play-spin-img">
                  <img
                    :src="play_icn"
                    :style="{ width: '20px', height: '20px' }"
                  />
                </div>
                <img
                  :src="play_bg"
                  :style="{ width: '60px', height: '60px' }"
                />
              </div>
              <div class="winner-name" v-if="this.winnerName !== ''">
                <div class="winner-main">
                  <span class="winner-inner">
                    <div class="winner-tag">
                      {{ this.winnerName }}
                    </div>
                    <img
                      :style="{ width: '15px', height: '15px' }"
                      loading="lazy"
                      class="pointer m-1"
                      :src="cancelImg"
                      @click="handleClickWinTag()"
                    />
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div
          class="spinner-complition-animation"
          ref="animationContainer"
          v-if="this.animationStarted"
        ></div>

        <!-- <div class="winner-name" v-if="this.winnerName !== ''">
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
        </div> -->
      </div>

      <div v-if="!isItemAdded" class="empty-spinner-main">
        <div
          class="empty-circle"
          :style="{
            paddingTop: this.chooseSpinner ? '50px' : null,
            justifyContent: this.chooseSpinner ? null : 'center',
            backgroundColor: this.chooseSpinner ? 'rgb(195 195 195)' : null,
          }"
        >
          <div v-if="!this.chooseSpinner" class="empty-inner">
            <div
              class="pointer"
              :style="{ display: 'flex' }"
              data-bs-toggle="modal"
              data-bs-target="#addEditTagModal"
              @click="
                () => {
                  showAddModal = true;
                }
              "
            >
              <div>
                <img
                  :src="CreteSpinner"
                  alt="CreteSpinner"
                  :style="{ width: '25px', height: '25px' }"
                />
              </div>
              <div class="ms-2">Create New Spinner</div>
            </div>

            <div
              v-if="!this.chooseSpinner"
              class="mt-3 pointer"
              @click="handleClickChooseSpinner"
            >
              <div :style="{ display: 'flex' }">
                <div>
                  <img
                    :src="ChooseSpinner"
                    alt="ChooseSpinner"
                    :style="{ width: '25px', height: '25px' }"
                  />
                </div>
                <div class="ms-2">Choose Spinner</div>
              </div>
            </div>
          </div>

          <div v-if="this.chooseSpinner" class="col-9">
            <select class="spinner-list-options" @change="handleSpinnerChange">
              <option value="" hidden>Choose Spinner</option>

              <option
                v-for="(spinner, index) in getSpinnersList()"
                :value="spinner.spinnerName"
                :key="index"
              >
                {{ spinner.spinnerName }}
              </option>
            </select>
          </div>
        </div>
      </div>

      <!-- bottom buttons -->

      <div
        class="p-2"
        :class="
          this.isItemAdded
            ? 'bottom-buttons-spinner'
            : 'bottom-buttons-empty-spinner'
        "
      >
        <div>
          <img
            :src="ResetImg"
            class="pointer"
            alt="reset"
            :style="{ width: '36px', height: '36px' }"
            @click="handleClickReset"
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
        <div v-if="this.isItemAdded">
          <img
            data-bs-toggle="modal"
            data-bs-target="#addEditTagModal"
            :src="editImg"
            class="pointer"
            alt="edit tags"
            :style="{ width: '36px', height: '36px' }"
            @click="handleClickEdit"
          />
        </div>

        <div v-if="this.isItemAdded">
          <img
            :src="SaveImg"
            class="pointer"
            alt="SaveImg"
            :data-bs-toggle="this.selectedSpinner ? null : 'modal'"
            data-bs-target="#saveSpinnerModal"
            :style="{ width: '36px', height: '36px' }"
            @click="handleClickSave"
          />
        </div>

        <div v-if="this.isItemAdded && this.selectedSpinner">
          <img
            :src="SaveAsImg"
            class="pointer"
            alt="SaveAsImg"
            data-bs-toggle="modal"
            data-bs-target="#saveSpinnerModal"
            :style="{ width: '36px', height: '36px' }"
          />
        </div>
        <div v-if="this.isItemAdded && this.selectedSpinner">
          <img
            :src="deleteImg"
            class="pointer"
            alt="deleteImg"
            data-bs-toggle="modal"
            data-bs-target="#deleteSpinnerModal"
            :style="{ width: '36px', height: '36px' }"
          />
        </div>
      </div>
    </div>
  </div>

  <audio ref="tickAudio" :src="celebration_bell"></audio>

  <!-- Add edit tag Modal -->

  <div class="modal fade" id="addEditTagModal" data-bs-backdrop="static">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-body">
          <div class="form-main">
            <span class="form-inner">
              <div class="name-label">Tag Name</div>
              <span class="input-main">
                <input
                  v-model="message"
                  placeholder="Enter Tag Name..."
                  @keydown.enter="handleClickTagEnter"
                />

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
                  :data-bs-dismiss="dismissAttributeSaveTags"
                  >Save</span
                >
              </div>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Save Spinner Modal -->

  <div class="modal fade" id="saveSpinnerModal" data-bs-backdrop="static">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-body">
          <div class="form-main">
            <span class="form-inner">
              <div class="save-header pb-2">
                {{ this.selectedSpinner ? "Save as" : "Save spinner list" }}
              </div>
              <span class="input-main">
                <input v-model="spinnerName" placeholder="Enter spinner name" />
              </span>

              <hr />
              <div class="cancle-save-btn">
                <span
                  class="btn-common cancle col-6 me-2 pointer"
                  @click="handleCancleSpinnerSave"
                  :data-bs-dismiss="dismissAttributeCancel"
                  >Cancel</span
                >
                <span
                  class="btn-common save col-6 pointer"
                  @click="handleClickSaveSpinner('new')"
                  :data-bs-dismiss="dismissAttributeSaveSpinner"
                  >Save</span
                >
              </div>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Delete Spinner Modal -->

  <div class="modal fade" id="deleteSpinnerModal" data-bs-backdrop="static">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-body">
          <div class="form-main">
            <span class="form-inner">
              <div class="save-header pb-2">
                <div>
                  <img
                    :src="deleteSvg"
                    data-bs-target="#saveSpinnerModal"
                    :style="{ width: '36px', height: '36px' }"
                  />
                </div>
                Are you sure you want to delete <br />"{{
                  selectedSpinner?.spinnerName
                }}" spinner?
              </div>
              <div class="cancle-save-btn">
                <span
                  class="btn-common cancle col-6 me-2 pointer"
                  @click="handleCancleSpinnerSave"
                  :data-bs-dismiss="dismissAttributeCancel"
                  >Cancel</span
                >
                <span
                  class="btn-common delete col-6 pointer"
                  @click="handleClickDeleteSpinner"
                  data-bs-dismiss="modal"
                  >Delete</span
                >
              </div>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import lottie from "lottie-web";
import animationData from "../assets/images/celebration.json";
import celebration_bell from "@/assets/sound/SpinnerWidget/celebration_bell.mp3";
import { colorsList } from "./constants/ConstData.js";
import { ref } from "vue";

let message = ref("");
let spinnerName = ref("");

export default {
  data() {
    return {
      editImg: require("@/assets/images/SpinnerWidget/editImg.png"),
      Sun: require("@/assets/images/TimerWidget/Sun.png"),
      Moon: require("@/assets/images/TimerWidget/Moon.png"),
      VolumOn: require("@/assets/images/TimerWidget/VolumOn.png"),
      VolumOff: require("@/assets/images/TimerWidget/VolumOff.png"),
      ChooseSpinner: require("@/assets/images/SpinnerWidget/ChooseSpinner.svg"),
      CreteSpinner: require("@/assets/images/SpinnerWidget/CreteSpinner.svg"),
      deleteSvg: require("@/assets/images/SpinnerWidget/deleteSvg.svg"),
      play_bg: require("@/assets/images/SpinnerWidget/play_bg.png"),
      ResetImg: require("@/assets/images/SpinnerWidget/ResetImg.png"),
      SaveImg: require("@/assets/images/SpinnerWidget/SaveImg.png"),
      deleteImg: require("@/assets/images/SpinnerWidget/deleteImg.png"),
      SaveAsImg: require("@/assets/images/SpinnerWidget/SaveAsImg.png"),
      arrow: require("@/assets/images/SpinnerWidget/arrow.png"),
      EnterImg: require("@/assets/images/SpinnerWidget/EnterImg.png"),
      cancelImg: require("@/assets/images/SpinnerWidget/cancelImg.png"),
      play_icn: require("@/assets/images/SpinnerWidget/play_icn.png"),

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
      spinnerName: spinnerName,
      canvasVerify: true,
      celebration_bell: celebration_bell,

      selectedSound: null,
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
      chooseSpinner: false,
      selectedSpinner: null,
    };
  },

  computed: {
    dismissAttributeSaveTags() {
      return this.tagList.length > 1 ? "modal" : null;
    },

    dismissAttributeCancel() {
      return "modal";
    },
    dismissAttributeSaveSpinner() {
      const isDuplicateSpinnerName = this.getSpinnersList().some(
        (spinner) => spinner.spinnerName === spinnerName.value.trim()
      );
      return spinnerName.value.trim() !== "" && !isDuplicateSpinnerName
        ? "modal"
        : null;
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
      const textMargin = 20; // Adjust this margin as needed
      const textRadius = this.rad - textMargin;
      const textX = textRadius * Math.cos(ang + this.arc / 2);
      const textY = textRadius * Math.sin(ang + this.arc / 2);

      this.ctx.translate(this.rad + textX, this.rad + textY);
      this.ctx.rotate(ang + this.arc / 2);
      this.ctx.textAlign = "right";
      this.ctx.fillStyle = "#fff";
      this.ctx.font = "14px sans-serif";

      const maxTextWidth = 80; // Maximum width for the text
      const label = sector.label;

      let truncatedLabel = label;
      let labelWidth = this.ctx.measureText(label).width;

      while (labelWidth > maxTextWidth && truncatedLabel.length > 0) {
        truncatedLabel = truncatedLabel.slice(0, -1);
        labelWidth = this.ctx.measureText(truncatedLabel + "...").width;
      }

      if (truncatedLabel.length < label.length) {
        truncatedLabel += "...";
      }

      // this.ctx.fillText(truncatedLabel, 10, 0);

      this.ctx.fillText(
        truncatedLabel,
        truncatedLabel.length < label.length ? 10 : -20,
        0
      );

      this.ctx.restore();
    },

    rotate() {
      this.$refs.wheelCanvas.style.transform = `rotate(${
        this.ang - this.PI / 2
      }rad)`;
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
      // this.angVelMax = this.rand(15, 4);

      this.angVelMax = this.rand(
        Math.floor(Math.random() * 50) + 1,
        Math.floor(Math.random() * 5) + 1
      );
      this.engine(); // Start engine!
    },

    //     generateRandomNumber() {
    //     const randomInteger = Math.floor(Math.random() * 10) + 1;

    //     return randomInteger;
    // },

    rand(m, M) {
      return Math.random() * (M - m) + m;
    },

    generateRandomColor(existingColors) {
      let randomColor;
      for (let i = 0; i < 10; i++) {
        const darkColor = {
          r: Math.floor(Math.random() * 128),
          g: Math.floor(Math.random() * 128),
          b: Math.floor(Math.random() * 128),
        };

        const color = `rgb(${darkColor.r},${darkColor.g},${darkColor.b})`;

        if (!existingColors.has(color)) {
          randomColor = color;
          break;
        }
      }
      return randomColor;
    },

    handleClickTagEnter() {
      if (message.value.trim() !== "") {
        const isDuplicateLabel = this.tagList.some(
          (tag) => tag.label === message.value.trim()
        );

        if (isDuplicateLabel) {
          alert("You can not able to add same tag names... !!!");
          return;
        }

        const existingColors = new Set(this.tagList.map((tag) => tag.color));

        const randomColor = this.generateRandomColor(existingColors);

        this.tagList.push({
          label: message.value.trim(),
          color: randomColor,
        });

        message.value = "";
      }
    },

    handleClickSaveItems() {
      if (this.tagList.length < 2) {
        alert("Create at least two tags...");
        return;
      }

      this.handleClickWinTag();

      this.prizes = [...this.tagList];

      this.num += 1;

      this.prevPrizes = this.prizes;
      this.tempTagList = [...this.prizes];
      this.showAddModal = false;
      this.isItemAdded = true;
      message.value = "";

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

    handleCancleSpinnerSave() {
      spinnerName.value = "";
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

    handleClickReset() {
      this.selectedSpinner = null;
      this.chooseSpinner = false;
      this.isItemAdded = false;
      this.tagList = [];
      this.prizes = [];
      this.prevPrizes = [];
      this.tempTagList = [];
      this.showEditModal = false;
      this.showAddModal = false;
      this.animationStarted = false;
      this.winnerName = "";
      this.isAccelerating = false;
      this.isSpinning = false;
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

    getSpinnersList() {
      const storedSpinners =
        JSON.parse(localStorage.getItem("spinnersList")) || [];
      return storedSpinners;
    },

    handleSpinnerChange(event) {
      const selectedValue = event.target.value;

      this.selectedSpinner = this.getSpinnersList().find(
        (spinner) => spinner.spinnerName === selectedValue
      );

      for (let i = 0; i < this.selectedSpinner.prizes.length; i++) {
        const existingColors = new Set(
          this.selectedSpinner.prizes.map((item) => item.color)
        );

        this.selectedSpinner.prizes[i].color =
          this.generateRandomColor(existingColors);
      }

      if (this.selectedSpinner) {
        this.prizes = [...this.selectedSpinner.prizes];
        this.prevPrizes = [...this.selectedSpinner.prizes];
        this.tagList = [...this.selectedSpinner.prizes];
        this.tempTagList = [...this.selectedSpinner.prizes];

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
      }
    },

    handleClickChooseSpinner() {
      if (this.getSpinnersList().length === 0) {
        alert("Please create at least one spinner for choose spinner.");
        return;
      }
      this.chooseSpinner = true;
    },

    handleClickSave() {
      if (this.selectedSpinner) {
        alert("Spinner data saved !!!.");
        this.handleClickSaveSpinner("old");
        return;
      }
    },

    handleClickSaveSpinner(type) {
      if (type === "old") {
        //save existing spinner

        let spinnersList = this.getSpinnersList();

        const updatedSpinnersList = spinnersList.map((spinner) => {
          if (spinner.spinnerName === this.selectedSpinner.spinnerName) {
            return {
              ...spinner,
              spinnerName: this.selectedSpinner.spinnerName,
              prizes: this.prizes,
            };
          } else {
            return spinner;
          }
        });

        localStorage.setItem(
          "spinnersList",
          JSON.stringify(updatedSpinnersList)
        );
        this.selectedSpinner = {
          spinnerName: this.selectedSpinner.spinnerName,
          prizes: this.prizes,
        };
      } else {
        //save and saveas
        if (spinnerName.value.trim() === "") {
          alert("Please add spinner name for save spinner.");
          return;
        }

        const isDuplicateSpinnerName = this.getSpinnersList().some(
          (spinner) => spinner.spinnerName === spinnerName.value.trim()
        );

        if (isDuplicateSpinnerName) {
          alert("This named spinner is already exists.");
          return;
        }

        this.prizes.forEach((object) => {
          delete object["color"];
        });

        let saveSpinner = {
          spinnerName: spinnerName.value.trim(),
          prizes: this.prizes,
        };

        let spinnersList = this.getSpinnersList();
        spinnersList.push(saveSpinner);
        localStorage.setItem("spinnersList", JSON.stringify(spinnersList));
        this.selectedSpinner = saveSpinner;

        spinnerName.value = "";
      }
    },

    handleClickDeleteSpinner() {
      let spinnersList = this.getSpinnersList();

      let remeiningSpinnersList = spinnersList.filter(
        (a) => a.spinnerName !== this.selectedSpinner.spinnerName
      );

      localStorage.setItem(
        "spinnersList",
        JSON.stringify(remeiningSpinnersList)
      );
      this.handleClickReset();
    },
  },
};
</script>

<style scoped>
@import "../assets/style.css";
@import "~bootstrap-icons/font/bootstrap-icons.css";
</style>
