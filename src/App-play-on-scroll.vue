<template>
  <div class="home">
    <div class="background-video-container">
      <video ref="backgroundVideo" autoplay loop muted>
        <source src="./assets/v3.mp4" type="video/mp4" />
        Your browser does not support the video tag.
      </video>
    </div>
    <div class="content">
      <h1>Welcome to the Homepage</h1>
      <p>Scroll down to see the effect!</p>
      <div class="scrollable-content">
        <!-- Content that users can scroll through -->
        <section v-for="n in 50" :key="n">section {{ n }}</section>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      scrollPosition: 0,
    }
  },
  mounted() {
    window.addEventListener('scroll', this.onScroll)
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.onScroll)
  },
  methods: {
    onScroll() {
      this.scrollPosition = window.scrollY
      this.controlVideoPlayback()
    },
    controlVideoPlayback() {
      const video = this.$refs.backgroundVideo
      // Play the video when the scroll position is above a certain threshold (e.g., 100px)
      if (this.scrollPosition > 100) {
        if (video.paused) {
          video.play()
        }
      } else {
        if (!video.paused) {
          video.pause()
        }
      }
    },
  },
}
</script>

<style scoped>
section {
  width: 100%;
  border: 1px solid #fff;
  border-radius: 15px;
  padding: 20px;
  margin-bottom: 20px;
}

.home {
  position: relative;
  height: 100vh;
  width: 100%;
}

.background-video-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.content {
  position: relative;
  z-index: 1;
  padding: 20px;
  color: white;
}

.scrollable-content {
  height: 2000px; /* Add a large height to make the page scrollable */
}
</style>
