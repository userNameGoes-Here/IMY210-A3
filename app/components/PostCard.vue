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

        <NuxtLink :to="'/posts/' + post.slug">Read more</NuxtLink>
    </div>
</template>


<style scoped>
    .post-card {
    background-color: white;
    padding: 22px;
    border-radius: 16px;
    border: 1px solid #e5e7eb;
    box-shadow: 0 4px 14px rgba(0, 0, 0, 0.06);
    }

    .post-card h2 {
    margin-top: 0;
    color: #1f2937;
    }

    .post-card p {
    color: #4b5563;
    line-height: 1.5;
    }

    a {
    display: inline-block;
    margin-top: 10px;
    background-color: #1f2937;
    color: white;
    padding: 10px 14px;
    border-radius: 8px;
    text-decoration: none;
    }

    a:hover {
    background-color: #374151;
    }
</style>