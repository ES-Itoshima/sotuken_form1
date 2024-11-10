<template>
    <div class="upload-flow">
      <!-- Step 1: 画像アップロード -->
      <div v-if="currentStep === 'upload'">
        <h2 class="text-2xl mb-4">画像アップロード</h2>
        <upload-area @file-selected="handleFileSelect" />
      </div>
  
      <!-- Step 2: 画像プレビューと書籍情報入力 -->
      <div v-if="currentStep === 'info'" class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <h3 class="text-xl mb-2">プレビュー</h3>
          <img :src="bookData.imageUrl" class="max-w-full rounded shadow">
        </div>
        
        <book-form 
          :book-data="bookData"
          @update="updateBookData"
          @submit="handleSubmit"
        />
      </div>
    </div>
  </template>
  
  <script>
  import UploadArea from './upload/UploadArea.vue'
  import BookForm from './book/BookForm.vue'
  import { defaultBookInfo } from '../types/book'
  
  export default {
    components: {
      UploadArea,
      BookForm
    },
    data() {
      return {
        currentStep: 'upload',
        bookData: { ...defaultBookInfo }
      }
    },
    methods: {
      handleFileSelect(file) {
        const reader = new FileReader()
        reader.onload = (e) => {
          this.bookData.imageUrl = e.target.result
          this.currentStep = 'info'
        }
        reader.readAsDataURL(file)
      },
      updateBookData(data) {
        this.bookData = { ...this.bookData, ...data }
      },
      handleSubmit() {
        this.$emit('book-saved', this.bookData)
        this.bookData = { ...defaultBookInfo }
        this.currentStep = 'upload'
      }
    }
  }
  </script>