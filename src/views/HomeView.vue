<template>
  <div class="home">
    <div class="feature-card">
        <router-link to="/movie/tt0409591">
            <p class="banner">Featured</p>
            <img src="https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_FMjpg_UX1000_.jpg" 
            alt="Naruto poster" class="feature-img">
            <div class="detail">
                <h3>Naruto</h3>
                <p>Naruto Uzumaki, a mischievous adolescent ninja, struggles as he searches for
                     recognition and dreams of becoming the Hokage, the village's 
                     leader and strongest ninja.</p>
                <span class="screen-size-bigger">
                    <br><br><br>
                    <p>2002-2007</p><br>
                    <p>TV-PG</p>
                </span>
            </div>
        </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
        <input type="text" placeholder="Type in movie to search for" v-model="search">
        <input type="submit" value="Search">
    </form>

    <div class="movies-list">
        <div class="movie" v-for="movie in movies" :key="movie.imdbID">
            <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
                <div class="product-image">
                    <img :src="movie.Poster" alt="Movie poster" loading="lazy"/>
                    <div class="type">{{ movie.Type }}</div>
                </div>
                <div class="detail">
                    <p class="year">{{ movie.Year }}</p>
                    <h3>{{ movie.Title }}</h3>
                </div>
            </router-link>
        </div>
    </div>
  </div>
</template>

<script setup>

import axios from 'axios';
import { ref } from 'vue';
import env from '@/env.js'

const search = ref('');
const movies = ref([]);

const SearchMovies = async () => {
    if(search.value !== ''){ 
        const resp = await axios.get(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`);
        movies.value = resp.data.Search;
        search.value = '';
    }
};


</script>

<style lang="scss">
// Global scss variable
$screen-width: 1200px;
//
.home{
    width: 100vw;
    display: flex;
    flex-direction: column;
    .feature-card{
        position: relative;

        .banner{
            position: absolute;
            z-index: 99;
            width: fit-content;
            padding: 10px;
            background-color: white;
        }

        .feature-img{
            display: block;
            width: 100%;
            height: 300px;
            object-fit: cover;
            position: relative;
            z-index: 0;
        }

        .detail{
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0,0,0,0.6);
            padding: 16px;
            z-index: 1;

            h3{
                color: #FFF;
                margin-bottom: 16px;
            }

            p{
                color: #FFF;
            }
            .screen-size-bigger{
                display: none;
            }
        }
    }

    @media (min-width: $screen-width) {
        .feature-card{
            display: flex;
            width: 70%;
            left: 15%;

            & > a{
                display: flex;
            }

            .feature-img{
                height: 450px;
                width: 80%;
                object-fit: cover;
            }

            .detail{
                position: relative;

                h3{
                    font-size: 20pt;
                }
                p{
                    font-size: 15pt;
                }
                .screen-size-bigger{
                    display: block;
                    font-size: 20pt;
                }
            }
        }
    }

    .search-box{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 16px;

        input{
            display: block;
            appearance: none;
            border: none;
            outline: none;
            background: none;

            &[type="text"]{
                width: 100%;
                color: #FFF;
                background-color: #496583;
                font-size: 20px;
                padding: 10px 16px;
                border-radius: 8px;
                margin-bottom: 15px;
                transition: 0.4s;

                &::placeholder{
                    color: #f3f3f3;
                }

                &:focus{
                    box-shadow: 0px 3px 6px rgba(0,0,0, 0.2);
                }
            }

            &[type="submit"]{
                width: 100%;
                max-width: 300px;
                background-color: #42B883;
                padding: 16px;
                border-radius: 8px;
                color: #FFF;
                font-size: 20px;
                text-transform: uppercase;
                transition: 0.4s;

                &:active{
                    background-color: #3B8070;
                }
            }
        }
        

    }
    @media (min-width: $screen-width) {
        .search-box{
            display: flex;
            flex-direction: row;
            gap: 10%;


            input{
    
                &[type="text"]{
                    width: 40%;
                    text-align: center;
                    margin: 0;
                }

                &[type="submit"]{
                    margin: 0;
                    padding: 10px 16px;
                }
            }
        }
    }

    .movies-list{
        display: flex;
        flex-wrap: wrap;
        margin: 0px 8px;
        justify-content: space-around;

        .movie{
            width: 30%;
            max-width: 300px;
            flex: 1 1 50%;
            padding: 16px 8px;
            
            .movie-link{
                display: flex;
                flex-direction: column;
                height: 100%;

                .product-image{
                    position: relative;
                    display: block;

                    img{
                        display: block;
                        width: 100%;
                        height: 275px;
                        object-fit: cover;
                    }

                    .type{
                        position: absolute;
                        padding: 8px 16px;
                        background-color: #42B883;
                        color: #FFF;
                        bottom: 16px;
                        left: 0px;
                        text-transform: capitalize;
                    }
                }

                .detail{
                    background-color: #496583;
                    padding: 16px 8px;
                    flex: 1 1 100%;
                    border-radius: 0px 0px 8px 8px; 

                    .year{
                        color: #AAA;
                        font-size: 14px;
                    }

                    h3{
                        color: #FFF;
                        font-weight: 600;
                        font-size: 18px;
                    }
                }
            }
        }
    }
}

</style>
