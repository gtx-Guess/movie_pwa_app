<template>
    <div class="movie-detail">
        <h2>{{ movie.Title }}</h2>
        <p>{{ movie.Year }}</p>
        <div class="poster">
            <img :src="movie.Poster" alt="Movie Poster" class="featured-img" loading="lazy">
            <div class="pre-plot">
                <p><span class="header">Country of Origin</span>: {{ movie.Country }}</p>
                <p><span class="header">Language</span>: {{ movie.Language }}</p>
                <p><span class="header">Actors</span>: {{ movie.Actors }}</p>
                <p><span class="header">Genre</span>: {{ movie.Genre }}</p>
                <p><span class="header">Rating</span>: {{ movie.imdbRating }}/10</p>
                <p><span class="header">Rated</span>: {{ movie.Rated }}</p>
            </div>
        </div><br>
        <p>{{ movie.Plot }}</p>
    </div>
</template>

<style lang="scss">
$header-color: #42B883;
.movie-detail{
    padding: 16px;

    .poster{
        margin-bottom: 16px;
        display: flex;
        .pre-plot{
            padding-left: 1.5vw;
            background-color: rgba(0,0,0,0.4);
            .header{
                color: $header-color;
                text-decoration: underline;
            }
            p{
                font-size: 11pt;
                margin-bottom: 6px;
            }
        }

        .featured-img{
            display: block;
            max-width: 200px;
        }
    }

    h2{
        color: #FFF;
        font-size: 28px;
        font-weight: 600;
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
    console.log(resp.data);
});

</script>
