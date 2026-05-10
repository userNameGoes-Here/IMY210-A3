<script setup>
    defineProps(['categories']);
    const emit = defineEmits(['filter']);

    function handleChange(event) {
        emit('filter', event.target.value)
    }
</script>

<template>
  <div>
    <label>Filter by category:</label>

    <select @change="handleChange">
      <option value="">All</option>

      <option v-for="category in categories" :key="category.id" :value="category.id">
        {{ category.name }}
      </option>
    </select>
  </div>
</template>