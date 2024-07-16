<script setup>
import axios from "axios";
import AppLayout from "@/components/AppLayout.vue";
import { computed, ref } from "vue";
import { COCKTAIL_RANDOM, INGREDIENT_PIC } from "@/constants/api";


const cocktail = ref(null);

const ingredients = computed(() => {
  const ingredients = [];

  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) break;

    const ingredient = cocktail.value[`strIngredient${i}`];

    ingredients.push(ingredient);
  }

  return ingredients;
});

async function getCocktail() {
  const data = await axios.get(COCKTAIL_RANDOM);
  cocktail.value = data?.data?.drinks[0];
}


getCocktail();
</script>

<template>
  <div v-if="cocktail" class="wrapp">
    <AppLayout :imgUrl="cocktail.strDrinkThumb">
      <div class="wrapper">
        <div class="info">
          <div class="title">{{ cocktail.strDrink }}</div>
          <div class="line" style="margin-bottom: 20px;"></div>
          <div class="slider" style="margin-bottom: 20px;">
            <div
              class="slide"
              v-for="(ingredient, key) in ingredients"
              :key="key"
            >
              <img :src="`${INGREDIENT_PIC}${ingredient}-Small.png`">
              <div class="name">{{ ingredient }}</div>
            </div>
          </div>
          <div class="instructions">
            {{ cocktail.strInstructions }}
          </div>
        </div>
      </div>
    </AppLayout>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.slider
    display: grid
    grid-template-columns: 1fr 1fr 1fr
    gap: 40px

.name
    padding-top: 20px
</style>
