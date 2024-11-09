<template>
    <div class="mt-4">
      <h3 class="text-xl mb-2">プレビュー</h3>
      <img :src="previewUrl" class="max-w-md">
      <button 
        @click="$emit('save')" 
        class="mt-4 px-4 py-2 bg-green-500 text-white rounded"
      >
        保存
      </button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      previewUrl: {
        type: String,
        required: true
      }
    }
  }
  </script>