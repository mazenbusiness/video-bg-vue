<template>
  <div class="home" :class="{ loading: isLoading }">
    <div class="loader-container">
      <div class="loader"></div>
    </div>
    <div class="background-video-container">
      <video ref="backgroundVideo" loop muted preload="metadata">
        <source src="./assets/1sec.mp4" type="video/mp4" />
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
      isLoading: true,
    }
  },
  mounted() {
    window.addEventListener('scroll', this.onScroll)
    this.$refs.backgroundVideo.addEventListener('loadedmetadata', this.bgVideoLoaded)
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.onScroll)
  },
  computed: {
    viwportHeight() {
      return Math.max(document.documentElement.clientHeight || 0, window.innerHeight || 0)
    },
    pageHeight() {
      return document.documentElement.scrollHeight
    },
    scrollLength() {
      return this.pageHeight - this.viwportHeight
    },
  },
  methods: {
    bgVideoLoaded() {
      this.isLoading = false
    },
    onScroll() {
      this.scrollPosition = window.scrollY
      this.controlVideoPlayback()
    },
    controlVideoPlayback() {
      let percentage = 1 - (this.scrollLength - this.scrollPosition) / this.scrollLength
      let newTime = percentage * this.$refs.backgroundVideo.duration
      this.$refs.backgroundVideo.currentTime = newTime
    },
  },
}
</script>

<style scoped>
.loading {
  overflow: hidden;
}
.loading .loader-container {
  display: flex;
}
.loader-container {
  display: none;
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  justify-content: center;
  align-items: center;
  z-index: 10;
}
.loader {
  width: 48px;
  height: 48px;
  display: inline-block;
  position: relative;
  border: 3px solid;
  border-color: #de3500 #0000 #fff #0000;
  border-radius: 50%;
  box-sizing: border-box;
  animation: 1s rotate linear infinite;
}
.loader:before,
.loader:after {
  content: '';
  top: 0;
  left: 0;
  position: absolute;
  border: 10px solid transparent;
  border-bottom-color: #fff;
  transform: translate(-10px, 19px) rotate(-35deg);
}
.loader:after {
  border-color: #de3500 #0000 #0000 #0000;
  transform: translate(32px, 3px) rotate(-35deg);
}
@keyframes rotate {
  100% {
    transform: rotate(360deg);
  }
}

section {
  width: 100%;
  border: 1px solid #fff;
  border-radius: 15px;
  padding: 20px;
  margin-bottom: 20px;
}
section:not(:last-child) {
  margin-bottom: 20px;
}

.home {
  position: relative;
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
</style>
