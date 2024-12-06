<script setup>
import { ref } from 'vue';
import { RouterLink } from 'vue-router';

const api_key = '7c63ecc439395d19ead94ec5353a34ff';
const base_url = `https://api.themoviedb.org/3/movie/now_playing?api_key=${api_key}`;

const nowplaying = ref([]);
const getNowPlayingMovies = async () => {
    const resp = await fetch(base_url);
    const data = await resp.json();
    nowplaying.value = data.results.map((movie) => {
        return {
            id: movie.id,
            title: movie.title,
            overview: movie.overview,
            poster_path: movie.poster_path,
        };
    });
};
getNowPlayingMovies();
</script>

<template>
    <h3 class="display-6 text-dark text-center fw-bold shadow-sm">List of Now Playing Movies</h3>

    <div class="row">
        <div v-for="m in nowplaying" :key="m.id" class="col-md-3 mb-4">
            <div class="card">
                <img
                    v-if="m.poster_path"
                    :src="`https://image.tmdb.org/t/p/w200${m.poster_path}`"
                    class="card-img-top"
                    alt="Movie Poster"
                />
                <div class="card-body text-center">
                    <h5 class="card-title">{{ m.title }}</h5>
                    <RouterLink :to="`/movie/${m.id}`" class="btn btn-primary mt-2">View Details</RouterLink>
                </div>
            </div>
        </div>
    </div>
</template>