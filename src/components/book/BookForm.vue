<template>
  <div class="book-form bg-white p-6 rounded-lg shadow-md">
    <h3 class="text-2xl font-bold mb-4">書籍情報入力</h3>
    <form @submit.prevent="handleSubmit" class="space-y-4">
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">タイトル *</label>
        <input
          v-model="localData.title"
          type="text"
          required
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">著者 *</label>
        <input
          v-model="localData.author"
          type="text"
          required
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">出版社</label>
        <input
          v-model="localData.publisher"
          type="text"
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">出版年</label>
        <input
          v-model="localData.publishedYear"
          type="number"
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">ISBN</label>
        <input
          v-model="localData.isbn"
          type="text"
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">説明</label>
        <textarea
          v-model="localData.description"
          rows="3"
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        ></textarea>
      </div>

      <div class="flex justify-end">
        <button
          type="submit"
          class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
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

<style>
.book-form {
  background-color: #fff;
  padding: 1.5rem;
  border-radius: 0.5rem;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.book-form h3 {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.book-form .space-y-4 > * {
  margin-bottom: 1rem;
}

.book-form label {
  display: block;
  font-size: 0.875rem;
  font-weight: medium;
  color: #4a5568;
  margin-bottom: 0.25rem;
}

.book-form input,
.book-form textarea {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.375rem;
  outline: none;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.book-form input:focus,
.book-form textarea:focus {
  border-color: #4299e1;
  box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.5);
}

.book-form button {
  display: block;
  margin-left: auto;
  padding: 0.5rem 1rem;
  background-color: #4299e1;
  color: #fff;
  border-radius: 0.375rem;
  transition: background-color 0.15s ease-in-out;
}

.book-form button:hover {
  background-color: #3182ce;
}
</style>