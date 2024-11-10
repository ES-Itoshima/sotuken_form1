<!-- src/components/BookGallery.vue -->
<template>
    <div class="book-gallery">
      <h2 class="text-2xl mb-4">書籍ギャラリー</h2>
      
      <div v-if="books.length === 0" class="text-center py-8">
        保存された書籍がありません
      </div>
      
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <div
          v-for="(book, index) in books"
          :key="index"
          class="border rounded-lg overflow-hidden shadow-lg"
        >
          <img
            :src="book.imageUrl"
            :alt="book.title"
            class="w-full h-48 object-cover"
          >
          <div class="p-4">
            <h3 class="text-lg font-semibold mb-2">{{ book.title }}</h3>
            <p class="text-gray-600">{{ book.author }}</p>
            <p class="text-sm text-gray-500">{{ book.publisher }}・{{ book.publishedYear }}</p>
            
            <div class="mt-4 flex justify-end">
              <button
                @click="$emit('delete-book', index)"
                class="px-3 py-1 bg-red-500 text-white rounded hover:bg-red-600"
              >
                削除
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      books: {
        type: Array,
        required: true
      }
    }
  }
  </script>