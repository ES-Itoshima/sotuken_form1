<template>
    <div class="book-annotation">
      <div class="max-w-4xl mx-auto px-4 py-8">
        <div class="flex items-center justify-between mb-6">
          <h2 class="text-2xl font-semibold">アノテーション追加</h2>
          <button 
          class="back-button"
          @click="$emit('back')"
        >
          戻る
        </button>

        </div>
  
        <!-- アノテーション入力フォーム -->
        <div class="bg-white rounded-lg shadow-lg p-6">
          <form @submit.prevent="saveAnnotation" class="space-y-6">
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-2">
                ページ番号
              </label>
              <input
                v-model="annotation.page"
                type="number"
                min="1"
                class="w-32 border rounded-md px-3 py-2"
                required
              >
            </div>
  
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-2">
                アノテーション内容
              </label>
              <textarea
                v-model="annotation.content"
                rows="4"
                class="w-full border rounded-md px-3 py-2"
                required
              ></textarea>
            </div>
  
            <div class="flex justify-end space-x-4">
              <button
                type="button"
                class="cancel-button"
                @click="$router.push('/')"
              >
                キャンセル
              </button>
              <button
                type="submit"
                class="save-button"
              >
                保存
              </button>
            </div>
          </form>
  
          <!-- アノテーション一覧 -->
          <div class="mt-8">
            <h4 class="text-lg font-medium mb-4">保存済みアノテーション</h4>
            <div v-if="annotations.length > 0">
              <div
                v-for="(note, index) in annotations"
                :key="index"
                class="border-b py-4"
              >
                <div class="flex justify-between items-start">
                  <div>
                    <p class="text-sm text-gray-600 mb-1">ページ: {{ note.page }}</p>
                    <p class="whitespace-pre-wrap">{{ note.content }}</p>
                  </div>
                  <button
                    @click="deleteAnnotation(index)"
                    class="text-red-500 hover:text-red-700"
                  >
                    削除
                  </button>
                </div>
              </div>
            </div>
            <div v-else class="text-gray-500 text-center py-4">
              アノテーションはまだありません
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
export default {
  name: 'BookAnnotation',
  props: {
    bookId: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      annotations: [],
      annotation: {
        page: '',
        content: ''
      }
    }
  },
  methods: {
    saveAnnotation() {
      this.annotations.push({
        page: this.annotation.page,
        content: this.annotation.content,
        createdAt: new Date().toISOString()
      })

      this.annotation.page = ''
      this.annotation.content = ''
    },
    deleteAnnotation(index) {
      if (confirm('このアノテーションを削除してもよろしいですか？')) {
        this.annotations.splice(index, 1)
      }
    }
  }
}
</script>
  
  <style scoped>
  .back-button {
    padding: 8px 16px;
    background-color: #e5e7eb;
    border-radius: 4px;
    transition: background-color 0.2s;
    text-decoration: none;
    color: #374151;
  }
  
  .back-button:hover {
    background-color: #d1d5db;
  }
  
  .save-button {
    padding: 8px 16px;
    background-color: #3b82f6;
    color: white;
    border-radius: 4px;
    transition: background-color 0.2s;
  }
  
  .save-button:hover {
    background-color: #2563eb;
  }
  
  .cancel-button {
    padding: 8px 16px;
    background-color: #e5e7eb;
    color: #374151;
    border-radius: 4px;
    transition: background-color 0.2s;
  }
  
  .cancel-button:hover {
    background-color: #d1d5db;
  }
  
  input, textarea {
    border-color: #e2e8f0;
  }
  
  input:focus, textarea:focus {
    outline: none;
    border-color: #3b82f6;
    ring: 2px;
    ring-color: #93c5fd;
  }
  </style>