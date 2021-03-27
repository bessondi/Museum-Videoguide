<template>
  <div @click="playVideo" class="video">
    <img :src="image" :alt="title" class="video__cover">
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
    title: String
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
    position: relative;
    top: 1px;
    border-radius: 20px;
    margin: 10px;

    &__cover {
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