<template>
  <div class="media-controls">
    <audio controls autoplay ref="player">
      <source :src="$store.state.music" type="audio/ogg" />
      <source :src="$store.state.music" type="audio/mpeg" />
      Your browser does not support the audio element.
      <p>Hello</p>
    </audio>

    <!-- <button
      @click="
        $store.commit('setMusic', {
          url: 'https://mp3l.jamendo.com/?trackid=1561379&format=mp31&from=iDywHKCe+xqfg1wwxV7eiA==|GmIQIvUjuucTL+rzbvj6XA==',
        })
      "
    >
      Click
    </button> -->
  </div>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {},
  watch: {
    '$store.state.music'(value, oldValue) {
      console.log(value)
      console.log(oldValue)
      this.$refs.player.load()
    },
  },
}
</script>

<style scoped>
.media-controls {
  display: flex;
  width: 100%;
  justify-content: center;
}

audio {
  background-color: #8a86ff;
  border-radius: 0;
  width: 100%;
  position: fixed;
  bottom: 3rem;
}

@media only screen and (min-width: 770px) {
  audio {
    width: 100%;
    border-radius: 0rem;
    bottom: 0rem;
    position: relative;
    border: solid 1px white;
    border-top: 0px;
    border-bottom: 0px;
  }
}

@media only screen and (min-width: 770px) {
  .media-controls {
    width: 80%;
    max-width: 50rem;
    bottom: 0rem;
    position: relative;
  }
}
</style>
