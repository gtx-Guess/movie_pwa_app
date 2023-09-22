<template>
    <div class="movie-detail">
        <h2>{{ movie.Title }}</h2>
        <p>{{ movie.Year }}</p>
        <img :src="movie.Poster" alt="Movie Poster" class="featured-img" loading="lazy">
        <p>{{ movie.Plot }}</p>
    </div>
</template>

<style lang="scss">

.movie-detail{
    padding: 16px;
    h2{
        color: #FFF;
        font-size: 28px;
        font-weight: 600;
        margin-bottom: 16px;
    }
    .featured-img{
        display: block;
        max-width: 200px;
        margin-bottom: 16px;
    }
    p{
        color: #FFF;
        font-size: 18px;
        line-height: 1.4;
    }
}

</style>

<script setup>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import env from '@/env.js';

const movie = ref({});
const route = useRoute();

onBeforeMount( async () => {
    const resp = await axios.get(`https://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`);
    movie.value = resp.data;
});

</script>
