<script setup lang="ts">
import { reactive, ref, computed } from "vue";

const isLoading = ref(false);

let swiftBirdsInEurope = reactive([
  "Alpine swift",
  "Chimney swift",
  "Pacific swift",
]);

const arrLength = computed(() => swiftBirdsInEurope.length)

function loadMoreBirds() {
  isLoading.value = true;
  swiftBirdsInEurope = [
    "Common swift",
    "Little swift",
    "Pallid swift",
    "White-rumped swift",
  ];
  setTimeout(() => (isLoading.value = false), 2000);
}
</script>
<template>
  <main class="md:min-h-screen md:p-5 p-2">
    <h1 class="text-5xl">Ouch, Mistakes - Replacing reactive value</h1>

    <section class="mt-16 relative">
      <button
        @click="loadMoreBirds"
        class="px-4 py-1.5 absolute -top-16 right-5 bg-green-700 text-white"
      >
        Add more birds
      </button>
      <h1 class="text-3xl my-3">Swift birds in Europe</h1>
      <pre>{{ arrLength }}</pre>

      <h1 class="text-3xl my-3" v-if="isLoading">Loading...</h1>
      <ul v-else class="list-disc pl-3">
        <li v-for="item in swiftBirdsInEurope" :key="item">{{ item }}</li>
      </ul>
    </section>
  </main>
</template>

<style></style>
