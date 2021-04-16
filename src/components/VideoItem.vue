<template>
  <div @click="playVideo" :class="isLoaded ? 'video' : 'video video_hidden'">
    <img :src="image" :alt="title" class="video__cover">
    <div class="video__cover-gradient"/>
<!--    <div class="video__index">-->
<!--      <div class="video__title">-->
<!--        <span class="video__number">{{ num }}</span>-->
<!--      </div>-->
<!--    </div>-->
  </div>
</template>


<script>
export default {
  props: {
    num: Number,
    image: String,
    idLink: String,
    title: String,
    delay: Number
  },
  data() {
    return {
      isLoaded: false
    }
  },
  methods: {
    playVideo() {
      this.$emit('playNewVideo', this.idLink, this.title)
    }
  },
  mounted() {
    setTimeout(() => this.isLoaded = true, this.delay)
  }
}
</script>


<style lang="scss" scoped>
  .video.video_hidden {
    opacity: 0;
    transform: translateY(-15px);
    transition: ease-in .4s;
  }

  .video {
    position: relative;
    border-radius: 20px;
    height: 170px;
    margin: 10px;
    opacity: 1;
    transform: translateY(0);
    transition: .2s ease-in;

    &:hover {
      box-shadow: 0px 10px 25px 0px gray;
      cursor: pointer;
      transform: translateY(-2px);
      transition: .2s ease-in;
    }

    &__cover {
      width: 300px;
      height: 170px;
      border-radius: 20px;
      transition: .3s ease-in;
    }

    &__cover-gradient {
      position: absolute;
      top: 0;
      bottom: 0;
      width: 100%;
      height: 170px;
      border-radius: 20px;
      background-image: linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0));
      transition: .2s ease-in;
    }

    &__index {
      position: absolute;
      display: flex;
      flex-flow: column;
      align-items: center;
      top: 0;
      right: 0;
      width: 60px;
      height: 123px;
      border-radius: 0 20px 0 0;
      background: rgba(0, 0, 0, .5);
    }

    &__title {
      display: inherit;
      justify-content: center;
      align-items: center;
      position: relative;
      top: 15px;
      width: 40px;
      height: 40px;
      border: 1px solid #eee;
      box-sizing: border-box;
    }

    &__number {
      margin-top: 3px;
      font-size: 1.5rem;
      color: #eee;
      font-family: "CirceBold", sans-serif;
    }
  }
</style>