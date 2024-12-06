<script setup>
    defineProps({
        id: Number,
        title: String,
        body:String
    })    
</script>

<template>
    <div>
        <hr>
        <div class="row">
            <div class="col">
                <h3 class="text-primary text-dark">{{ id }}: {{  title }}</h3>
                <p>{{ body }}</p>
            </div>
        </div>
    </div>
</template>
