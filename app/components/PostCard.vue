<script setup>
    defineProps(['post'])
</script>

<template>
    <div class="post-card">
        <h2>{{ post.title }}</h2>

        <p>
        <strong>Author:</strong>
        {{ post.author ? post.author.name : 'Unknown' }}
        </p>

        <p>{{ post.snippet }}</p>

        <NuxtLink :to="'/posts/' + post.slug">
        Read more
        </NuxtLink>
    </div>
</template>