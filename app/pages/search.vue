<script setup>
    const API_URL = 'http://localhost:1337';

    const searchTerm = ref('');
    const posts = ref([]);
    const loading = ref(false);
    const error = ref('');
    const searched = ref(false);

    async function searchPosts() {
        if (searchTerm.value === '') {
            posts.value = [];
            searched.value = false;
            return;
        }

        loading.value = true;
        error.value = '';
        searched.value = true;

        try {
            const url = API_URL +
            '/api/posts?populate=*' +
            '&filters[$or][0][title][$containsi]=' +
            searchTerm.value +
            '&filters[$or][1][author][name][$containsi]=' +
            searchTerm.value;

            const response = await fetch(url);
            const result = await response.json();

            posts.value = result.data
        } 
        
        catch (err) {
            error.value = 'Search failed';
        }

        loading.value = false;
    }
</script>

<template>
    <main>
        <h1>Search</h1>

        <form @submit.prevent="searchPosts">
            <input v-model="searchTerm" type="text" placeholder="Search by title or author"/>

            <button type="submit">Search</button>
        </form>

        <p v-if="loading">Searching...</p>
        <p v-if="error">{{ error }}</p>

        <div class="posts">
            <PostCard v-for="post in posts" :key="post.id" :post="post"/>
        </div>

        <p v-if="searched && posts.length === 0 && !loading">No results found.</p>
    </main>
</template>