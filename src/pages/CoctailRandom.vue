<script setup>
import { computed, ref } from "vue";
import { COCKTAIL_RANDOM, INGREDIENTS_PIC } from "@/constants";
import axios from "axios";

import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";

import AppLayot from "../components/AppLayout.vue";

const cocktail = ref(null);

const ingredients = computed(() => {
  const ingredients = [];
  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) {
      break;
    }
    const ingredient = cocktail.value[`strIngredient${i}`];
    ingredients.push(ingredient);
  }
  return ingredients;
});
async function getCocktail() {
  const { data } = await axios.get(`${COCKTAIL_RANDOM}`);
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
          <div class="slider">
            <Swiper class="swiper" :slides-per-view="3" :space-between="50">
              <SwiperSlide
                v-for="(ingredient, key) in ingredients"
                :key="key"
                class="slider-item"
              >
                <img
                  :src="`${INGREDIENTS_PIC}${ingredient}-Small.png`"
                  :alt="ingredient"
                />
                <div class="name">
                  {{ ingredient }}
                </div>
              </SwiperSlide>
            </Swiper>
          </div>
          <div class="instruction">{{ cocktail.strInstructions }}</div>
        </div>
      </div>
    </AppLayot>
  </div>
</template>
<style lang="sass" scoped>
@import '../assets/styles/main'
.slider
    padding: 50px 0

.swiper
    width: 586px

.name
    padding-top: 20px
</style>
