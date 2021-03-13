<template>
  <div class="playList">
    <VideoItem
       v-for="video in videos"
       :key="video.id"
       :idLink="video.key"
       :image="video.img"
       @playNewVideo="setVideo"
    />
  </div>
</template>


<script>
import VideoItem from "@/components/VideoItem";

export default {
  props: {
    videos: Array
  },
  components: {
    VideoItem
  },
  methods: {
    setVideo(key) {
      this.$emit('setNewVideo', key)
    }
  }
}
</script>


<style lang="scss" scoped>
  .playList {
    display: flex;
    flex-flow: wrap;
    justify-content: space-around;
    margin: 50px auto;
    max-width: 1000px;
    width: 100%;
    height: 100%;
  }
</style>