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
              @click="resetForm"
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

        <div class="mt-8">
          <h4 class="text-lg font-medium mb-4">保存済みアノテーション</h4>
          <div v-if="savedAnnotations.length > 0">
            <div
              v-for="(note, index) in savedAnnotations"
              :key="index"
              class="border-b py-4"
            >
              <div class="flex justify-between items-start">
                <div>
                  <p class="text-sm text-gray-600 mb-1">ページ: {{ note.page }}</p>
                  <p class="whitespace-pre-wrap">{{ note.content }}</p>
                  <p class="text-sm text-gray-500 mt-2">
                    {{ formatDate(note.createdAt) }}
                  </p>
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
      required: true
    }
  },
  data() {
    return {
      annotation: {
        page: '',
        content: ''
      }
    }
  },
  computed: {
    savedAnnotations() {
      const annotations = JSON.parse(localStorage.getItem('bookAnnotations') || '{}')
      return annotations[this.bookId] || []
    }
  },
  methods: {
    saveAnnotation() {
      const annotations = JSON.parse(localStorage.getItem('bookAnnotations') || '{}')
      if (!annotations[this.bookId]) {
        annotations[this.bookId] = []
      }
      
      annotations[this.bookId].push({
        page: this.annotation.page,
        content: this.annotation.content,
        createdAt: new Date().toISOString()
      })
      
      localStorage.setItem('bookAnnotations', JSON.stringify(annotations))
      this.resetForm()
    },
    deleteAnnotation(index) {
      if (confirm('このアノテーションを削除してもよろしいですか？')) {
        const annotations = JSON.parse(localStorage.getItem('bookAnnotations') || '{}')
        annotations[this.bookId].splice(index, 1)
        localStorage.setItem('bookAnnotations', JSON.stringify(annotations))
      }
    },
    resetForm() {
      this.annotation.page = ''
      this.annotation.content = ''
    },
    formatDate(dateString) {
      return new Date(dateString).toLocaleString('ja-JP')
    }
  }
}
</script>