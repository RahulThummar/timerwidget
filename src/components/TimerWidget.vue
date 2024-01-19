<template>
  <div
    class="inner-main"
    :style="{
      'background-color': IsLightMode
        ? colorTypes?.LIGHTBACKGROUND
        : colorTypes?.DARKBACKGROUND,
    }"
  >
    <div v-if="this.musicSelection.IsSelectMusicIcn">
      <div class="p-3">
        <div class="heade"></div>
        <div class="bodyy">
          <div class="align-middle">
            <button
              class="btn-common track w-100"
              @click="handleClickSelectTrackBtn()"
            >
              Select Background Track
            </button>
          </div>
        </div>
        <div class="footer">
          <div>
            <button
              class="btn-common cancle w-100"
              @click="handleClickCancleBtn()"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="this.musicSelection.Is_SelectMusic">
      <div class="">
        <div class="heade p-3">
          <div
            class="d-flex justify-content-between visually-hidden align-items-center px-4"
          >
            <img
              :src="BigHourGlass"
              alt="BigHourGlass"
              :style="{ width: '32px', height: '32px' }"
            />
            <img
              :src="CircleTimer"
              alt="CircleTimer"
              :style="{ width: '32px', height: '32px' }"
            />
            <img
              :src="WatchTimer"
              alt="WatchTimer"
              :style="{ width: '32px', height: '32px' }"
            />
          </div>
        </div>
        <div class="bodyy mb-3">
          <div
            class="d-flex justify-content-between align-items-center border-bottom border-2 py-3 px-3"
          >
            <div class="d-flex gap-3">
              <img
                :src="musical"
                alt="musical"
                :style="{ width: '22px', height: '22px' }"
              />
              <h6>Beep</h6>
            </div>
            <input
              value="beep"
              class="form-check-input mt-0 pointer"
              type="radio"
              name="musicTypes"
              id="beep"
              :checked="tempSound === 'beep'"
              @click="
                () => {
                  tempSound = 'beep';
                }
              "
            />
          </div>
          <div
            class="d-flex justify-content-between align-items-center border-bottom border-2 py-3 px-3"
          >
            <div class="d-flex gap-3">
              <img
                :src="musical"
                alt="musical"
                :style="{ width: '22px', height: '22px' }"
              />
              <h6>Bubble</h6>
            </div>
            <input
              value="bubble"
              class="form-check-input mt-0 pointer"
              type="radio"
              name="musicTypes"
              id="bubble"
              :checked="tempSound === 'bubble'"
              @click="
                () => {
                  tempSound = 'bubble';
                }
              "
            />
          </div>
          <div
            class="d-flex justify-content-between align-items-center border-bottom border-2 py-3 px-3"
          >
            <div class="d-flex gap-3">
              <img
                :src="musical"
                alt="musical"
                :style="{ width: '22px', height: '22px' }"
              />
              <h6>Buzzer</h6>
            </div>
            <input
              value="buzzer"
              class="form-check-input mt-0 pointer"
              type="radio"
              name="musicTypes"
              id="buzzer"
              :checked="tempSound === 'buzzer'"
              @click="
                () => {
                  tempSound = 'buzzer';
                }
              "
            />
          </div>
          <div
            class="d-flex justify-content-between align-items-center border-bottom border-2 py-3 px-3"
          >
            <div class="d-flex gap-3">
              <img
                :src="musical"
                alt="musical"
                :style="{ width: '22px', height: '22px' }"
              />
              <h6>Smooth</h6>
            </div>
            <input
              value="smooth"
              class="form-check-input mt-0 pointer"
              type="radio"
              name="musicTypes"
              id="smooth"
              :checked="tempSound === 'smooth'"
              @click="
                () => {
                  tempSound = 'smooth';
                }
              "
            />
          </div>
          <div
            class="d-flex justify-content-between align-items-center border-bottom border-2 py-3 px-3"
          >
            <div class="d-flex gap-3">
              <img
                :src="musical"
                alt="musical"
                :style="{ width: '22px', height: '22px' }"
              />
              <h6>Sweet End</h6>
            </div>
            <input
              value="sweet_end"
              class="form-check-input mt-0 pointer"
              type="radio"
              name="musicTypes"
              id="sweet_end"
              :checked="tempSound === 'sweet_end'"
              @click="
                () => {
                  tempSound = 'sweet_end';
                }
              "
            />
          </div>
          <div
            class="d-flex justify-content-between align-items-center border-bottom border-2 py-3 px-3"
          >
            <div class="d-flex gap-3">
              <img
                :src="musical"
                alt="musical"
                :style="{ width: '22px', height: '22px' }"
              />
              <h6>Times Up</h6>
            </div>
            <input
              value="times_up"
              class="form-check-input mt-0 pointer"
              type="radio"
              name="musicTypes"
              id="times_up"
              :checked="tempSound === 'times_up'"
              @click="
                () => {
                  tempSound = 'times_up';
                }
              "
            />
          </div>
        </div>
        <div class="footer px-3">
          <div class="d-flex gap-2">
            <button
              class="btn-common cancle w-100"
              @click="handleClickCancleBtn()"
            >
              Cancel
            </button>
            <button class="btn-common save w-100" @click="handleSaveSound()">
              Save
            </button>
          </div>
        </div>
      </div>
    </div>

    <div
      v-if="
        !this.musicSelection.IsSelectMusicIcn &&
        !this.musicSelection.Is_SelectMusic
      "
    >
      <!-- header buttons -->
      <div
        v-if="this.completedProgress !== 0"
        class="timer-types p-1 d-flex justify-content-between"
      >
        <div
          class="timer-type-box"
          :style="{
            backgroundColor:
              timerData.selectedTimer === timerTypes?.HOURGLASS
                ? colorTypes?.SELECTEDTIMERCOLOR
                : colorTypes?.UNSELECTEDTIMERCOLOR,
          }"
          @click="selectTimer(timerTypes?.HOURGLASS)"
        >
          <img
            :src="BigHourGlass"
            alt="Hour Glass"
            :style="{ width: '32px', height: '32px' }"
          />
        </div>
        <div
          class="timer-type-box"
          :style="{
            backgroundColor:
              timerData.selectedTimer === timerTypes?.CIRCLETIMER
                ? colorTypes?.SELECTEDTIMERCOLOR
                : colorTypes?.UNSELECTEDTIMERCOLOR,
          }"
          @click="selectTimer(timerTypes?.CIRCLETIMER)"
        >
          <img
            :src="CircleTimer"
            alt="Circle Timer"
            :style="{ width: '32px', height: '32px' }"
          />
        </div>
        <div
          class="timer-type-box"
          :style="{
            backgroundColor:
              timerData.selectedTimer === timerTypes?.WATCHTIMER
                ? colorTypes?.SELECTEDTIMERCOLOR
                : colorTypes?.UNSELECTEDTIMERCOLOR,
          }"
          @click="selectTimer(timerTypes?.WATCHTIMER)"
        >
          <img
            :src="WatchTimer"
            alt="Watch Timer"
            :style="{ width: '32px', height: '32px' }"
          />
        </div>
      </div>

      <div v-else class="completed-txt">
        <h5>Completed</h5>
      </div>
      <div class="content">
        <img
          v-if="
            timerData.selectedTimer === timerTypes?.WATCHTIMER &&
            this.timerData.isTimeAdded
          "
          :src="BigWatchTimer"
          alt="Selected Timer"
          class="circle-progress"
          :style="{ width: '170px', height: '170px' }"
        />

        <div
          v-else-if="
            timerData.selectedTimer === timerTypes?.CIRCLETIMER &&
            this.timerData.isTimeAdded
          "
          class="circle-progress"
        >
          <circle-progress
            :percent="calculateProgress()"
            :stroke-width="17"
            :inner-stroke-width="17"
            :size="170"
            :empty-color="colorTypes?.EMPTYPROGRESSBARCOLOR"
            :stroke-color="colorTypes?.FILLPROGRESSBARCOLOR"
            :linecap="'square'"
          ></circle-progress>
        </div>

        <div
          v-else-if="!this.timerData.isTimeAdded"
          class="d-flex inner-content p-4"
        >
          <div class="d-flex">
            <div>
              <div>
                <i
                  class="bi bi-caret-up-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="incrementFirstDigit('minutes')"
                ></i>
              </div>

              <div
                class="m-1"
                :class="{
                  'count-light count-size': !IsLightMode,
                  'count-dark count-size': IsLightMode,
                }"
              >
                {{ firstDigitMinutes }}
              </div>

              <div>
                <i
                  class="bi bi-caret-down-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="decrementFirstDigit('minutes')"
                ></i>
              </div>
            </div>

            <div class="ms-2">
              <div>
                <i
                  class="bi bi-caret-up-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="incrementSecondDigit('minutes')"
                ></i>
              </div>
              <div
                class="m-1"
                :class="{
                  'count-light count-size': !IsLightMode,
                  'count-dark count-size': IsLightMode,
                }"
              >
                {{ secondDigitMinutes }}
              </div>
              <div>
                <i
                  class="bi bi-caret-down-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="decrementSecondDigit('minutes')"
                ></i>
              </div>
            </div>
          </div>

          <div
            class="m-1"
            :class="{
              'count-light count-size ms-2': !IsLightMode,
              'count-dark count-size ms-2': IsLightMode,
            }"
          >
            :
          </div>

          <div class="d-flex ms-2">
            <div>
              <div>
                <i
                  class="bi bi-caret-up-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="incrementFirstDigit('seconds')"
                ></i>
              </div>
              <div
                class="m-1"
                :class="{
                  'count-light count-size': !IsLightMode,
                  'count-dark count-size': IsLightMode,
                }"
              >
                {{ firstDigitSeconds }}
              </div>
              <div>
                <i
                  class="bi bi-caret-down-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="decrementFirstDigit('seconds')"
                ></i>
              </div>
            </div>
            <div class="ms-2">
              <div>
                <i
                  class="bi bi-caret-up-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="incrementSecondDigit('seconds')"
                ></i>
              </div>
              <div
                class="m-1"
                :class="{
                  'count-light count-size': !IsLightMode,
                  'count-dark count-size': IsLightMode,
                }"
              >
                {{ secondDigitSeconds }}
              </div>
              <div>
                <i
                  class="bi bi-caret-down-fill pointer arrow-size"
                  :class="{
                    'count-light': !IsLightMode,
                    'count-dark': IsLightMode,
                  }"
                  @click="decrementSecondDigit('seconds')"
                ></i>
              </div>
            </div>
          </div>
        </div>

        <div
          :style="{
            zIndex:
              timerData.selectedTimer === timerTypes?.HOURGLASS &&
              this.timerData.isTimeAdded
                ? '1'
                : '-1',
            opacity:
              timerData.selectedTimer === timerTypes?.HOURGLASS &&
              this.timerData.isTimeAdded
                ? '1'
                : '0',
            display: !this.timerData.isTimeAdded ? 'none' : '', // or 'none' depending on your preferred default display value
          }"
          class="hr-glass"
        >
          <svg viewBox="0 0 500 500" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <radialGradient
                gradientUnits="userSpaceOnUse"
                cx="226.928"
                cy="99.801"
                r="143.9"
                id="gradient-1"
                gradientTransform="matrix(2.520381, 0, 0, 0.105047, -345.016998, 89.317177)"
              >
                <stop offset="0" style="stop-color: rgb(250, 149, 121)" />
              </radialGradient>
              <radialGradient
                gradientUnits="userSpaceOnUse"
                cx="226.928"
                cy="99.801"
                r="143.9"
                id="gradient-2"
                gradientTransform="matrix(2.520381, 0, 0, 0.105047, -344.7435, -413.743469)"
              >
                <stop offset="0" style="stop-color: rgb(250, 149, 121)" />
              </radialGradient>
            </defs>
            <g
              transform="matrix(1.488654, 0, 0, 1.488654, -85.968338, -124.319496)"
            >
              <g id="topGlass">
                <polygon
                  style="fill: rgb(101, 214, 206)"
                  points="126.025 116.627 329.326 116.627 321.346 148.544 302.612 194.339 273.817 227.297 249.532 246.031 236.002 251.582 221.777 251.235 202.696 243.603 179.105 224.175 155.514 197.808 140.249 167.625 130.188 138.136"
                  id="topGlassSand"
                />
                <polygon
                  style="fill: rgb(255, 233, 214, 0.01); visibility: visible"
                  points="125.294 105.142 126.097 119.33 132.807 148.472 143.16 174.738 153.13 195.635 166.551 211.74 182.464 228.42 200.485 241.84 215.823 250.084 228.093 252.002 236.721 250.851 264.712 235.513 292.704 206.18 304.207 191.034 318.778 155.757 327.405 124.89 329.706 104.184"
                  id="topGlass"
                  class="zindex-1"
                />
                <path
                  d="M 124.797 107.509 C 124.797 138.949 150.131 190.99 153.291 195.278 C 156.418 199.522 195.775 252.31 227.32 252.31"
                  style="
                    stroke-width: 4px;
                    fill: none;
                    stroke: rgb(255, 233, 214);
                  "
                />
                <path
                  d="M 227.259 252.315 C 227.259 220.875 252.593 168.834 255.753 164.546 C 258.88 160.302 298.237 107.514 329.782 107.514"
                  style="
                    stroke-width: 4px;
                    fill: none;
                    stroke: rgb(255, 233, 214);
                  "
                  transform="matrix(-1, 0, 0, -1, 557.041016, 359.828995)"
                />
                <!-- <polygon
                  style="fill: url(#gradient-0)"
                  points="135.19 105.569 137.516 133.871 153.025 173.805 183.654 208.312 215.834 224.208 244.137 192.416 249.952 154.032 238.709 122.628 223.588 98.977"
                /> -->
              </g>
              <g
                transform="matrix(1, 0, 0, -1, 0.002997, 504.836395)"
                id="bottomGlass"
              >
                <polygon
                  style="fill: rgb(101, 214, 206)"
                  points="180.30499267578125 225.71600341796875 188.2050018310547 228.48599243164062 193.4530029296875 231.3000030517578 201.25999450683594 235.82699584960938 210.5489959716797 241.2469940185547 218.22500610351562 245.05499267578125 223.13900756835938 247.56300354003906 227.4550018310547 248.3040008544922 232.44700622558594 247.99899291992188 241.26499938964844 244.85800170898438 245.47000122070312 242.03199768066406 253.6529998779297 237.7729949951172 258.9440002441406 234.53199768066406 267.614013671875 228.1909942626953 276.17401123046875 221.01400756835938 284.3210144042969 217.03900146484375 286.7049865722656 214.45599365234375 301.4110107421875 198.55799865722656 319.6929931640625 157.2239990234375 327.2030029296875 134.00900268554688 329.9339904785156 107.78399658203125 125.0469970703125 109.96900177001953 131.8780059814453 146.8489990234375 145.53599548339844 180.9969940185547 158.6490020751953 203.67100524902344"
                  id="bottomGlassSand"
                />
                <polygon
                  style="fill: rgb(255, 233, 214, 0.01)"
                  points="124.947 105.142 126.097 119.33 132.807 148.472 143.16 174.738 153.13 195.635 166.551 211.74 182.464 228.42 200.485 241.84 215.823 250.084 228.093 252.002 236.721 250.851 264.712 235.513 292.704 206.18 304.207 191.034 318.778 155.757 327.405 124.89 329.706 104.184"
                  id="bottomGlass"
                />
                <path
                  d="M 124.797 107.509 C 124.797 138.949 150.131 190.99 153.291 195.278 C 156.418 199.522 195.775 252.31 227.32 252.31"
                  style="
                    stroke-width: 4px;
                    fill: none;
                    stroke: rgb(255, 233, 214);
                  "
                />
                <path
                  d="M 227.259 252.315 C 227.259 220.875 252.593 168.834 255.753 164.546 C 258.88 160.302 298.237 107.514 329.782 107.514"
                  style="
                    stroke-width: 4px;
                    fill: none;
                    stroke: rgb(255, 233, 214);
                  "
                  transform="matrix(-1, 0, 0, -1, 557.041016, 359.828995)"
                />
              </g>
              <rect
                x="93.028"
                y="88.21"
                width="270.801"
                height="23.182"
                style="fill: url(#gradient-1)"
                rx="10"
                ry="10"
              />
              <rect
                x="93.301"
                y="-414.851"
                width="270.801"
                height="23.182"
                style="fill: url(#gradient-2)"
                transform="matrix(1, 0, 0, -1, 0, 0)"
                rx="10"
                ry="10"
              />
            </g>
            <polygon
              style="fill: rgb(101, 214, 206)"
              points="237.024 244.856 243.903 242.98 250.468 242.355 257.034 241.73 263.6 242.98 269.708 245.097 264.171 247.051 262.217 247.865 260.751 248.517 260.425 249.82 260.262 252.262 260.1 255.031 259.774 261.708 263.008 458.515 256.353 458.515 248.242 257.34 247.207 253.68 247.045 252.549 246.561 250.771 246.076 249.64 245.106 247.702 235.574 245.278"
              id="middleSand"
            />
          </svg>
        </div>

        <div
          v-if="this.completedProgress == 0"
          class="complition-animation"
          ref="animationContainer"
        ></div>

        <div
          :class="[
            {
              'hour-glass-count':
                timerData.selectedTimer === timerTypes?.HOURGLASS &&
                this.timerData.isTimeAdded,
            },
            {
              'circle-count':
                timerData.selectedTimer === timerTypes?.CIRCLETIMER &&
                this.timerData.isTimeAdded,
            },
            {
              'watch-count':
                timerData.selectedTimer === timerTypes?.WATCHTIMER &&
                this.timerData.isTimeAdded,
            },
            { 'count-light': !IsLightMode },
            { 'count-dark': IsLightMode },
          ]"
        >
          <!-- {{
            timerData.selectedTimer && this.timerData.isTimeAdded
            ? formattedTime
            : ""
          }} -->
          <span>
            <template v-if="timerData.selectedTimer && timerData.isTimeAdded">
              <span class="d-flex">
                <span
                  v-for="(digit, index) in minutes"
                  :key="index"
                  style="width: 25px"
                  >{{ digit }}</span
                >
                <span class="ms-1 me-1">:</span>
                <span
                  v-for="(digit, index) in seconds"
                  :key="index"
                  style="width: 25px"
                  >{{ digit }}</span
                >
              </span>
            </template>
          </span>
        </div>
      </div>

      <!-- bottom buttons -->

      <div
        v-if="
          !this.musicSelection.IsSelectMusicIcn &&
          !this.musicSelection.Is_SelectMusic
        "
        class="bottom-buttons-timer p-2"
      >
        <div>
          <img
            v-if="this.completedProgress === 0"
            :src="ResetImg"
            class="pointer"
            alt="reset timer"
            :style="{ width: '36px', height: '36px' }"
            @click="resettimer"
          />
          <img
            v-else-if="!timerRunning"
            :src="PlayTimerImg"
            class="pointer"
            alt="startTimer"
            :style="{ width: '36px', height: '36px' }"
            @click="startTimer"
          />
          <img
            v-else
            :src="PauseTimerImg"
            class="pointer"
            alt="PauseTimerImg"
            :style="{ width: '36px', height: '36px' }"
            @click="stopTimer"
          />
        </div>
        <div>
          <img
            v-if="this.completedProgress === 100"
            :src="MusicImg"
            class="pointer"
            alt="Volume On"
            :style="{ width: '36px', height: '36px' }"
            @click="MusicImgClick"
          />
          <img
            v-else-if="IsSoundOn"
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
        <div>
          <img
            v-if="IsLightMode"
            :src="Sun"
            class="pointer"
            alt="light Mode On"
            :style="{ width: '36px', height: '36px' }"
            @click="stopLightMode"
          />
          <img
            v-else
            :src="Moon"
            class="pointer"
            alt="light Mode Off"
            :style="{ width: '36px', height: '36px' }"
            @click="startLightMode"
          />
        </div>
        <!-- <div>
        <img
          :src="DeleteIcn"
          alt="DeleteIcn"
          class="mt-1 pointer"
          :style="{ width: '28px', height: '28px' }"
        />
      </div> -->
      </div>
    </div>
  </div>
  <audio
    ref="tickAudio"
    :src="selectedSound || require('@/assets/sound/TimerWidget/beep.mp3')"
  ></audio>
