<template>
    <div class="gallery-page">
      <h2 class="text-2xl mb-4">画像ギャラリー</h2>
      <div v-if="images.length === 0" class="text-center py-8">
        保存された画像がありません
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <gallery-item
          v-for="(image, index) in images"
          :key="index"
          :image="image"
          :index="index"
          @delete="$emit('delete-image', index)"
        />
      </div>
    </div>
  </template>
  
  <script>
  import GalleryItem from './gallery/GalleryItem.vue'
  
  export default {
    components: {
      GalleryItem
    },
    props: {
      images: {
        type: Array,
        required: true
      }
    }
  }
  </script>