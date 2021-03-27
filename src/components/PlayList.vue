<template>
  <div class="playList">
    <Loader v-if="!isDataLoaded" />

    <VideoItem v-else
       v-for="(video, i) in videos"
       :key="video.id"
       :num="video.id"
       :idLink="video.key"
       :image="video.img"
       :title="video.title"
       :delay="delay + (i * 200) / 2"
       @playNewVideo="setVideo"
    />
  </div>
</template>


<script>
import Loader from "@/components/Loader";
import VideoItem from "@/components/VideoItem";

export default {
  props: {
    videos: Array
  },
  data() {
    return {
      delay: 500,
      isDataLoaded: false
    }
  },
  components: {
    Loader,
    VideoItem
  },
  methods: {
    setVideo(key, title) {
      this.$emit('setNewVideo', key, title)
    }
  },
  mounted() {
    this.videos ? setTimeout(() => this.isDataLoaded = true, this.delay) : null
  }
}
</script>


<style lang="scss" scoped>
  .playList {
    display: flex;
    flex-flow: wrap;
    justify-content: space-around;
    margin: auto;
    max-width: 1000px;
    width: 100%;
    height: 100%;
  }
</style>