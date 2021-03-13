<template>
  <div @click="playVideo" class="video">
    <img :src="image" alt="Video cover" class="cover">
  </div>
</template>


<script>
export default {
  props: {
    image: String,
    idLink: String,
  },
  methods: {
    playVideo() {
      this.$emit('playNewVideo', this.idLink)
    }
  }
}
</script>


<style lang="scss" scoped>
  .video {
    border-radius: 20px;
    margin: 10px;

    .cover {
      width: 300px;
      height: 170px;
      border-radius: 20px;
      transition: 0.2s linear;

      &:hover {
        transform: translate(0px, -1px);
        box-shadow: 0px 10px 25px 0px gray;
        cursor: pointer;
      }
    }
  }
</style>