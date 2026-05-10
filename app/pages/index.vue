<script setup>
    const API_URL = 'http://localhost:1337'

    const posts = ref([])
    const categories = ref([])
    const selectedCategory = ref('')
    const loading = ref(false)
    const error = ref('')

    async function getPosts() {
        loading.value = true
        error.value = ''

        try {
            const response = await fetch(API_URL + '/api/posts?populate=*')
            const result = await response.json()

            posts.value = result.data
        } catch (err) {
            error.value = 'Could not load posts'
        }

        loading.value = false
    }

    async function getCategories() {
        try {
            const response = await fetch(API_URL + '/api/categories')
            const result = await response.json()

            categories.value = result.data
        } catch (err) {
            console.log(err)
        }
        }

    const filteredPosts = computed(function () {
    if (selectedCategory.value === '') {
        return posts.value
    }

    return posts.value.filter(function (post) {
        if (!post.category) {
        return false
        }

        return String(post.category.id) === String(selectedCategory.value)
    })
    })

    function filterPosts(categoryId) {
    selectedCategory.value = categoryId
    }

    onMounted(function () {
    getPosts()
    getCategories()
    })
</script>

<template>
  <main>
    <h1>Posts</h1>

    <CategoryFilter :categories="categories" @filter="filterPosts"/>

    <p v-if="loading">Loading...</p>
    <p v-if="error">{{ error }}</p>

    <div class="posts">
      <PostCard v-for="post in filteredPosts" :key="post.id" :post="post" class="post"/>
    </div>

    <p v-if="!loading && filteredPosts.length === 0">No posts found for this category.</p>
  </main>
</template>

<style scoped>
    main {
        width: 90%;
        max-width: 1100px;
        margin: 35px auto;
    }

    h1 {
        background-color: rgb(235, 235, 235);
        padding: 30px;
        border-radius: 16px;
        margin-bottom: 25px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.07);
    }

    .posts {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 22px;
        margin-top: 25px;
    }

    .post:hover {
        scale: 1.1;
        background-color: rgb(207, 255, 229);
    }

    p {
        font-size: 16px;
    }

    @media (max-width: 850px) {
        .posts {
            grid-template-columns: 1fr;
        }
    }
</style>