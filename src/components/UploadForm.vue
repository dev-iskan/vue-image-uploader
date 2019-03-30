<template>
  <div
    class="dragndrop"
    :class="{'dragndrop--dragged': isDraggedOver}"
    @dragover.prevent="enter"
    @dragenter.prevent="enter"
    @dragleave.prevent="leave"
    @dragend.prevent="leave"
    @drop.prevent="drop"
  >
    <input
      id="file"
      ref="input"
      type="file"
      name="files[]"
      multiple
      class="dragndrop__input"
      @change="select"
    >
    <label
      for="file"
      class="dragndrop__header"
    >
      <strong>Drag files here</strong> or click to select files
    </label>
  </div>
</template>

<script>
export default {
  data () {
    return {
      files: [],
      isDraggedOver: false
    }
  },
  methods: {
    enter () {
      this.isDraggedOver = true
    },

    leave () {
      this.isDraggedOver = false
    },

    drop (e) {
      this.leave()
      // add files
      this.addFiles(e.dataTransfer.files)
    },

    select (e) {
      // add files
      this.addFiles(this.$refs.input.files)
    },

    addFiles (files) {
      Object.keys(files).forEach(key => {
        const file = files[key]
        this.storeMeta(file)
          .then(fileObject => {
            // upload file

          }, fileObject => {
            // failed
            fileObject.failed = true
          })
      })
    },

    storeMeta (file) {
      // create file object
      const fileObject = this.generateFileObject(file)
      return new Promise((resolve, reject) => {
        this.$http.get('http://drag-n-drop.test/api/store')
          .then(res => {
          // set id
            fileObject.id = res.data.data.id
            // resolve
            resolve(fileObject)
          }, () => {
            // reject
            reject(fileObject)
          })
      })
    },

    generateFileObject (file) {
      const fileObjectIndex = this.files.push({
        id: null,
        file,
        progress: 0,
        failed: false,
        loadedBytes: 0,
        totalBytes: 0,
        timeStarted: (new Date()).getTime(),
        secondsRemaining: 0,
        finished: false,
        cancelled: false,
        xhr: null
      }) - 1

      return this.files[fileObjectIndex]
    }
  }
}
</script>

<style scoped>
  .dragndrop {
    --dragndrop-min-height: 200px;
    width: 100%;
    min-height: var(--dragndrop-min-height);
    background-color: #f8f8f8;
    position: relative;
    border: 3px dashed rgba(0,0,0, .2);
  }

  .dragndrop--dragged {
    border-color: #333;
  }

  .dragndrop__input {
    display: none;
  }

  .dragndrop__header {
    display: block;
    font-size: 1.1em;
    color: #555;
    text-align: center;
    margin: calc(var(--dragndrop-min-height) / 2) 20px;
  }

  .dragndrop__header:hover {
    text-decoration: underline;
    cursor: pointer;
  }

  .dragndrop__header--compact {
    margin: calc(var(--dragndrop-min-height) / 4) 20px;
  }
</style>
