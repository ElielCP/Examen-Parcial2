<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';


const api_key = '7c63ecc439395d19ead94ec5353a34ff';
const base_url = 'https://api.themoviedb.org/3/tv';

const tvDetails = ref(null);
const loading = ref(true);
const route = useRoute();

const getTvDetails = () => {
    const tvId = route.params.id;
    const url = `${base_url}/${tvId}?api_key=${api_key}`;

    axios
        .get(url)
        .then((response) => {
            const data = response.data;
            tvDetails.value = {
                name: data.name,
                overview: data.overview,
                poster_path: data.poster_path,
                first_air_date: data.first_air_date,
                genres: data.genres.map((genre) => genre.name).join(', '),
                vote_average: data.vote_average,
                seasons: data.number_of_seasons,
                episodes: data.number_of_episodes,
            };
        })
        .finally(() => {
            loading.value = false;
        });
};

onMounted(() => {
    getTvDetails();
});
</script>

<template>
    <div class="container mt-4">
        <div v-if="loading" class="text-center">
            <p>Loading TV show details...</p>
        </div>
        <div v-else>
            <div class="card">
                <img
                    :src="`https://image.tmdb.org/t/p/w500${tvDetails.poster_path}`"
                    alt="TV Show Poster"
                    class="card-img-top"
                     style="max-width: 500px; margin: 0 auto;"
                />
                <div class="card-body">
                    <h1 class="card-title">{{ tvDetails.name }}</h1>
                    <p><strong>Genres:</strong> {{ tvDetails.genres }}</p>
                    <p><strong>First Air Date:</strong> {{ tvDetails.first_air_date }}</p>
                    <p><strong>Rating:</strong> {{ tvDetails.vote_average }} / 10</p>
                    <p><strong>Seasons:</strong> {{ tvDetails.seasons }}</p>
                    <p><strong>Episodes:</strong> {{ tvDetails.episodes }}</p>
                    <p class="card-text">{{ tvDetails.overview }}</p>
                    <RouterLink class="btn btn-success mt-3" to="/tv">Back</RouterLink>
                </div>
            </div>
        </div>
    </div>
</template>