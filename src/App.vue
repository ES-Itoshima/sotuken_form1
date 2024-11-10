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
    />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import UploadFlow from './components/UploadFlow.vue'
import BookGallery from './components/BookGallery.vue'

export default {
  components: {
    NavBar,
    UploadFlow,
    BookGallery
  },
  data() {
    return {
      currentPage: 'upload',
      books: []
    }
  },
  mounted() {
    const savedBooks = localStorage.getItem('savedBooks')
    if (savedBooks) {
      this.books = JSON.parse(savedBooks)
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
      this.books.splice(index, 1)
      localStorage.setItem('savedBooks', JSON.stringify(this.books))
    }
  }
}
</script>