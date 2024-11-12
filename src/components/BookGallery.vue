<!-- src/components/BookGallery.vue -->
<template>
  <div class="book-gallery">
    <h2 class="text-2xl mb-4">書籍ギャラリー</h2>
    
    <div v-if="books.length === 0" class="text-center py-8">
      保存された書籍がありません
    </div>
    
    <div v-else class="overflow-x-auto">
      <table class="min-w-full border-collapse">
        <thead>
          <tr class="bg-gray-100">
            <th class="py-3 px-4 text-left border-b w-32">画像</th>
            <th class="py-3 px-4 text-left border-b">タイトル</th>
            <th class="py-3 px-4 text-left border-b">著者</th>
            <th class="py-3 px-4 text-left border-b">出版社</th>
            <th class="py-3 px-4 text-left border-b">ISBN</th>
            <th class="py-3 px-4 text-left border-b">説明</th>
            <th class="py-3 px-4 text-left border-b w-36">操作</th>
          </tr>
        </thead>
        <tbody>
          <tr 
            v-for="(book, index) in books" 
            :key="index"
            class="hover:bg-gray-50 border-b"
          >
            <td class="py-2 px-4">
              <img 
                :src="book.imageUrl" 
                :alt="book.title"
                class="thumbnail-image rounded shadow cursor-pointer"
                @click="showLargeImage(book.imageUrl)"
              >
            </td>
            <td class="py-2 px-4">
              <div class="font-medium">{{ book.title }}</div>
            </td>
            <td class="py-2 px-4">{{ book.author }}</td>
            <td class="py-2 px-4">{{ book.publisher }}</td>
            <td class="py-2 px-4">
              <span class="font-mono">{{ book.isbn }}</span>
            </td>
            <td class="py-2 px-4">
              <div class="max-w-xs">
                <div class="line-clamp-2">{{ book.description || '説明なし' }}</div>
                <button 
                  v-if="book.description && book.description.length > 100"
                  @click="showFullDescription(book.description)"
                  class="text-blue-500 text-sm mt-1 hover:underline"
                >
                  続きを読む
                </button>
              </div>
            </td>
            <td class="py-2 px-4">
              <div class="flex space-x-2">
                <button
                  @click="$emit('delete-book', index)"
                  class="delete-button"
                >
                  削除
                </button>
                <button 
                  @click="$emit('show-annotation', index)"
                  class="annotation-button"
                    >
                  アノテーション
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>

      <!-- モーダル: 大きい画像表示 -->
      <div 
        v-if="largeImageUrl" 
        class="modal-overlay"
        @click="largeImageUrl = null"
      >
        <div class="modal-content">
          <img 
            :src="largeImageUrl" 
            class="modal-image"
            alt="拡大画像"
          >
        </div>
      </div>

      <!-- モーダル: 説明全文表示 -->
      <div 
        v-if="fullDescription" 
        class="modal-overlay"
        @click="fullDescription = null"
      >
        <div class="description-modal">
          <h3 class="modal-title">説明</h3>
          <p class="description-text">{{ fullDescription }}</p>
          <button 
            class="close-button"
            @click="fullDescription = null"
          >
            閉じる
          </button>
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
  },
  data() {
    return {
      largeImageUrl: null,
      fullDescription: null
    }
  },
  methods: {
    showLargeImage(imageUrl) {
      this.largeImageUrl = imageUrl
    },
    showFullDescription(description) {
      this.fullDescription = description
    }
  }
}
</script>

<style scoped>
/* 基本スタイル */
.book-gallery {
  margin: 0 auto;
  padding: 20px;
}

/* テーブルスタイル */
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 12px 16px;
  text-align: left;
  border-bottom: 1px solid #e2e8f0;
}

th {
  background-color: #f7fafc;
  font-weight: 600;
}

tr:hover {
  background-color: #f8fafc;
}

/* 画像スタイル */
.thumbnail-image {
  width: 300px;
  height: 150px;
  object-fit: cover;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.2s;
}

.thumbnail-image:hover {
  transform: scale(1.02);
}

/* モーダルスタイル */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 50;
}

.modal-content {
  padding: 16px;
  background-color: white;
  border-radius: 8px;
}

.modal-image {
  width: 300px;
  height: 400px;
  object-fit: contain;
  background-color: white;
}

.description-modal {
  background-color: white;
  border-radius: 8px;
  padding: 24px;
  max-width: 42rem;
  max-height: 90vh;
  overflow-y: auto;
  margin: 16px;
}

.modal-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 16px;
}

.description-text {
  white-space: pre-wrap;
}

/* ボタンスタイル */
.delete-button {
  padding: 8px 12px;
  background-color: #ef4444;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.delete-button:hover {
  background-color: #dc2626;
}

.close-button {
  margin-top: 16px;
  padding: 8px 16px;
  background-color: #e5e7eb;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.close-button:hover {
  background-color: #d1d5db;
}

/* テキスト省略 */
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* レスポンシブ対応 */
.overflow-x-auto {
  overflow-x: auto;
}

/* その他のユーティリティクラス */
.text-2xl {
  font-size: 1.5rem;
  line-height: 2rem;
}

.mb-4 {
  margin-bottom: 1rem;
}

.max-w-xs {
  max-width: 20rem;
}

.font-medium {
  font-weight: 500;
}

.font-mono {
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
}

.text-blue-500 {
  color: #3b82f6;
}

.text-sm {
  font-size: 0.875rem;
  line-height: 1.25rem;
}

.mt-1 {
  margin-top: 0.25rem;
}

.hover\:underline:hover {
  text-decoration: underline;
}

.annotation-button {
  padding: 8px 12px;
  background-color: #3b82f6;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
  text-decoration: none;
  font-size: 0.875rem;
  display: inline-block;
}

.annotation-button:hover {
  background-color: #2563eb;
}
</style>