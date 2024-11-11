<template>
    <div>
      <div class="image-container">
        <img :src="clippedImageData" alt="Clipped Image" />
        <div
          v-for="annotation in annotations"
          :key="annotation.id"
          class="annotation"
          :style="annotationStyle(annotation)"
        >
          <span class="annotation-text">{{ annotation.text }}</span>
        </div>
      </div>
      <div class="controls">
        <input v-model="annotationText" placeholder="アノテーションを入力" />
        <button @click="addAnnotation">アノテーションを追加</button>
        <button @click="showAnnotations = !showAnnotations">
          {{ showAnnotations ? '非表示' : '表示' }}
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      clippedImageData: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        annotationText: '',
        annotations: [],
        showAnnotations: true,
        nextAnnotationId: 1
      }
    },
    methods: {
      addAnnotation() {
        if (this.annotationText.trim() !== '') {
          this.annotations.push({
            id: this.nextAnnotationId++,
            text: this.annotationText,
            x: 10,
            y: 10,
            width: 100,
            height: 50
          })
          this.annotationText = ''
        }
      },
      annotationStyle(annotation) {
        return {
          left: `${annotation.x}px`,
          top: `${annotation.y}px`,
          width: `${annotation.width}px`,
          height: `${annotation.height}px`,
          display: this.showAnnotations ? 'block' : 'none'
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .image-container {
    position: relative;
    max-width: 800px;
    margin: 0 auto;
  }
  
  .image-container img {
    max-width: 100%;
    height: auto;
  }
  
  .annotation {
    position: absolute;
    background-color: rgba(255, 255, 0, 0.5);
    padding: 4px 8px;
    border-radius: 4px;
    cursor: move;
  }
  
  .annotation-text {
    font-size: 14px;
    font-weight: bold;
  }
  
  .controls {
    margin-top: 1rem;
    text-align: center;
  }
  </style>