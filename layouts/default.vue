<template>
  <div>
    <div class="layout">
      <Nav :showInput="showSearchBar" v-on:showsearch="showSearch()" />
      <Search
        class="toggleDesktop"
        v-if="showSearchBar"
        v-on:showsearch="closeSearch()"
      />
      <Nuxt class="desktop" />
      <footer class="global-footer"><p>Powered by Jamendo</p></footer>
      <div class="audio-footer">
        <MusicPlayer />
      </div>
      <LazyMobileNav ref="usernameInput" v-on:showsearch="showSearch()" />
    </div>
  </div>
</template>

<script>
import Nav from '../components/Nav_desktop'
import MusicPlayer from '../components/MusicPlayer'
import Search from '../components/Search.vue'

export default {
  components: { Nav, MusicPlayer, Search },
  data: () => {
    return {
      showSearchBar: false,
    }
  },

  methods: {
    showSearch() {
      return (this.showSearchBar = !this.showSearchBar)
    },

    closeSearch: function () {
      this.showSearch()
      return this.$refs.usernameInput.focus()
    },
  },
}
</script>

<style>
.layout {
  width: 100%;
  display: flex;
  flex-direction: column;
}

.global-footer {
  background-color: transparent;
  color: #00ddff;
  padding-left: 1.5rem;
  margin-bottom: auto;
  position: relative;
  bottom: 5.5rem;
}

html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

h1,
h2,
h4,
h4,
h5,
h6 {
  font-family: 'Montserrat', sans-serif;
}

p,
input {
  font-family: 'Lato', sans-serif;
}

.card {
  background-color: #1e1133;
  border: solid white 1px;
  border-radius: 0.2rem;
  color: white;
}

body {
  background-color: #271642;
}

.globalBtnActive {
  background-color: #17a2b8;
  color: white;
  font-family: 'Lato', sans-serif;
}

.globalBtnActive:hover {
  background-color: #17a2b8;
  color: white;
}

.globalBtnInactive {
  background-color: #17a2b89e;
  /* #106B79 */
  font-family: 'Lato', sans-serif;
  color: white;
}

.globalBtnInactive:hover {
  background-color: #17a2b89e;
  color: white;
}

@media only screen and (min-width: 770px) {

  .audio-footer {
    width: 100%;
    background-color: #2f2f47; /* ##1e1133 */
    height: 4rem;
    bottom: 0rem;
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    border: solid 1px rgba(255, 255, 255, 0.274);
    border-bottom: 0px;
    border-left: 0px;
    border-right: 0px;
  }

  .toggleDesktop {
    display: none;
  }

  .desktop {
    margin-top: 3rem;
  }
}

@media only screen and (min-width: 1200px) {
  .global-footer {
    background-color: transparent;
    color: #00ddff;
    padding-left: 1.5rem;
    margin-bottom: auto;
    position: fixed;
    bottom: 4rem;
  }
}
</style>
