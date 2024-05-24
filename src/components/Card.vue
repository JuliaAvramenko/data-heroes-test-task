<script setup>
const props = defineProps({
    info: {
        type: [Object, null],
        required: true,
    }
});

function indicateStyle(status) {
    if (status === 'Alive') {
        return 'background-color: rgb(34, 144, 24);'
    }
    if (status === 'Dead') {
        return 'background-color: rgb(164, 14, 14);'
    }
    return ''
}
</script>

<template>
    <article class="card" v-for="(item, index) in info" :key="index">
        <img :src="item.image" alt="character" class="image">
        <div class="wrapper">
            <h2 class="title">{{ item.name }}</h2>
            <p class="status">
            <div class="circle" :style="indicateStyle(item.status)"></div>{{ item.status }} - {{ item.species }}</p>
            <p class="text">Last known location:</p>
            <p class="info">{{ item.origin.name }}</p>
            <p class="text">First seen in:</p>
            <p class="info">{{ item.location.name }}</p>
        </div>
    </article>
</template>

<style scoped>
.card {
    display: flex;
    width: 600px;
    height: 220px;
    background-color: rgb(60, 62, 68);
    border-radius: 10px;
    color: #fff;
}

.image {
    width: 42%;
    height: 100%;
    object-fit: cover;
    border-radius: 10px 0 0 10px;
}

.wrapper {
    display: flex;
    flex-direction: column;
    padding: 15px;
    row-gap: 5px;
}

.title {
    font-size: 30px;
}

.status {
    display: flex;
    font-size: 18px;
    margin-bottom: 20px;
    align-items: center;
    column-gap: 5px;
}

.text {
    font-size: 15px;
    color: rgb(158, 158, 158);
}

.info {
    font-size: 18px;
}

.circle {
    width: 10px;
    height: 10px;
    background-color: #fff;
    border-radius: 50%;
}
</style>
