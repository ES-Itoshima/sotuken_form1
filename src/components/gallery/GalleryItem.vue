<template>
    <div class="relative">
      <img :src="image" class="w-full h-48 object-cover rounded">
      <button 
        @click="$emit('delete')" 
        class="absolute top-2 right-2 bg-red-500 text-white px-2 py-1 rounded"
      >
        削除
      </button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      image: {
        type: String,
        required: true
      },
      index: {
        type: Number,
        required: true
      }
    }
  }
  </script>