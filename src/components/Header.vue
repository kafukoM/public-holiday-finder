<script setup>
const props = defineProps(['title']);
</script>


<template>
    <div class="container">
        <h1>{{ title }}</h1>
    </div>
</template>


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
        color: green;
    }
</style>

