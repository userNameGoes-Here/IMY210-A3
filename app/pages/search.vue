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

<style scoped>
    main {
    width: 90%;
    max-width: 1100px;
    margin: 35px auto;
    }

    h1 {
    background-color: white;
    padding: 30px;
    border-radius: 16px;
    margin-bottom: 25px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.07);
    }

    form {
    background-color: white;
    padding: 18px;
    border-radius: 12px;
    display: flex;
    gap: 10px;
    border: 1px solid #e5e7eb;
    }

    input {
    flex: 1;
    padding: 12px;
    border: 1px solid #cbd5e1;
    border-radius: 8px;
    }

    button {
    background-color: #1f2937;
    color: white;
    border: none;
    padding: 12px 18px;
    border-radius: 8px;
    cursor: pointer;
    }

    button:hover {
    background-color: #374151;
    }

    .posts {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 22px;
    margin-top: 25px;
    }

    @media (max-width: 850px) {
    form {
        flex-direction: column;
    }

    .posts {
        grid-template-columns: 1fr;
    }
    }
</style>