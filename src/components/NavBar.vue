<template>
    <nav class="mb-8">
      <button 
        v-for="page in pages"
        :key="page.id"
        @click="$emit('change-page', page.id)"
        :class="[
          'px-4 py-2 mr-2 rounded',
          currentPage === page.id ? 'bg-blue-500 text-white' : 'bg-gray-200'
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