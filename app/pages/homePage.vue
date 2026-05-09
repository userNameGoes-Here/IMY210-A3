<script setup>

</script>

<template>
    <main>
        <h1>Search</h1>

        <form @submit.prevent="searchPosts">
            <input v-model="searchTerm" type="text" placeholder="Search"/>

            <button type="submit">Search</button>
        </form>
    </main>
</template>