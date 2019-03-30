<template>
  <div>
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

</style>
