<script setup>
import AppLayout from '../components/AppLayout.vue';
import { ref, computed } from 'vue';
import {useRoute} from 'vue-router';
import {COCKTAIL_BY_ID} from '@/constants';
import axios from 'axios';

const route = useRoute();

const cocktailID = computed(() => route.path.split('/').pop())

const ingredients = computed(() => {
    const ingredients = [];

    for (let i = 1; i <= 15; i++) {

        if (!cocktail.value[`strIngredient${i}`]) break

        const ingredient = {};
        ingredient.name = cocktail.value[`strIngredient${i}`]
        ingredient.measure = cocktail.value[`strMeasure${i}`]
        ingredients.push(ingredient)
    }

    return ingredients
})

const cocktail = ref(null);

async function getDetailCocktail() {
    const data = await axios.get(`${COCKTAIL_BY_ID}${cocktailID.value}`);
    cocktail.value = data?.data?.drinks[0];
}

getDetailCocktail();
</script>

<template>
    <div v-if="cocktail" class="wrap">
        <AppLayout :imgUrl='cocktail.strDrinkThumb'>
            <div class="wrapper">
                <div class="info">
                    <div class="title">{{ cocktail.strDrink }}</div>
                    <div class="line"></div>
                    <div class="list">
                        <div 
                        class="list-item" 
                        v-for="(item, key) in ingredients"
                        :key="key"
                        >
                        {{ item.name }}
                            <template v-if="item.measure">
                                |
                                {{ item.measure }}
                            </template>
                        </div>
                    </div>
                    <div class="instructions">{{ cocktail.strInstructions }}</div>
                </div>
            </div>
        </AppLayout>
    </div>

</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

</style>