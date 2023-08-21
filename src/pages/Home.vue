<script setup>
import AppLayout from '../components/AppLayout.vue';
import CocktailThumb from '../components/CocktailThumb.vue'
import {useRootStore} from '@/stores/root';
import {storeToRefs} from 'pinia';

const rootStore = useRootStore();
rootStore.getIngredients();

const {ingredients, ingredient, cocktails} = storeToRefs(rootStore);

function iventGetCocktails() {
    rootStore.getCocktails(rootStore.ingredient)
}

function removeIngredient() {
    rootStore.setIngredient(null)
}
</script>

<template>
    <AppLayout 
    imgUrl='src/assets/img/bg-1.jpg' 
    :backButton="removeIngredient"
    :isBackButtonVisible="!!ingredient"
    >
        <div class="wrapper">
            <div v-if="!ingredient" class="info">
                <div class="title">Choose your drink</div>
                <div class="line"></div>
                <div class="select-wrapper">
                    <el-select 
                    v-model="rootStore.ingredient" 
                    placeholder="Choose main ingredient" 
                    size="large"
                    class="select"
                    @change="iventGetCocktails"
                    >
                        <el-option
                        v-for="item in ingredients"
                        :key="item.strIngredient1"
                        :label="item.strIngredient1"
                        :value="item.strIngredient1"
                        />
                    </el-select>
                </div>
                <div class="text">Try our delicious cocktail recipes for every occasion. Find delicious cocktail recipes by ingredient through our cocktail generator.</div>
                <img src="src/assets/img/cocktails.png" alt="Cocktails" class="img">
            </div>
            <div v-else class="info">
                <div class="title">COCKTAILS WITH {{ ingredient }}</div>
                <div class="line"></div>
                <div class="cocktails">
                    <CocktailThumb
                    v-for="cocktail in cocktails"
                    :key="cocktail.idDrink"
                    :cocktail="cocktail"
                    >
                    </CocktailThumb>
                </div>
            </div>   
        </div>
    </AppLayout>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.select-wrapper
    padding-top: 50px
.text
    padding-top: 50px
    min-width: 516px
    letter-spacing: 0.1em
    line-height: 36px
    color: $textMuted
    margin: 0 auto
.img
    padding-top: 60px
.cocktails
    display: flex
    flex-wrap: wrap
    margin-top: 60px
    max-height: 700px
    overflow-y: auto 
    
</style>