<template>
  <div class="container">
    <div class="file-upload-container" 
      @dragenter="onDragenter"
      @dragover="onDragover"
      @dragleave="onDragleave"
      @drop="onDrop"
      @click="onClick"
    >
      <div class="file-upload" :class="isDragged ? 'dragged' : ''">
        Drag & Drop Files
      </div>
    </div>
    <!-- 파일 업로드 -->
    <input type="file" ref="fileInput" class="file-upload-input" @change="onFileChange" multiple>
    <!-- 업로드된 리스트 -->
    <div class="file-upload-list">
      <div class="file-upload-list__item" v-for="(file, index) in fileList" :key="index">
        <div class="file-upload-list__item__data">
          <img class="file-upload-list__item__data-thumbnail" :src="file.src">
          <div class="file-upload-list__item__data-name">
            {{ file.name }}
          </div>
        </div>
        <div class="file-upload-list__item__btn-remove" @click="handleRemove(index)">
          삭제
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'fileUpload',
  data () {
    return {
      fileList: [],
      isDragged: false
    }
  },
  methods: {
    onClick () {
      this.$refs.fileInput.click()
    },
    onDragenter (event) {
      // class 넣기
      this.isDragged = true
    },
    onDragleave (event) {
      // class 삭제
      this.isDragged = false
    },
    onDragover (event) {
      // 드롭을 허용하도록 prevetDefault() 호출
      event.preventDefault()
    },
    onDrop (event) {
      // 기본 액션을 막음 (링크 열기같은 것들)
      event.preventDefault()
      this.isDragged = false
      const files = event.dataTransfer.files
      this.addFiles(files)
    },
    onFileChange (event) {
      const files = event.target.files
      this.addFiles(files)
    },
    async addFiles (files) {
      for(let i = 0; i < files.length; i++) {
        const src = await this.readFiles(files[i])
        files[i].src = src
        this.fileList.push(files[i])
      }
    },
    async readFiles (files) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader()
        reader.onload = async (e) => {
          resolve(e.target.result) 
        }
        reader.readAsDataURL(files)
      })
    },
    handleRemove (index) {
      this.fileList.splice(index, 1)
    }
  }
}
</script>
<style lang="scss">
.container {
  min-height: 300px;
  width: 500px;
  margin: 0 auto;
}
.file-upload {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  border: transparent;
  border-radius: 20px;
  cursor: pointer;
  &.dragged {
    border: 1px dashed powderblue;
    opacity: .6;
  }
  &-container {
    height: 300px;
    padding: 20px;
    margin: 0 auto;
    box-shadow: 0 0.625rem 1.25rem #0000001a;
    border-radius: 20px;
  }
  &-input {
    display: none;
    /* position: absolute;
    top: 0;
    left: 0;
    visibility: hidden; */
  }
  &-list {
    margin-top: 10px;
    width: 100%;
    &__item {
      padding: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      &__data {
        display: flex;
        align-items: center;
        &-thumbnail {
          margin-right: 10px;
          border-radius: 20px;
          overflow: hidden;
          width: 120px;
          height: 120px;
          z-index: 10;
        }
      }
      &__btn-remove {
        cursor: pointer;
        border: 1px solid powderblue;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 5px;
        border-radius: 6px;
      }
    }
  }
}
</style>