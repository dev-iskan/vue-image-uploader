<template>
  <div id="app">
    <upload-form />
  </div>
</template>

<script>
import UploadForm from '@/components/UploadForm'
export default {
  name: 'App',
  components: {
    UploadForm
  }
}
</script>

<style>
  body {
    font-family: Arial, sans-serif
  }
</style>
