<template>
  <nav class="mb-8">
    <button 
      v-for="page in pages"
      :key="page.id"
      @click="$emit('change-page', page.id)"
      :class="[
        'px-4 py-2 mr-2 rounded',
        currentPage === page.id ? 'bg-blue-500 text-white' : 'bg-white text-gray-700 hover:bg-gray-200'
      ]"
    >
      {{ page.name }}
    </button>
  </nav>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      pages: [
        { id: 'upload', name: 'アップロード' },
        { id: 'gallery', name: 'ギャラリー' }
      ]
    }
  }
}
</script>

<style scoped>
nav {
  margin-bottom: 2rem; /* Adjusts the margin for the nav */
}

nav button {
  transition: background-color 0.3s, color 0.3s;
  padding: 0.5rem 1rem; /* Consistent padding */
  margin-right: 0.5rem; /* Space between buttons */
  border: none; /* Remove default border */
  border-radius: 0.25rem; /* Rounded corners */
  cursor: pointer; /* Pointer cursor on hover */
}

nav button.bg-white {
  background-color: white;
  color: #4a5568; /* Gray color for text */
}

nav button.bg-white:hover {
  background-color: #e5e7eb; /* Light gray on hover */
}

nav button.bg-blue-500 {
  background-color: #3b82f6; /* Blue background */
  color: white; /* White text */
}
</style>