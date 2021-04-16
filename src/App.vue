<template>
  <div id="app">
    <div class="title">
      <img :src="handsLogo" alt="hands logo" class="title__logo">
      <h1 class="title__heading">Видеогид РЖЯ Музея Фаберже</h1>
    </div>

    <Player
        v-if="isPlayerOpen"
        :videoId="currentVideo"
        :videoTitle="currentVideoTitle"
        @hide="hidePlayer"
    />

    <PlayList
        :videos="videos"
        @setNewVideo="setNewVideoToPlayer"
    />
  </div>
</template>


<script>
import Player from './components/Player.vue'
import PlayList from './components/PlayList.vue'
import handsLogo from './assets/svg/hands.svg'

export default {
  data() {
    return {
      handsLogo,
      currentVideo: null,
      currentVideoTitle: null,
      isPlayerOpen: false,
      videos: [],
      youTubeData: null,
    }
  },
  components: {
    Player,
    PlayList
  },
  methods: {
    setNewVideoToPlayer(key, title) {
      this.currentVideoTitle = title
      this.currentVideo = key
      this.isPlayerOpen = true
    },
    hidePlayer() {
      this.isPlayerOpen = false
    },
    async getDataFromYT() {
      const YOUTUBE_PLAYLIST_ITEMS_API = 'https://www.googleapis.com/youtube/v3/playlistItems'
      const YOUTUBE_PLAYLIST = 'PLBjThViczbwqRPoX01nNR3vppD1nCy_7c'
      const YOUTUBE_API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY

      const firstPart = await fetch(`${YOUTUBE_PLAYLIST_ITEMS_API}?key=${YOUTUBE_API_KEY}&playlistId=${YOUTUBE_PLAYLIST}&part=snippet&maxResults=50&pageToken=CDIQAQ`)
      const secondPart = await fetch(`${YOUTUBE_PLAYLIST_ITEMS_API}?key=${YOUTUBE_API_KEY}&playlistId=${YOUTUBE_PLAYLIST}&part=snippet&maxResults=38&pageToken=CDIQAA`)
      const data1 = await firstPart.json()
      const data2 = await secondPart.json()

      const youTubeData = [...data1.items, ...data2.items]
      // console.log(data1)

      const covers = await youTubeData.map((c, i) => {
        return {
          id: i + 1,
          key: c.snippet.resourceId.videoId,
          img: c.snippet.thumbnails.medium.url,
          title: c.snippet.title
        }
      })
      this.videos.push(...covers)
    }
  },
  mounted() {
    this.getDataFromYT()
  }
}
</script>


<style lang="scss">
@font-face {
  font-family: "PlayfairDisplay";
  src: url("./assets/fonts/PlayfairDisplay-Regular.ttf") format('truetype');
}

@font-face {
  font-family: "CirceLight";
  src: url("./assets/fonts/Circe-Light.ttf") format('truetype');
}

@font-face {
  font-family: "CirceBold";
  src: url("./assets/fonts/Circe-Bold.ttf") format('truetype');
}

body {
  margin: 0;
  padding: 0;
  background: #fff;
}

#app {
  font-family: PlayfairDisplay, CirceLight, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.title {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 50px 0;
  animation: showTitle 1s ease;

  @keyframes showTitle {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }

  @media screen and (max-width: 768px) {
    margin: 20px 0;
  }

  &__logo {
    width: 150px;
    height: 150px;
    margin: 10px 0;
  }

  &__heading {
    text-align: center;
    font-size: 3rem;
    color: #000;
    padding: 0 20px;

    @media screen and (max-width: 768px) {
      font-size: 2.5rem;
    }
  }
}

</style>