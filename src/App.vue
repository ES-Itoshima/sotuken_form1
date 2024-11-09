<template>
  <div class="container mx-auto p-4">
    <nav-bar 
      :current-page="currentPage" 
      @change-page="currentPage = $event"
    />
    
    <upload-page 
      v-if="currentPage === 'upload'"
      @image-saved="handleImageSaved"
    />
    
    <gallery-page 
      v-if="currentPage === 'gallery'"
      :images="images"
      @delete-image="deleteImage"
    />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import UploadPage from './components/UploadPage.vue'
import GalleryPage from './components/GalleryPage.vue'

export default {
  components: {
    NavBar,
    UploadPage,
    GalleryPage
  },
  data() {
    return {
      currentPage: 'upload',
      images: []
    }
  },
  mounted() {
    const savedImages = localStorage.getItem('savedImages')
    if (savedImages) {
      this.images = JSON.parse(savedImages)
    }
  },
  methods: {
    handleImageSaved(imageData) {
      this.images.push(imageData)
      localStorage.setItem('savedImages', JSON.stringify(this.images))
      this.currentPage = 'gallery'
    },
    deleteImage(index) {
      this.images.splice(index, 1)
      localStorage.setItem('savedImages', JSON.stringify(this.images))
    }
  }
}
</script>