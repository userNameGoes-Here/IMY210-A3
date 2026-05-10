<script setup>
    const API_URL = 'http://localhost:1337'

    const title = ref('')
    const slug = ref('')
    const snippet = ref('')
    const content = ref('')
    const author = ref('')
    const category = ref('')

    const message = ref('')
    const error = ref('')
    const loading = ref(false)

    async function createPost() {
    loading.value = true
    message.value = ''
    error.value = ''

    try {
        const response = await fetch(API_URL + '/api/posts', {
        method: 'POST',

        headers: {
            'Content-Type': 'application/json'
        },

        body: JSON.stringify({
            data: {
            title: title.value,
            slug: slug.value,
            snippet: snippet.value,

            content: [
                {
                type: 'paragraph',
                children: [
                    {
                    text: content.value
                    }
                ]
                }
            ],

            author: Number(author.value),
            category: Number(category.value)
            }
        })
        })

        if (!response.ok) {
        throw new Error('Failed to create post')
        }

        message.value = 'Post uploaded successfully'

        title.value = ''
        slug.value = ''
        snippet.value = ''
        content.value = ''
        author.value = ''
        category.value = ''
    }

    catch (err) {
        error.value = err.message
    }

    loading.value = false
    }
</script>

<template>
  <main>
    <h1>Create Blog Post</h1>

    <form @submit.prevent="createPost">

      <input v-model="title" type="text" placeholder="Title" required/>

      <input v-model="slug" type="text" placeholder="Slug" required/>

      <textarea v-model="snippet" placeholder="Snippet" rows="3" required></textarea>

      <textarea v-model="content" placeholder="Content" rows="100" required></textarea>

      <input v-model="author" type="number" placeholder="Author ID" required/>

      <input v-model="category" type="number" placeholder="Category ID" required/>

      <button type="submit">Upload Post</button>

    </form>

    <p v-if="loading">Uploading...</p>

    <p v-if="message" class="success">{{ message }}</p>

    <p v-if="error" class="error">{{ error }}</p>
  </main>
</template>

<style scoped>
main {
  width: 90%;
  max-width: 800px;
  margin: 40px auto;
  background: white;
  padding: 30px;
  border-radius: 16px;
  box-shadow: 0 4px 14px rgba(0,0,0,0.08);
}

h1 {
  margin-top: 0;
}

form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

input,
textarea {
  padding: 12px;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-size: 16px;
}

button {
  background-color: #1f2937;
  color: white;
  border: none;
  padding: 14px;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background-color: #374151;
}

.success {
  color: green;
  font-weight: bold;
}

.error {
  color: red;
  font-weight: bold;
}
</style>