<script setup>
    const API_URL = 'http://localhost:1337'

    const posts = ref([])
    const categories = ref([])
    const loading = ref(false)
    const error = ref('')
    const selectedCategory = ref('')

    async function getPosts() {
        loading.value = true
        error.value = ''

        try {
            const url = API_URL + '/api/posts?populate=*';

            if(selectedCategory.value !== '') {
            url = url + '&filters[category][id][$eq]=' + selectedCategory.value
            }

            const response = await fetch(url);
            const result = await response.json();

            posts.value = result.data
        } 
        
        catch (err) {
            error.value = 'Failed to load content';
        }

        loading.value = false;
        }

    async function getCategories() {
        try {
            const response = await fetch(API_URL + '/api/categories')
            const result = await response.json()

            categories.value = result.data;
        } 
        
        catch (err) {
            console.log(err);
        }
    }

    function filterPosts(categoryId) {
        selectedCategory.value = categoryId
        getPosts();
        }

        onMounted(function () {
            getPosts();
            getCategories();
    })

</script>

<template>
    <main>
        <h1>Posts</h1>


    </main>
</template>