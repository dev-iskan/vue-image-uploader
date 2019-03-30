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
      console.log(e.dataTransfer.files)
    },

    select (e) {
      // add files
      console.log(this.$refs.input.files)
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
