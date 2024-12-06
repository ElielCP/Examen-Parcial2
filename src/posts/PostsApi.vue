<script setup>
import { ref } from 'vue';
import PostCard from './components/PostCard.vue';

const data = ref([])

const getPosts = async () => {
    try {
        const resp = await fetch('https://jsonplaceholder.typicode.com/posts');
        const posts = await resp.json();
        data.value = posts
    } catch (error) {
        console.log(error);
    } finally {

    }
}

const valor = 10;
const inicio = ref(0)
const fin = ref(10)

const nextPosts = () => {
    inicio.value = inicio.value + valor;
    fin.value = fin.value + valor;
}
const backPosts = () => {
    inicio.value = inicio.value - valor;
    fin.value = fin.value - valor;
}

getPosts()
</script>

<template>
    <h3 class="display-6 text-dark text-center fw-bold shadow-sm">Home Page of Post</h3>
    
    <button class="btn btn-primary me-3" 
        @click="backPosts"
        :disabled=" inicio <=0 ? true: false "
        >
        Back
    </button>
    <button class="btn btn-primary" 
        v-on:click="nextPosts" 
        :disabled="fin >= 100 ? true : false">
        Next
    </button>

    <hr>

    <PostCard v-for="p in data.slice(inicio, fin)" :key="p.id" :id="p.id" :title="p.title" :body="p.body">
    </PostCard>
</template>
