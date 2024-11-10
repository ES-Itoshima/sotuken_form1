
<template>
    <div class="book-form">
      <h3 class="text-xl mb-4">書籍情報入力</h3>
      <form @submit.prevent="handleSubmit" class="space-y-4">
        <div>
          <label class="block text-sm font-medium mb-1">タイトル *</label>
          <input
            v-model="localData.title"
            type="text"
            required
            class="w-full p-2 border rounded"
          >
        </div>
  
        <div>
          <label class="block text-sm font-medium mb-1">著者 *</label>
          <input
            v-model="localData.author"
            type="text"
            required
            class="w-full p-2 border rounded"
          >
        </div>
  
        <div>
          <label class="block text-sm font-medium mb-1">出版社</label>
          <input
            v-model="localData.publisher"
            type="text"
            class="w-full p-2 border rounded"
          >
        </div>
  
        <div>
          <label class="block text-sm font-medium mb-1">出版年</label>
          <input
            v-model="localData.publishedYear"
            type="number"
            class="w-full p-2 border rounded"
          >
        </div>
  
        <div>
          <label class="block text-sm font-medium mb-1">ISBN</label>
          <input
            v-model="localData.isbn"
            type="text"
            class="w-full p-2 border rounded"
          >
        </div>
  
        <div>
          <label class="block text-sm font-medium mb-1">説明</label>
          <textarea
            v-model="localData.description"
            rows="3"
            class="w-full p-2 border rounded"
          ></textarea>
        </div>
  
        <div class="flex justify-end space-x-2">
          <button
            type="submit"
            class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
          >
            保存
          </button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      bookData: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        localData: { ...this.bookData }
      }
    },
    watch: {
      localData: {
        deep: true,
        handler(newValue) {
          this.$emit('update', newValue)
        }
      }
    },
    methods: {
      handleSubmit() {
        this.$emit('submit')
      }
    }
  }
  </script>