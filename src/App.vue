<template>
  <div class="container mx-auto p-4">
    <nav-bar 
      :current-page="currentPage" 
      @change-page="changePage"
    />
    
    <upload-flow 
      v-if="currentPage === 'upload'"
      @book-saved="handleBookSaved"
    />
    
    <book-gallery 
      v-if="currentPage === 'gallery'"
      :books="books"
      @delete-book="deleteBook"
      @show-annotation="handleShowAnnotation"
    />

    <book-annotation
      v-if="currentPage === 'annotation'"
      :book-id="selectedBookId"
      @back="currentPage = 'gallery'"
    />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import UploadFlow from './components/UploadFlow.vue'
import BookGallery from './components/BookGallery.vue'
import BookAnnotation from './components/annotator/BookAnnotation.vue'

export default {
  components: {
    NavBar,
    UploadFlow,
    BookGallery,
    BookAnnotation
  },
  data() {
    return {
      currentPage: 'upload',
      books: [],
      selectedBookId: null,
      annotations: {} // 書籍ごとのアノテーションを管理
    }
  },
  mounted() {
    // 書籍データの読み込み
    const savedBooks = localStorage.getItem('savedBooks')
    if (savedBooks) {
      this.books = JSON.parse(savedBooks)
    }
    
    // アノテーションデータの読み込み
    const savedAnnotations = localStorage.getItem('bookAnnotations')
    if (savedAnnotations) {
      this.annotations = JSON.parse(savedAnnotations)
    }
  },
  methods: {
    changePage(page) {
      this.currentPage = page
    },
    handleBookSaved(bookData) {
      this.books.push({
        ...bookData,
        createdAt: new Date().toISOString()
      })
      localStorage.setItem('savedBooks', JSON.stringify(this.books))
      this.currentPage = 'gallery'
    },
    deleteBook(index) {
      // 書籍の削除時に関連するアノテーションも削除
      delete this.annotations[this.books[index].id]
      this.books.splice(index, 1)
      localStorage.setItem('savedBooks', JSON.stringify(this.books))
      localStorage.setItem('bookAnnotations', JSON.stringify(this.annotations))
    },
    handleShowAnnotation(bookId) {
      this.selectedBookId = bookId
      this.currentPage = 'annotation'
    },
    // アノテーション関連のメソッドを追加
    saveAnnotation(bookId, annotation) {
      if (!this.annotations[bookId]) {
        this.annotations[bookId] = []
      }
      this.annotations[bookId].push({
        ...annotation,
        createdAt: new Date().toISOString()
      })
      localStorage.setItem('bookAnnotations', JSON.stringify(this.annotations))
    },
    deleteAnnotation(bookId, index) {
      this.annotations[bookId].splice(index, 1)
      localStorage.setItem('bookAnnotations', JSON.stringify(this.annotations))
    }
  }
}
</script>