<template>
    <div>
      <div class="cropper-container">
        <vue-cropper
          ref="cropper"
          :img="imageUrl"
          :outputSize="option.size"
          :outputType="option.outputType"
        ></vue-cropper>
      </div>
      <div class="controls">
        <button @click="cropImage">クロップ</button>
      </div>
    </div>
  </template>
  
  <script>
  import { VueCropper } from 'vue-cropper'
  
  export default {
    components: {
      VueCropper
    },
    props: {
      imageUrl: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        option: {
          size: 1,
          outputType: 'jpeg'
        }
      }
    },
    methods: {
      cropImage() {
        this.$refs.cropper.getCropData((data) => {
          this.$emit('image-clipped', data)
        })
      }
    }
  }
  </script>
  
  <style scoped>
  .cropper-container {
    max-width: 600px;
    margin: 0 auto;
  }
  
  .controls {
    margin-top: 1rem;
    text-align: center;
  }
  </style>