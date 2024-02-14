<template>
    <div class="container">
        <h1>{{ title }}</h1>
        <button @click="showDropdown" class="anchor">Select Country</button>
        <div class="menu-item" v-if="showMenu" v-for="country in countries" :key="country.code" @click="setCountry(country)"> {{ country.name }}</div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
const props = defineProps(['title', 'countries'])
const emits = defineEmits(['selected-country'])


const showMenu = ref(false)

const showDropdown = () => {
    showMenu.value = !showMenu.value
}

const setCountry = (country) => {
    showMenu.value = false
    emits('selected-country', country)
}

</script>




<style scoped>
    .container {
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }

    
    h1 {
        color: blue;
    }

    .menu {
        background-color: #fff;
        border-radius: .25rem;
        color: #212529;
        cursor: pointer;
    }

    .menu-item {
        color: green;
    }

    .menu-item:hover {
        background-color: green;
        color: white;
        cursor: pointer;
    }
</style>