</template>

<script>
import lottie from "lottie-web";
import animationData from "../assets/images/celebration.json";
import CircleProgress from "vue3-circle-progress";
import "vue3-circle-progress/dist/circle-progress.css"; 
import filledTimerImg from "@/assets/images/TimerWidget/filledTimerImg.png";
import Group4 from "@/assets/images/TimerWidget/Group4.png";
import trafficlight from "@/assets/images/TimerWidget/trafficlight.png";
import musical from "@/assets/images/TimerWidget/musical.png";
import HourGlass from "@/assets/images/TimerWidget/HourGlass.png";
import CircleTimer from "@/assets/images/TimerWidget/CircleTimer.png";
import WatchTimer from "@/assets/images/TimerWidget/WatchTimer.png";
import PlayTimerImg from "@/assets/images/TimerWidget/PlayTimerImg.png";
import BigHourGlass from "@/assets/images/TimerWidget/BigHourGlass.png";
import BigWatchTimer from "@/assets/images/TimerWidget/BigWatchTimer.png";
import PauseTimerImg from "@/assets/images/TimerWidget/PauseTimerImg.png";
import VolumOn from "@/assets/images/TimerWidget/VolumOn.png";
import VolumOff from "@/assets/images/TimerWidget/VolumOff.png";
import Sun from "@/assets/images/TimerWidget/Sun.png";
import Moon from "@/assets/images/TimerWidget/Moon.png";
import DeleteIcn from "@/assets/images/TimerWidget/DeleteIcn.png";
import ResetImg from "@/assets/images/TimerWidget/ResetImg.png";
import MusicImg from "@/assets/images/TimerWidget/MusicImg.png";
import OuterHourGlassImg from "@/assets/images/TimerWidget/OuterHourglass.svg";
import { timerTypes, colorsList } from "./constants/ConstData.js";

