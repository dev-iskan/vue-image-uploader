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
          Percentage: {{ overallProgress }}%
        </li>
        <li class="list-inline__item list-inline__item--last">
          Time remaining: {{ timeRemaining }}
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
import EventBus from '@/events'
import File from './File'
import { setInterval, clearInterval } from 'timers'
import timeremaining from '@/helpers/timeRemaining'
import pad from '@/helpers/pad'
export default {
  components: {
    File
  },
  props: {
    files: {
      required: true,
      type: Array
    }
  },
  data () {
    return {
      overallProgress: 0,
      interval: null,
      secondsRemaining: 0,
      timeRemaining: 'Calculating'
    }
  },
  created () {
    EventBus.$on('progress', (fileObject, e) => {
      this.updateOverallProgress(fileObject, e)
    })
    EventBus.$on('init', () => {
      if (!this.interval) {
        this.interval = setInterval(() => {
          if (this.unfinishedFiles().length === 0) {
            this.updateOverallProgress()
            clearInterval(this.interval)
            this.interval = null
          }
          this.updateTimeRemaining()
        }, 1000)
      }
    })
  },
  methods: {
    updateOverallProgress (fileObject, e) {
      const unfinished = this.unfinishedFiles()
      let totalProgress = 0
      // for each unfinished file
      unfinished.forEach(file => {
        // total progress
        totalProgress += file.progress
      })
      // total progress / unfinished file count
      // otherwise 0
      this.overallProgress = parseInt(totalProgress / unfinished.length || 0)
    },

    updateTimeRemaining () {
      this.secondsRemaining = 0
      this.unfinishedFiles().forEach(file => {
        file.secondsRemaining = timeremaining.calculate(file.totalBytes, file.loadedBytes, file.timeStarted)
        this.secondsRemaining += file.secondsRemaining
      })
      const minutes = Math.floor(this.secondsRemaining / 60)
      const seconds = this.secondsRemaining - minutes * 60

      this.timeRemaining = pad.left('00', minutes) + ':' + pad.left('00', seconds)
    },

    unfinishedFiles () {
      let files = []
      this.files.forEach(file => {
        if (file.finished || file.cancelled) {
          return false
        }

        files.push(file)
      })
      return files
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
