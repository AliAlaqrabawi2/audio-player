<template>
  <div class="container">
    <div class="audio-player">
      <img
        v-show="!isBasic"
        src="/icons/backward.svg"
        alt="Backward"
        @click="rewind"
      >
      <img
        v-show="isPlaying"
        src="/icons/pause.svg"
        class="play-pause-button"
        alt="Play"
        @click="togglePlay"
      >
      <img
        v-show="!isPlaying"
        src="/icons/play.svg"
        class="play-pause-button"
        alt="Play"
        @click="togglePlay"
      >

      <img
        v-show="!isBasic"
        src="/icons/forward.svg"
        alt="Forward"
        @click="fastForward"
      >
      <p class="time-indicator">
        {{ `${formatTime(currentTime)}/${formatTime(reminderTime)}` }}
      </p>
      <input
        v-show="!isBasic"
        v-model.number="currentTime"
        type="range"
        :max="duration"
      >
      <img
        v-show="isMuted && !isBasic"
        src="/icons/unmute.svg"
        alt="unMute"
        @click="toggleMute"
      >
      <img
        v-show="!isMuted && !isBasic"
        src="/icons/mute.svg"
        alt="Mute"
        @click="toggleMute"
      >
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import { formatTime } from '../utils/time.ts';

interface Props {
  duration: number;
  isPlaying?: boolean;
  isBasic?: boolean;
}
const props = withDefaults(defineProps<Props>(), {
  isPlaying: false,
  isBasic: false,
});

const isPlaying = ref<boolean>(props.isPlaying);
const isMuted = ref<boolean>(false);
const currentTime = ref<number>(0);
const timer = ref();
const reminderTime = ref(props.duration);

const togglePlay = () => {
  isPlaying.value = !isPlaying.value;
  updateTimer();
};
const updateTimer = () => {
  if (isPlaying.value) {
    timer.value = setInterval(() => {
      if (currentTime.value < props.duration) {
        currentTime.value += 1;
        reminderTime.value = props.duration - currentTime.value;
      } else {
        clearInterval(timer.value);
        isPlaying.value = false;
        currentTime.value = 0;
        reminderTime.value = props.duration;
      }
    }, 1000);
  } else {
    clearInterval(timer.value);
  }
};

const rewind = () => {
  currentTime.value = Math.max(0, currentTime.value - 5);
};

const fastForward = () => {
  currentTime.value = Math.min(props.duration, currentTime.value + 5);
};

const toggleMute = () => {
  isMuted.value = !isMuted.value;
};

updateTimer();
</script>

<style scoped>
.container {
  background-color: rgba(0, 0, 0, 0.9);
  width: fit-content;
  max-width: 100vw;
  height: 50px;
  border-radius: 9px;
}
.audio-player {
  margin: 0 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
img {
  cursor: pointer;
}
.play-pause-button {
  margin-top: calc(50px - 41px);
}

input {
  -webkit-appearance: none;
  appearance: none;
  overflow: hidden;
  outline: none;
  cursor: pointer;
  background-color: #fff;
  border-radius: 9px;
  height: 5px;
  margin: 0 5px;
}

input::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  background-color: var(--primary-color);
  width: 8px;
  height: 8px;
  box-shadow: -407px 0 0 400px var(--primary-color);
  border-radius: 50%;
}

.time-indicator {
  color: #fff;
  font-size: 12px;
  margin: 0 5px;
}
</style>
