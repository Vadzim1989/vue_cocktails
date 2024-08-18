<script setup>
import { computed, ref } from "vue";
import { useRoute } from "vue-router";
import { COCTAIL_BY_ID } from "@/constants";
import axios from "axios";

import AppLayot from "../components/AppLayout.vue";

const route = useRoute();

const cocktailId = computed(() => route.path.split("/").pop());
const cocktail = ref(null);

const ingredients = computed(() => {
  const ingredients = [];
  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) {
      break;
    }
    const ingredient = {};
    ingredient.name = cocktail.value[`strIngredient${i}`];
    ingredient.measure = cocktail.value[`strMeasure${i}`];
    ingredients.push(ingredient);
  }
  return ingredients;
});
async function getCocktail() {
  const { data } = await axios.get(`${COCTAIL_BY_ID}${cocktailId.value}`);
  cocktail.value = data?.drinks[0];
}
getCocktail();
</script>
<template>
  <div v-if="cocktail" class="wrap">
    <AppLayot :img-url="cocktail.strDrinkThumb">
      <div class="wrapper">
        <div class="info">
          <div class="title">{{ cocktail.strDrink }}</div>
          <div class="line"></div>
          <div class="list">
            <div
              v-for="(ingredient, key) in ingredients"
              :key="key"
              class="list-item"
            >
              {{ ingredient.name }}
              <template v-if="ingredient.measure">
                | {{ ingredient.measure }}
              </template>
            </div>
          </div>
          <div class="instruction">{{ cocktail.strInstructions }}</div>
        </div>
      </div>
    </AppLayot>
  </div>
</template>
<style lang="sass" scoped>
@import '../assets/styles/main'
</style>
