<template>
  <div id="app">
    <h1 class="title">Видеогид РЖЯ Музея Фаберже</h1>
    <Player
        v-if="isPlayerOpen"
        :videoId="currentVideo"
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

export default {
  data() {
    return {
      currentVideo: null,
      isPlayerOpen: false,
      videos: [],
      youTubeData: null
    }
  },
  components: {
    Player,
    PlayList
  },
  methods: {
    setNewVideoToPlayer(key) {
      this.currentVideo = key
      this.isPlayerOpen = true
    },
    hidePlayer() {
      this.isPlayerOpen = false
    },
    async getDataFromYT() {
      const YOUTUBE_PLAYLIST_ITEMS_API = 'https://www.googleapis.com/youtube/v3/playlistItems'
      const YOUTUBE_PLAYLIST = 'PLBjThViczbwqRPoX01nNR3vppD1nCy_7c'

      console.log(process.env)
      const firstPart = await fetch(`${YOUTUBE_PLAYLIST_ITEMS_API}?key=${process.env.YOUTUBE_API_KEY}&playlistId=${YOUTUBE_PLAYLIST}&part=snippet&maxResults=50&pageToken=CDIQAQ`)
      const secondPart = await fetch(`${YOUTUBE_PLAYLIST_ITEMS_API}?key=${process.env.YOUTUBE_API_KEY}&playlistId=${YOUTUBE_PLAYLIST}&part=snippet&maxResults=38&pageToken=CDIQAA`)
      const data1 = await firstPart.json()
      const data2 = await secondPart.json()

      const youTubeData = [...data1.items, ...data2.items]

      const covers = await youTubeData.map((c, i) => {
        return {
          id: i + 1,
          key: c.snippet.resourceId.videoId,
          img: c.snippet.thumbnails.medium.url
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


<style>
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
  background: #eeeeee;
}

#app {
  font-family: PlayfairDisplay, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  //padding-top: 60px;
}

.title {
  text-align: center;
  font-size: 2.5rem;
  color: #000;
}
</style>