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

<style scoped>
    div {
    background-color: white;
    padding: 18px;
    border-radius: 12px;
    margin-bottom: 20px;
    border: 1px solid #e5e7eb;
    }

    label {
    font-weight: bold;
    margin-right: 10px;
    }

    select {
    padding: 10px;
    border-radius: 8px;
    border: 1px solid #cbd5e1;
    }
</style>