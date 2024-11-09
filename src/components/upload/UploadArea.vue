<template>
    <div 
      class="border-dashed border-2 border-gray-300 p-8 text-center cursor-pointer"
      @drop.prevent="handleDrop"
      @dragover.prevent
      @click="$refs.fileInput.click()"
    >
      <input 
        type="file" 
        ref="fileInput" 
        @change="handleFileSelect" 
        accept="image/*" 
        class="hidden"
      >
      <p>クリックまたはドラッグ&ドロップで画像をアップロード</p>
    </div>
  </template>
  
  <script>
  export default {
    methods: {
      handleDrop(e) {
        const file = e.dataTransfer.files[0]
        if (file && file.type.startsWith('image/')) {
          this.$emit('file-selected', file)
        }
      },
      handleFileSelect(e) {
        const file = e.target.files[0]
        if (file) {
          this.$emit('file-selected', file)
        }
      }
    }
  }
  </script>