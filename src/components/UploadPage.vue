<template>
    <div class="upload-page">
      <h2 class="text-2xl mb-4">画像アップロード</h2>
      <upload-area
        @file-selected="handleFileSelect"
      />
      <image-preview
        v-if="previewUrl"
        :preview-url="previewUrl"
        @save="saveImage"
      />
    </div>
  </template>
  
  <script>
  import UploadArea from './upload/UploadArea.vue'
  import ImagePreview from './upload/ImagePreview.vue'
  
  export default {
    components: {
      UploadArea,
      ImagePreview
    },
    data() {
      return {
        previewUrl: null,
        currentFile: null
      }
    },
    methods: {
      handleFileSelect(file) {
        this.currentFile = file
        const reader = new FileReader()
        reader.onload = (e) => {
          this.previewUrl = e.target.result
        }
        reader.readAsDataURL(file)
      },
      saveImage() {
        if (this.previewUrl) {
          this.$emit('image-saved', this.previewUrl)
          this.previewUrl = null
          this.currentFile = null
        }
      }
    }
  }
  </script>