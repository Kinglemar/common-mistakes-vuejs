<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";

export type Recipe = {
  id: number;
  name: string;
  image: string;
};

const props = defineProps<{
  recipe: Recipe;
}>();

const recipeDetails = ref();

async function getMoreContext() {
  try {
    const dataFromApi = await axios.get(
      `https://dummyjson.com/recipes/${props.recipe.id}`
    );
    recipeDetails.value = dataFromApi.data;
  } catch (e) {
    console.log(e);
  }
}

function getInfo() {
  if (recipeDetails.value) {
    alert(JSON.stringify(recipeDetails.value, null, 2));
  } else {
    alert("Still fetching data please wait..");
  }
}

onMounted(() => {
  getMoreContext();
  console.log('Detailing');
});
</script>
<template>
  <section class="h-52 overflow-hidden rounded-lg relative">
    <img class="object-contain" :src="recipe.image" />
    <p class="absolute bottom-4 left-1 dark:bg-black bg-white px-2 py-.5 rounded-md">
      {{ recipe.name }}
    </p>
    <button @click="getInfo" class="absolute top-2 right-2 p-2 rounded-md">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="28"
        height="28"
        viewBox="0 0 24 24"
      >
        <path
          fill="#000"
          d="M22 12c0 5.523-4.477 10-10 10S2 17.523 2 12S6.477 2 12 2s10 4.477 10 10"
          opacity=".5"
        />
        <path
          fill="#000"
          d="M12 17.75a.75.75 0 0 0 .75-.75v-6a.75.75 0 0 0-1.5 0v6c0 .414.336.75.75.75M12 7a1 1 0 1 1 0 2a1 1 0 0 1 0-2"
        />
      </svg>
    </button>
  </section>
</template>

<style scoped></style>
