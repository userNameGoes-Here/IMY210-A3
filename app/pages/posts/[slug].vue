<script setup>
  const API_URL = 'http://localhost:1337'
  const route = useRoute()

  const post = ref(null)
  const loading = ref(false)
  const error = ref('')

  async function getPost() {
    loading.value = true
    error.value = ''

    try {
      const slug = route.params.slug

      const url =
        API_URL +
        '/api/posts?populate=*' +
        '&filters[slug][$eq]=' +
        slug

      console.log('Fetching:', url)

      const response = await fetch(url)

      if (!response.ok) {
        throw new Error('Request failed')
      }

      const result = await response.json()

      console.log(result)

      if (!result.data || result.data.length === 0) {
        throw new Error('No post found with this slug')
      }

      post.value = result.data[0]
    } 
    
    catch (err) {
      error.value = err.message
    } 
    
    finally {
      loading.value = false
    }
  }

  onMounted(function () {
    getPost()
  })
</script>

<template>
  <main>
    <NuxtLink to="/"><img src="https://img.icons8.com/?size=100&id=11511&format=png&color=000000" title="back"/></NuxtLink>

    <p v-if="loading">Loading...</p>

    <p v-if="error" class="error">
      {{ error }}
    </p>

    <div v-if="post" id="post">
      <div class="header">
        <h1>{{ post.title }}</h1>

        <p>
          <strong>Author:</strong>
          {{ post.author ? post.author.name : 'Unknown' }}
        </p>

        <p v-if="post.category">
          <strong>Category:</strong>
          {{ post.category.name }}
        </p>
      </div>

      <div class="content">
        <div v-for="block in post.content" :key="block.id">
          
          <p v-if="block.type === 'paragraph'">
            <span v-for="child in block.children" :key="child.text">
              {{ child.text }}
            </span>
          </p>

          <h2 v-if="block.type === 'heading'">
            <span v-for="child in block.children" :key="child.text">
              {{ child.text }}
            </span>
          </h2>

        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    width: 90%;
    max-width: 900px;
    margin: 35px auto;
    background: white;
    padding: 30px;
    border-radius: 16px;
  }

  .error {
    color: red;
    font-weight: bold;
  }

  a {
    display: inline-block;
    margin-bottom: 20px;
  }

  #post {
    background-color: rgb(229, 229, 229);
    border-radius: 15px 15px 15px 15px;
    padding: 15px 35px 15px 35px;
  }
  
  .header {
     background-color: rgb(179, 179, 179);
  }
</style>
