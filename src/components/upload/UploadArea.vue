<template>
  <div 
    class="file-upload-area"
    @drop.prevent="handleDrop"
    @dragover.prevent
    @dragenter="isDragging = true"
    @dragleave="isDragging = false"
    @click="$refs.fileInput.click()"
  >
    <input 
      type="file" 
      ref="fileInput" 
      @change="handleFileSelect" 
      accept="image/*" 
      class="hidden"
    >
    <p class="upload-text">
      <span class="upload-icon">📁</span>
      クリックまたはドラッグ&ドロップで画像をアップロード
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDragging: false
    }
  },
  methods: {
    handleDrop(e) {
      this.isDragging = false
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

<style scoped>
.file-upload-area {
  border: 2px dashed #cbd5e0;
  border-radius: 8px;
  padding: 2rem;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  background-color: #f8fafc;
  min-height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.file-upload-area:hover {
  border-color: #4a90e2;
  background-color: #f0f7ff;
}

.file-upload-area[data-dragging="true"],
.file-upload-area.dragging {
  border-color: #4a90e2;
  background-color: #f0f7ff;
  transform: scale(1.02);
}

.upload-text {
  margin: 0;
  color: #4a5568;
  font-size: 1rem;
  line-height: 1.5;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.upload-icon {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.hidden {
  display: none;
}

/* アニメーション */
@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}

.file-upload-area:active {
  transform: scale(0.98);
}

/* ドラッグ中のスタイル */
.file-upload-area[data-dragging="true"] .upload-text,
.file-upload-area.dragging .upload-text {
  color: #4a90e2;
}

/* レスポンシブデザイン */
@media (max-width: 640px) {
  .file-upload-area {
    min-height: 150px;
    padding: 1.5rem;
  }

  .upload-text {
    font-size: 0.875rem;
  }

  .upload-icon {
    font-size: 1.5rem;
  }
}

/* アクセシビリティのための追加スタイル */
.file-upload-area:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.5);
}

/* ホバー時のインタラクション */
@media (hover: hover) {
  .file-upload-area:hover .upload-icon {
    animation: pulse 1s infinite;
  }
}
</style>