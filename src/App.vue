<template>
  <div class="app">
    <AudioPlayer
      :duration="130"
      :is-playing="false"
    />
    <AudioPlayer
      :duration="130"
      :is-playing="true"
    />
    <AudioPlayer
      :duration="130"
      :is-playing="true"
      :is-basic="true"
    />
  </div>
</template>

<script setup lang="ts">
import AudioPlayer from './components/AudioPlayer.vue';
</script>

<style>
div {
  margin: 20px 0;
}
body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
</style>
