<script setup lang="ts">
import { ref, onMounted, computed } from "vue";
import axios from "axios";
import { shuffleArray } from "@/utils/shuffle";
import SingleRecipe from "@/components/SingleRecipe.vue";

type Recipe = {
  id: number;
  name: string;
  image: string;
};

const favoriteRecipeList = ref<Recipe[]>([]);
const isLoading = ref(true);

const haveRecipes = computed(() =>
  favoriteRecipeList.value.length > 1 ? true : false
);

async function fetchData() {
  try {
    const dataFromApi = await axios.get(
      "https://dummyjson.com/recipes?page=1&limit=10&skip=10&select=name,image"
    );
    const recipes = dataFromApi.data.recipes as Recipe[];

    favoriteRecipeList.value = [...favoriteRecipeList.value, ...recipes];
    isLoading.value = false;
  } catch (e) {
    isLoading.value = false;
    console.log(e);
  }
}

function shuffleRecipe() {
  favoriteRecipeList.value = shuffleArray([...favoriteRecipeList.value]);
}

onMounted(async () => {
  await fetchData();
});
</script>

<template>
  <main class="md:min-h-screen md:p-5 p-2">
    <h1 class="text-5xl">Ouch, Mistakes.</h1>

    <section
      v-if="isLoading"
      class="w-full h-72 flex flex-col items-center justify-center"
    >
      <p class="text-3xl">Loading...</p>
    </section>

    <section
      v-auto-animate
      v-else
      class="w-full grid md:grid-cols-3 sm:grid-cols-2 grid-cols-1 gap-3 mt-16 relative mb-14"
    >
      <button
        :disabled="!haveRecipes"
        @click="shuffleRecipe"
        class="px-4 py-1.5 absolute -top-16 right-5 bg-green-700 text-white"
      >
        Shuffle
      </button>
      <SingleRecipe
        v-for="item in favoriteRecipeList"
        :key="item.name"
        :recipe="item"
      />
    </section>
  </main>
</template>
