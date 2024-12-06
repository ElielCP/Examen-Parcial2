<script setup>
import { ref } from 'vue';
import { RouterLink } from 'vue-router';

const api_key = '7c63ecc439395d19ead94ec5353a34ff';
const base_url = `https://api.themoviedb.org/3/tv/popular?api_key=${api_key}`;

const popularTVShows = ref([]);

const getPopularTVShows = async () => {
    const resp = await fetch(base_url);
    const data = await resp.json();
    popularTVShows.value = data.results.map((tv) => {
        return {
            id: tv.id,
            name: tv.name,
            overview: tv.overview,
            poster_path: tv.poster_path,
        };
    });
};

getPopularTVShows();
</script>

<template>
    <h3 class="display-6 text-dark text-center fw-bold shadow-sm">Popular TV Shows</h3>
    <div class="row">
        <div v-for="tv in popularTVShows" :key="tv.id" class="col-md-3 mb-4">
            <div class="card">
                <img
                    v-if="tv.poster_path"
                    :src="`https://image.tmdb.org/t/p/w200${tv.poster_path}`"
                    class="card-img-top"
                    alt="TV Show Poster"
                />
                <div class="card-body text-center">
                    <h5 class="card-title">{{ tv.name }}</h5>
                    <RouterLink :to="`/tv/${tv.id}`" class="btn btn-primary mt-2">View Details</RouterLink>
                </div>
            </div>
        </div>
    </div>
</template>

