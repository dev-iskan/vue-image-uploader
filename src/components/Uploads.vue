<template>
  <div>
    <div
      v-if="files.length"
      class="dragndrop__status"
    >
      <ul class="list-inline">
        <li class="list-inline__item">
          Files: {{ files.length }}
        </li>
        <li class="list-inline__item">
          Percentage: x%
        </li>
        <li class="list-inline__item list-inline__item--last">
          Time remaining: 00:00
        </li>
      </ul>
    </div>
    <file
      v-for="file in files"
      :key="file.id"
      :file="file"
    />
  </div>
</template>

<script>
import File from './File'
export default {
  components: {
    File
  },
  props: {
    files: {
      required: true,
      type: Array
    }
  }
}
</script>

<style scoped>
  .dragndrop__status {
    text-align: center;
    padding: 20px;
  }
</style>
