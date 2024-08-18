<script setup>
import { storeToRefs } from "pinia";
import AppLayot from "../components/AppLayout.vue";
import { useRootStore } from "../stores/root";

import CocktailsThumbs from "@/components/CocktailsThumbs.vue";

const rootStore = useRootStore();
rootStore.getIngredients();
const { ingredients, cocktails, ingredient } = storeToRefs(rootStore);
function removeIngredient() {
  rootStore.setIngridient(null);
}
function getCocktails() {
  rootStore.getCocktails(rootStore.ingredient);
}
</script>
<template>
  <AppLayot
    img-url="/img/bg-1.jpg"
    :back-func="removeIngredient"
    :is-back-button-disable="ingredient"
  >
    <div class="wrapper">
      <div v-if="!ingredient || !cocktails" class="info">
        <div class="title">Choose your drink</div>
        <div class="line"></div>
        <div class="select-wrapper">
          <el-select
            v-model="rootStore.ingredient"
            placeholder="Choose main ingredient"
            size="large"
            filterable
            allow-create
            class="select"
            @change="getCocktails"
          >
            <el-option
              v-for="item in ingredients"
              :key="item.strIngredient1"
              :label="item.strIngredient1"
              :value="item.strIngredient1"
            ></el-option>
          </el-select>
        </div>
        <div class="text">
          Try our delicious cocktail recipes for every occasion. Find delicious
          cocktail recipes by ingredient through our cocktail generator.
        </div>
        <img src="/img/cocktails.png" alt="Cocktails" class="img" />
      </div>
      <div v-else class="info">
        <div class="title">COCKTAILS WITH {{ ingredient.toUpperCase() }}</div>
        <div class="line"></div>
        <div class="cocktails">
          <CocktailsThumbs
            v-for="cocktail in cocktails"
            :key="cocktail.idDrink"
            :cocktail="cocktail"
          />
        </div>
      </div>
    </div>
  </AppLayot>
</template>
<style lang="sass" scoped>
@import '../assets/styles/main'

.select-wrapper
    padding-top: 50px

.select
    width: 220px

.text
    max-width: 516px
    margin: 0 auto
    padding-top: 50px
    line-height: 36px
    letter-spacing: .1em
    color: $textMuted

.img
    margin-top: 60px

.cocktails
    display: flex
    align-items: center
    margin-top: 60px
    flex-wrap: wrap
    max-height: 400px
    overflow-y: auto
</style>
