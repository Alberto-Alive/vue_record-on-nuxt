<template>
  <v-container>
    <v-row>
      <v-spacer />
      <v-col>
        <span>Record ME</span>
        <VueRecordVideo
          @result="onResultVideo"
          @stream="toggleCamera"
          mode="press"
          class="text-right"
        />

        <video
          v-if="videos[0] && !isCameraOpen"
          :src="videos[videos.length - 1]"
          controls="controls"
        />

        <div
          v-if="isCameraOpen"
          v-show="!isLoading"
          class="camera-box"
          :class="{ flash: isShotPhoto }"
        >
          <video
            v-show="!isPhotoTaken"
            ref="camera"
            autoplay
          ></video>
        </div>

      </v-col>
      <v-spacer />
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      audios: [],
      videos: [],
      isCameraOpen: false,
      isPhotoTaken: false,
      isShotPhoto: false,
      isLoading: false,
      link: '#',
    }
  },

  methods: {
    onResultVideo(blob) {
      this.toggleCamera()
      this.videos.push(window.URL.createObjectURL(blob))
      this.videos = this.videos.map((value) => value)
      console.log(this.videos)
    },
    toggleCamera() {
      if (this.isCameraOpen) {
        this.isCameraOpen = false
        this.isPhotoTaken = false
        this.isShotPhoto = false
        this.stopCameraStream()
      } else {
        this.isCameraOpen = true
        this.createCameraElement()
      }
    },

    createCameraElement() {
      this.isLoading = true

      const constraints = (window.constraints = {
        audio: false,
        video: true,
      })

      navigator.mediaDevices
        .getUserMedia(constraints)
        .then((stream) => {
          this.isLoading = false
          this.$refs.camera.srcObject = stream
        })
        .catch((error) => {
          this.isLoading = false
          alert("May the browser didn't support or there are some errors.")
        })
    },

    stopCameraStream() {
      let tracks = this.$refs.camera.srcObject.getTracks()

      tracks.forEach((track) => {
        track.stop()
      })
    },

    takePhoto() {
      if (!this.isPhotoTaken) {
        this.isShotPhoto = true

        const FLASH_TIMEOUT = 50

        setTimeout(() => {
          this.isShotPhoto = false
        }, FLASH_TIMEOUT)
      }

      this.isPhotoTaken = !this.isPhotoTaken

      const context = this.$refs.canvas.getContext('2d')
      context.drawImage(this.$refs.camera, 0, 0, 450, 337.5)
    },

    downloadImage() {
      const download = document.getElementById('downloadPhoto')
      const canvas = document
        .getElementById('photoTaken')
        .toDataURL('image/jpeg')
        .replace('image/jpeg', 'image/octet-stream')
      download.setAttribute('href', canvas)
    },
  },
}
</script>