export default {
  components: {
    CircleProgress,
  },
  mounted() {
    this.$options.sounds = {
      beep: require("@/assets/sound/TimerWidget/beep.mp3"),
      bubble: require("@/assets/sound/TimerWidget/bubble.mp3"),
      buzzer: require("@/assets/sound/TimerWidget/buzzer.mp3"),
      smooth: require("@/assets/sound/TimerWidget/smooth.mp3"),
      sweet_end: require("@/assets/sound/TimerWidget/sweet_end.mp3"),
      times_up: require("@/assets/sound/TimerWidget/times_up.mp3"),
    };
  },

  data() {
    return { 
      filledTimerImg: filledTimerImg,
      Group4: Group4,
      trafficlight: trafficlight,
      musical: musical,
      selectedSound: null,
      timerTypes: timerTypes,
      colorTypes: colorsList,
      progressPercent: 50,
      HourGlass: HourGlass,
      CircleTimer: CircleTimer,
      WatchTimer: WatchTimer,
      BigWatchTimer: BigWatchTimer,
      PauseTimerImg: PauseTimerImg,
      PlayTimerImg: PlayTimerImg,
      BigHourGlass: BigHourGlass,
      OuterHourGlassImg: OuterHourGlassImg,
      ResetImg: ResetImg,
      MusicImg: MusicImg,
      Sun: Sun,
      Moon: Moon,
      DeleteIcn: DeleteIcn,
      VolumOn: VolumOn,
      VolumOff: VolumOff,
      timerData: {
        selectedTimer: timerTypes?.HOURGLASS,
        isTimeAdded: false,
      },
      musicSelection: {
        IsSelectMusicIcn: false,
        Is_SelectMusic: false,
      },
      firstDigitMinutes: 0,
      secondDigitMinutes: 0,
      firstDigitSeconds: 0,
      secondDigitSeconds: 0,
      timerRunning: false,
      IsSoundOn: true,
      IsLightMode: true,
      timerInterval: null,
      timerMinutes: 0,
      timerSeconds: 0,
      TotalSeconds: 0,
      completedProgress: 100,
      interval: 0,
      secondsLeft: 0,
      remainingTime: 0,
      tempSound: "beep",
    };
  },

  computed: {
    formattedTime() {
      return `${this.timerMinutes < 10 ? "0" : ""}${this.timerMinutes}:${
        this.timerSeconds < 10 ? "0" : ""
      }${this.timerSeconds}`;
    },
    minutes() {
      return this.formattedTime.split(":")[0];
    },
    seconds() {
      return this.formattedTime.split(":")[1];
    },
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

    selectTimer(timer) {
      this.timerData.selectedTimer = timer;
    },

    incrementFirstDigit(type) {
      this.remainingTime = 0;
      this.completedProgress = 100;
      const currentDigit =
        this[type === "minutes" ? "firstDigitMinutes" : "firstDigitSeconds"];
      if (type === "seconds" && currentDigit === 5) {
        this[
          type === "minutes" ? "firstDigitMinutes" : "firstDigitSeconds"
        ] = 0;
      } else {
        this[type === "minutes" ? "firstDigitMinutes" : "firstDigitSeconds"] =
          (currentDigit + 1) % 10;
      }
    },

    decrementFirstDigit(type) {
      this.remainingTime = 0;
      this.completedProgress = 100;
      const currentDigit =
        this[type === "minutes" ? "firstDigitMinutes" : "firstDigitSeconds"];
      if (type === "seconds" && currentDigit === 0) {
        this[
          type === "minutes" ? "firstDigitMinutes" : "firstDigitSeconds"
        ] = 5;
      } else {
        this[type === "minutes" ? "firstDigitMinutes" : "firstDigitSeconds"] =
          (currentDigit - 1 + 10) % 10;
      }
    },

    incrementSecondDigit(type) {
      this.remainingTime = 0;
      this.completedProgress = 100;
      this[type === "minutes" ? "secondDigitMinutes" : "secondDigitSeconds"] =
        (this[
          type === "minutes" ? "secondDigitMinutes" : "secondDigitSeconds"
        ] +
          1) %
        10;
    },

    decrementSecondDigit(type) {
      this.remainingTime = 0;
      this.completedProgress = 100;
      this[type === "minutes" ? "secondDigitMinutes" : "secondDigitSeconds"] =
        (this[
          type === "minutes" ? "secondDigitMinutes" : "secondDigitSeconds"
        ] -
          1 +
          10) %
        10;
    },

    startTimer() {
      let animationStarted = false;
      // this.timerData.selectedTimer = null;
      this.timerMinutes = this.firstDigitMinutes * 10 + this.secondDigitMinutes;
      this.timerSeconds = this.firstDigitSeconds * 10 + this.secondDigitSeconds;
      if (this.completedProgress == 100) {
        this.totalSeconds = this.timerMinutes * 60 + this.timerSeconds;
      }

      if (
        (this.timerMinutes > 0 || this.timerSeconds > 0) &&
        !this.timerRunning
      ) {
        if (this.remainingTime > 0) {
          // this.totalSeconds = this.remainingTime;
        } else {
          this.totalSeconds = this.timerMinutes * 60 + this.timerSeconds;
        }
        // this.completedProgress = 100;
        this.timerData.isTimeAdded = true;
        this.timerRunning = true;
        this.timerInterval = setInterval(() => {
          if (this.timerMinutes === 0 && this.timerSeconds === 0) {
            this.stopTimer();
            if (this.IsSoundOn) {
              this.$refs.tickAudio.play();
            }
          } else {
            if (this.timerSeconds === 0) {
              this.timerMinutes -= 1;
              this.timerSeconds = 59;
            } else {
              this.timerSeconds--;
            }
            this.updateDigits();
            this.calculateProgress();
            if (!animationStarted) {
              animationStarted = true;
              this.animationStart(this.totalSeconds);
            }
          }
        }, 1000);
      }
    },

    stopTimer() {
      clearInterval(this.timerInterval);
      this.timerRunning = false;
      if (this.completedProgress !== 0) {
        this.timerData.isTimeAdded = false;
      } else {
        this.initAnimation();
      }
      this.remainingTime = this.timerMinutes * 60 + this.timerSeconds;
    },

    animationStart(time) {
      if (this.remainingTime > 0) {
        time = this.remainingTime;
      }

      this.clearHourglass();
      setTimeout(() => {
        const middleAnimationTime = time / 10;
        const sandAnimationTime = time - middleAnimationTime;
        document.querySelector("#middleSand").style.animationDelay = `${
          sandAnimationTime - 0.04 * sandAnimationTime
        }s`;
        document.querySelector(
          "#middleSand"
        ).style.animationDuration = `${middleAnimationTime}s`;
        document.querySelector(
          "#bottomGlassSand"
        ).style.animationDuration = `${sandAnimationTime}s`;
        document.querySelector(
          "#topGlassSand"
        ).style.animationDuration = `${sandAnimationTime}s`;

        document.querySelector("#middleSand").style.animationName =
          "middleGlassAnimate";
        document.querySelector("#middleSand").style.visibility = "visible";
        document.querySelector("#bottomGlassSand").style.visibility = "visible";
        document.querySelector("#topGlassSand").style.animationName =
          "topGlassAnimate";
        document.querySelector("#bottomGlassSand").style.animationName =
          "bottomGlassAnimate";

        this.secondsLeft = time;
        this.interval = setInterval(() => {
          this.secondsLeft--;
          if (this.secondsLeft === 0) {
            clearInterval(this.interval);
          }
        }, 1000);
      }, 5);
    },
    clearHourglass() {
      clearInterval(this.interval);
      this.secondsLeft = 0;

      document.querySelector("#topGlassSand").style.animation = "";
      document.querySelector("#bottomGlassSand").style.animation = "";
      document.querySelector("#middleSand").style.animation = "";

      document.querySelector("#topGlassSand").style.animationDuration = "";
      document.querySelector("#bottomGlassSand").style.animationDuration = "";
      document.querySelector("#middleSand").style.animationDuration = "";

      document.querySelector("#middleSand").style.visibility = "hidden";
      document.querySelector("#bottomGlassSand").style.visibility = "hidden";
    },

    resettimer() {
      clearInterval(this.timerInterval);
      this.timerRunning = false;
      this.timerData.isTimeAdded = false;
      this.completedProgress = 100;
    },

    MusicImgClick() {
      this.musicSelection.IsSelectMusicIcn = true;
    },
    handleClickSelectTrackBtn() {
      this.musicSelection.IsSelectMusicIcn = false;
      this.musicSelection.Is_SelectMusic = true;
    },
    handleClickCancleBtn() {
      this.musicSelection.IsSelectMusicIcn = false;
      this.musicSelection.Is_SelectMusic = false;
    },

    handleSaveSound() {
      this.selectedSound = this.$options.sounds[this.tempSound];
      this.musicSelection.IsSelectMusicIcn = false;
      this.musicSelection.Is_SelectMusic = false;
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

    updateDigits() {
      const minutesStr = this.timerMinutes.toString().padStart(2, "0");
      const secondsStr = this.timerSeconds.toString().padStart(2, "0");
      this.firstDigitMinutes = parseInt(minutesStr.charAt(0));
      this.secondDigitMinutes = parseInt(minutesStr.charAt(1));
      this.firstDigitSeconds = parseInt(secondsStr.charAt(0));
      this.secondDigitSeconds = parseInt(secondsStr.charAt(1));
    },

    calculateProgress() {
      const totalSeconds = this.totalSeconds; 
      const elapsedSeconds =
        (this.firstDigitMinutes * 10 + this.secondDigitMinutes) * 60 +
        this.firstDigitSeconds * 10 +
        this.secondDigitSeconds;

      const remainingSeconds = Math.max(0, totalSeconds + elapsedSeconds);
      const progress = (remainingSeconds / totalSeconds) * 100;
      this.completedProgress = Math.round((progress - 100) * 100) / 100;

      return Math.round((progress - 100) * 100) / 100; // Round to 2 decimal places
    },
  },
};
</script>

<style scoped>
@import "../assets/style.css";
@import "~bootstrap-icons/font/bootstrap-icons.css";
</style>
