<template>
  <div class="dragndrop">
    <input
      id="file"
      type="file"
      name="files[]"
      multiple
      class="dragndrop__input"
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

  .dragndrop__input {
    display: none;
  }

  .dragndrop__header {
    display: block;
    font-size: 1.1em;
    color: #555;
    vertical-align: middle;
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
