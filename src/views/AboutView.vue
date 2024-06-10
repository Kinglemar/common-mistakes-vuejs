<script setup lang="ts">
import { ref, computed } from "vue";
import { useMediaControls } from "@vueuse/core";
import PlayButton from "@/components/icons/PlayButton.vue";
import PauseButton from "@/components/icons/PauseButton.vue";
const videoPlayer = ref<HTMLVideoElement>();
const videoRef = ref();

const { playing: videoPlaying } = useMediaControls(videoRef, {
  src: "/shrek_meets_donkey.mp4",
});

//Dependency not reactive
const playing = computed(() => !videoPlayer.value?.paused);
</script>
<template>
  <main class="md:min-h-screen md:p-5 p-2">
    <h1 class="text-5xl">Ouch, Mistakes. - Non reactive dependency.</h1>

    <section class="mt-16 relative">
      <video ref="videoRef" class="mx-auto h-96" />
      <div
        v-auto-animate
        class="inline-flex gap-3 absolute top-[48%] right-[46%]"
      >
        <button
          @click="videoRef?.play()"
          v-if="!videoPlaying"
          class="p-2 rounded-md bg-black"
        >
          <PlayButton />
        </button>
        <button
          v-else
          @click="videoRef?.pause()"
          class="p-2 rounded-md bg-red-700"
        >
          <PauseButton />
        </button>
      </div>
    </section>
  </main>
</template>

<style></style>
