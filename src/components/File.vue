<template>
  <div class="dragndrop__file">
    <div class="progress">
      <div class="progress__label">
        <span class="truncate">{{ file.file.name }}</span>
        <span v-if="!file.finished && !file.failed && !file.cancelled">({{ file.secondsRemaining }} seconds remaining)</span>
      </div>
      <div
        class="progress__fill"
        :style="{'width': file.progress + '%'}"
        :class="{'progress__fill--finished': file.finished,
                 'progress__fill--failed': file.failed || file.cancelled}"
      />
      <div class="progress_percentage">
        <span v-if="file.failed">Failed</span>
        <span v-if="file.finished">Completed</span>
        <span v-if="file.cancelled">Canceled</span>
        <span v-if="!file.finished && !file.failed && !file.cancelled">{{ file.progress }}%</span>
      </div>
    </div>
  </div>
</template>

<script>
import EventBus from '@/events'
export default {
  props: {
    file: {
      required: true,
      type: Object
    }
  },
  created () {
    EventBus.$on('progress', (fileObject, e) => {
      this.updateFileProgress(fileObject, e)
    })

    EventBus.$on('finished', (fileObject, e) => {
      if (fileObject.id === this.file.id) this.file.finished = true
    })

    EventBus.$on('failed', (fileObject, e) => {
      if (fileObject.id === this.file.id) this.file.failed = true
    })
  },
  methods: {
    updateFileProgress (fileObject, e) {
      if (!e.lengthComputable) {
        return false
      }

      fileObject.loadedBytes = e.loaded
      fileObject.totalBytes = e.total

      fileObject.progress = Math.ceil((e.loaded / e.total) * 100)
    }
  }
}
</script>

<style scoped>
  .dragndrop__file {
    margin: 20px;
    margin-top: 0;
    font-size: .9em;
    position: relative;
  }
  .progress {
    box-shadow: inset 0 1px 2px rgba(0, 0, 0, .1);
    border-radius: 3px;
    background-color: #f5f5f5;
    height: 35px;
  }

  .progress__label, .progress_percentage {
    position: absolute;
    color: #333;
    top: 50%;
    transform: translate(0, -50%);
    margin-left: 10px;
  }

  .progress_percentage {
    right: 0;
    margin-right: 10px;
  }

  .progress__fill {
    box-sizing: border-box;
    padding: 10px;
    border-radius: 3px;
    background-color: #42b983;
    width: 60%;
    height: 100%;
    box-shadow: inset 0 -1px rgba(0, 0, 0, .15);
    transition: width 500ms ease;
    opacity: .6;
  }

  .progress__fill--failed {
    transition: none;
    opacity: 1;
    width: 100%!important;
    background-color: #f66;
  }

  .progress__fill--finished {
    opacity: 1;
  }
</style>
