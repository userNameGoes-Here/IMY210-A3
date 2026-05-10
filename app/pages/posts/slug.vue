<script setup>
const API_URL = 'http://localhost:1337';
const route = useRoute();

const post = ref(null);
const loading = ref(false);
const error = ref('');

async function getPost() {
  loading.value = true;
  error.value = '';

  try {
    const url =
      API_URL +
      '/api/posts?populate=*' +
      '&filters[slug][$eq]=' +
      route.params.slug

    const response = await fetch(url);
    const result = await response.json();

    post.value = result.data[0];
  } 
  
  catch (err) {
    error.value = 'Failed to load post';
  }

  loading.value = false
}

function render(content) {
  if (!content) {
    return '';
  }

  return content
    .replace(/^# (.*$)/gim, '<h1>$1</h1>')
    .replace(/^## (.*$)/gim, '<h2>$1</h2>')
    .replace(/^### (.*$)/gim, '<h3>$1</h3>')
    .replace(/\*\*(.*?)\*\*/gim, '<strong>$1</strong>')
    .replace(/\*(.*?)\*/gim, '<em>$1</em>')
    .replace(/\n/gim, '<br>')
}

onMounted(function () {
  getPost()
})
</script>

<template>
    <main>
        <NuxtLink to="/">Back to posts</NuxtLink>

        <p v-if="loading">Loading...</p>
        <p v-if="error">{{ error }}</p>

        <div v-if="post">
            <h1>{{ post.title }}</h1>

            <p><strong>Author:</strong>{{ post.author ? post.author.name : 'Unknown' }}</p>

            <p v-if="post.category"><strong>Category:</strong>{{ post.category.name }}</p>

            <div v-html="render(post.content)"></div>
        </div>
  </main>
</template>