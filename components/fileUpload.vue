<template>
  <div>
    <div class="file-upload-container" 
      @dragenter="onDragenter"
      @dragover="onDragover"
      @dragleave="onDragleave"
      @drop="onDrop"
      @click="onClick"
    >
      <div class="file-upload">
        Drag & Drop Files
      </div>
    </div>
    <input type="file" ref="fileInput" class="file-upload-input" @onChange="onFileChange">
    <div class="file-upload-list">

    </div>
  </div>
</template>

<script>

export default {
  name: 'fileUpload',
  data () {
    return {
      files: [],
    }
  },
  methods: {
    onClick (event) {
      this.$refs.fileInput.click()
    },
    onDragenter (event) {
      // class 넣기
      event.target.classList.add('dragged')
    },
    onDragleave (event) {
      // class 삭제
      event.target.classList.remove('dragged')
    },
    onDragover (event) {
      // 드롭을 허용하도록 prevetDefault() 호출
      event.preventDefault()
    },
    onDrop (event) {
      // 기본 액션을 막음 (링크 열기같은 것들)
      event.preventDefault()
      event.target.classList.remove('dragged')
      // event.dataTransfer.files
    },
    onFileChange (event) {
      // read
      const files = event.target.files
      console.log(files)
    },
    readFiles (file) {
      
    }
  }
}
</script>
<style lang="scss">

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
    background-color: grey;
  }

  &-container {
    width: 500px;
    height: 300px;
    padding: 20px;
    margin: 0 auto;
    border: 1px solid powderblue;
    border-radius: 20px;
  }
}

</style>