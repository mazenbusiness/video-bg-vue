<template>
  <div>
    <div v-if="!isVideoLoaded" class="loading">Loading video...</div>
    <video ref="videoPlayer" controls preload="auto">
      <source src="./assets/Dark.mp4" type="video/mp4" />
      Your browser does not support the video tag.
    </video>
  </div>
</template>

<style>
.loading {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 1.5rem;
  color: white;
  background: rgba(0, 0, 0, 0.7);
  padding: 1rem;
  border-radius: 8px;
}
</style>
<script>
export default {
  data() {
    return {
      videoDuration: 0, // Total duration of the video
      isPlaying: false, // Track if the video is playing
      isVideoLoaded: false, // Track if the video is fully loaded
    }
  },
  mounted() {
    const video = this.$refs.videoPlayer

    // Wait for the video to be fully loaded
    video.addEventListener('canplaythrough', () => {
      this.isVideoLoaded = true
      this.videoDuration = video.duration // Get the video duration
      console.log('Video is fully loaded and ready to play.')
    })

    // Optional: Show loading state until the video is ready
    video.addEventListener('waiting', () => {
      console.log('Video is buffering...')
    })

    // Start tracking scroll only after the video is fully loaded
    if (this.isVideoLoaded) {
      window.addEventListener('scroll', this.handleScroll)
    }
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    handleScroll() {
      if (!this.isVideoLoaded) return // Do nothing if the video isn't fully loaded

      const video = this.$refs.videoPlayer
      const scrollPosition = window.scrollY
      const pageHeight = document.documentElement.scrollHeight - window.innerHeight

      // Calculate the scroll progress (0 to 1)
      const scrollProgress = scrollPosition / pageHeight

      // Map scroll progress to video time
      const videoTime = scrollProgress * this.videoDuration

      // Update the video's current time
      video.currentTime = videoTime

      // Play the video if it's not already playing
      if (!this.isPlaying) {
        video.play()
        this.isPlaying = true
      }

      // Pause the video when reaching the end of the page
      if (scrollPosition >= pageHeight) {
        video.pause()
      }
    },
  },
}
</script>
