<template>
  <div @click="changeVisibility" class="player">
    <span class="title">{{videoTitle}}</span>
    <youtube
        ref="youtube"
        :video-id="videoId"
        :player-vars="playerVars"
        :width="frameWidth"
        :height="frameHeight"
        class="youtubeFrame"
    />
    <button @click="changeVisibility" class="closePlayerBtn">&times;</button>
  </div>
</template>

<script>
export default {
  props: {
    videoId: String,
    videoTitle: String
  },
  data() {
    return {
      windowWidth: document.documentElement.clientWidth,
      playerVars: {
        autoplay: 1
      }
    }
  },
  methods: {
    playVideo() {
      this.player.playVideo()
    },
    changeVisibility() {
      this.$emit('hide')
    },
  },
  computed: {
    player() {
      return this.$refs.youtube.player
    },
    frameWidth() {
      if (this.windowWidth >= 1024) {
        return 865
      } else if (this.windowWidth >= 768 && this.windowWidth < 1024) {
        return 640
      } else return  320
    },
    frameHeight() {
      if (this.windowWidth >= 1024) {
        return 500
      } else if (this.windowWidth >= 768 && this.windowWidth < 1024) {
        return 360
      } else return 185
    },
  }
}
</script>

<style lang="scss" scoped>
  .player {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.8);
    z-index: 10;

    .title {
      color: #fff;
      font-size: 3rem;
      margin-top: -50px;
      margin-bottom: 50px;
      text-align: center;
      padding: 0 40px;

      @media screen and (max-width: 768px) {
        font-size: 1.5rem;
      }
    }

    .closePlayerBtn {
      position: fixed;
      top: 25px;
      right: 25px;
      outline: none;
      border: none;
      border-radius: 50%;
      width: 50px;
      height: 50px;
      color: white;
      font-size: 3rem;
      background: none;

      &:hover {
        cursor: pointer;
        color: lightgray;
      }
    }
  }
</style>
