<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

const api_key = '7c63ecc439395d19ead94ec5353a34ff';
const base_url = 'https://api.themoviedb.org/3/movie';

const movieDetails = ref(null);
const cast = ref([]); 
const loading = ref(true);
const route = useRoute();

const getMovieDetails = () => {
    const movieId = route.params.id; 
    const url = `${base_url}/${movieId}?api_key=${api_key}`;

    axios
        .get(url)
        .then((response) => {
            const data = response.data;
            movieDetails.value = {
                title: data.title,
                overview: data.overview,
                poster_path: data.poster_path,
                release_date: data.release_date,
                genres: data.genres.map((genre) => genre.name).join(', '),
                vote_average: data.vote_average, 
            };
        });
};

const getMovieCast = () => {
    const movieId = route.params.id; 
    const url = `${base_url}/${movieId}/credits?api_key=${api_key}`;

    axios
        .get(url)
        .then((response) => {
            cast.value = response.data.cast.slice(0, 5).map((actor) => ({
                name: actor.name,
                character: actor.character,
                profile_path: actor.profile_path,
            }));
        });
};

onMounted(() => {
    Promise.all([getMovieDetails(), getMovieCast()]).finally(() => {
        loading.value = false;
    });
});
</script>

<template>
    <div class="container mt-4">
        <div v-if="loading" class="text-center">
            <p>Loading movie details...</p>
        </div>
        <div v-else>
            <div class="card">
                <img
                    :src="`https://image.tmdb.org/t/p/w500${movieDetails.poster_path}`"
                    alt="Movie Poster"
                    class="card-img-top"
                    style="max-width: 500px; margin: 0 auto;"
                />
                <div class="card-body">
                    <h1 class="card-title">{{ movieDetails.title }}</h1>
                    <p><strong>Genres:</strong> {{ movieDetails.genres }}</p>
                    <p><strong>Release Date:</strong> {{ movieDetails.release_date }}</p>
                    <p><strong>Rating:</strong> {{ movieDetails.vote_average }} / 10</p>
                    <p class="card-text">{{ movieDetails.overview }}</p>
                    <h3 class="mt-4">Cast</h3>
                    <ul class="list-group">
                        <li v-for="actor in cast" :key="actor.name" class="list-group-item d-flex align-items-center">
                            <img
                                v-if="actor.profile_path"
                                :src="`https://image.tmdb.org/t/p/w200${actor.profile_path}`"
                                alt="Actor Image"
                                class="rounded-circle me-3"
                                style="width: 40px; height: 40px;"
                            />
                            <div>
                                <strong>{{ actor.name }}</strong>
                                <p class="mb-0 text-muted">as {{ actor.character }}</p>
                            </div>
                        </li>
                    </ul>
                    <RouterLink class="btn btn-success mt-3" to="/movie">Back</RouterLink>
                </div>
            </div>
        </div>
    </div>
</template>