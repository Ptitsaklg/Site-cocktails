<script setup>
import {Back} from '@element-plus/icons-vue';
import {useRoute, useRouter} from 'vue-router';
import { ROUTES_PATHS } from '../constants';
import {computed} from 'vue';

const props = defineProps({
    imgUrl: {
        type: String,
        required: true,
    },
    backButton: {
        type: Function,
    },
    isBackButtonVisible: {
        type: Boolean,
        default: true,
    }
})

const route = useRoute();
const router = useRouter();

const routeName = computed(() => route.name)

function getRandomCocktal() {
    router.push(ROUTES_PATHS.RANDOM_COCKTAIL);

    if (routeName.value === ROUTES_PATHS.RANDOM_COCKTAIL) {
        router.go();
    }
}

function goBack() {
    props.backButton ? props.backButton() : router.go(-1);
}
</script>

<template>
    <div class="root">
        <div :style="`background-image: url(${imgUrl})`" class="img"></div>
        <div class="main">
            <div>
                <el-button 
                class="btn-random"
                @click="getRandomCocktal"
                >
                Get random cocktail</el-button>

                <el-button
                v-if="isBackButtonVisible" 
                class="back" 
                type="primary" 
                :icon="Back" 
                circle 
                @click="goBack"
                />
            </div>
            <slot></slot>
        </div>
        
    </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'
.root 
    display: flex
    min-height: 100vh
    background-color: $background
.img
    width: 50%
    background-position: 50% 50%
    background-repeat: no-repeat
    background-size: 100%
    margin-left: 40px
.main
    position: relative
    width: 50%
    padding: 32px 40px

.btn-random
    position: absolute
    top: 32px
    right: 40px
    background-color: $accent
    border-color: $accent
    color: $text
    font-family: 'Raleway', 'Arias', sans-serif
    
    &:hover
        background-color: darken($accent, 15%)
        border-color: darken($accent, 15%)

.back
    background-color: transparent
    border-color: #fff
    width: 50px
    height: 50px
    font-size: 20px

    &:hover
        border-color: $accent
</style>