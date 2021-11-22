<template>
  <div>
    <div id="app-instasearch">
      <div class="input-container">
        <input
          class="input-field"
          type="text"
          placeholder="Search..."
          v-model="term"
        />
        <div class="exitSearchBar">
          <svg
            viewBox="0 0 300 500"
            width="25"
            height="25"
            v-on:click="closeSearch()"
          >
            <path
              fill="currentColor"
              d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"
            ></path>

            />
          </svg>
        </div>
      </div>

      <div
        v-bind:class="{ active: isActive, inactive: !isActive }"
        id="list-group"
        tag="ul"
        name="list-animation"
      >
        <div class="result-item" v-for="result in results" :key="result.id">
          <header class="result-item-header">
            <img class="card" v-bind:src="result.image" />
            <p class="p author">{{ result.album_name }}</p>
          </header>

          <footer class="result-item-footer">
            <button
              id="play-btn"
              @click="
                $store.commit('setMusic', {
                  url: result.audio,
                })
              "
            >
              <p class="mini-text">Play Sample</p>
            </button>

            <button id="lib-btn" @click="addTrack(result.id)">
              <p class="mini-text">Add to library</p>
            </button>
            <audio>
              <source v-bind:src="result.audio" />
            </audio>
          </footer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import Popupmodal from './Popupmodal.vue'
export default {
  components: { Popupmodal },
  data: () => {
    return {
      SearchString: '',
      term: '',
      results: [],
      data: null,
      audio: null,
      isActive: false,
    }
  },
  watch: {
    term: function () {
      if (!this.term) {
        console.log('Inside if')
        return (this.isActive = false)
      }
      console.log('Outside if')
      this.search()
      return (this.isActive = true)
    },
  },

  computed: {
    ...mapState(['credentialsExist']),
    filteredFeed: function () {
      var results = this.data
      var SearchString = this.SearchString

      if (!SearchString) {
        return results
      }

      this.searchString = SearchString.trim().toLowerCase()

      results = results.filter(function (item) {
        if (
          item.album_name.toLowerCase().indexOf(SearchString) !== -1 ||
          item.album_name.toUpperCase().indexOf(SearchString) !== -1
        ) {
          return item
        }
      })

      return results
    },
    webShareApiSupported() {
      return navigator.share
    },
  },

  methods: {
    ...mapActions({
      saveTrack: 'saveTrack',
    }),

    closeSearch: function () {
      console.log('I am working')
      return this.$emit('showsearch')
    },

    search: function () {
      let CLIENT_ID = process.env.clientId
      if (this.audio) {
        this.audio.pause()
        this.audio.currentTime = 0
      }

      fetch(
        `https://api.jamendo.com/v3.0/tracks/?client_id=${CLIENT_ID}&search=${encodeURIComponent(
          this.term
        )}&limit=5&order=downloads_week&groupby=artist_id`
      )
        .then((res) => res.json())
        .then((res) => {
          this.results = res.results
        })
      console.log(this.results)
    },
    play: function (s) {
      if (this.audio) {
        this.audio.pause()
        this.audio.currentTime = 0
      }
      this.audio = new Audio(s)
      this.audio.play()
    },
    addTrack: async function (trackId) {
      let vm = this
      await this.$fire.auth.onAuthStateChanged(async function (user) {
        if (user) {
          console.log('hej')
          for (var index in vm.results) {
            if (vm.results[index].id == trackId) {
              const email = user.email
              const track = vm.results[index]
              vm.saveTrack({ email, track })
            }
          }
        } else {
          console.log('you need to be logged in')
          return (window.location.href = '/login')
        }
      })
    },
    test() {
      this.$store.dispatch('checkUserExists')
    },
    displayComponent() {
      this.isActive = false
    },
    hideComponent() {
      this.isActive = true
    },
  },
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
.svg {
  width: 3rem;
}

button {
  background-color: transparent;
  color: white;
  border: none;
}
button:hover {
}
input:focus,
select:focus,
textarea:focus,
button:focus {
  outline: none;
}

html {
  overflow-y: scroll;
}

body {
  display: grid;
  grid-template-rows: 9.375rem 1fr;
  background-color: #f5f5f5;
  font-family: 'Roboto Slab', serif;
  margin-bottom: 0.625rem;
}

header {
  display: grid;
  color: white;
}

header h1 {
  place-self: center;
  font-size: 2.625rem;
}

#app-instasearch {
  display: flex;
  justify-content: center;
  align-items: center;
  place-self: center;
  width: max-content;
  top: 0;
  z-index: 50;
  height: 100%;
  flex-direction: column;
}

.input-container {
  border-radius: 0;
  background: #68687b;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 2rem;
  width: 25rem;
  padding-left: 0.5rem;
  border-left: solid white 1px;
}

.input-field {
  color: white;
  font-size: 1.125rem;
  border: none;
  border-radius: 0.25rem;
  background-color: #2f2f47;
  width: 100%;
  height: 2rem;
  padding-left: 0.5rem;
}

.exitSearchBar {
  display: none;
  cursor: default;
}

::placeholder {
  /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: white;
  opacity: 1; /* Firefox */
}

.photo {
  list-style: none;
  display: grid;
  grid-template-columns: 12.5rem auto;
  margin-top: 1.25rem;
  align-items: center;
  background-color: #e9edf0;
  padding: 1.875rem 3.125rem;
  border-radius: 0.3125rem;
  border: 0.0625rem solid #e3e3e3;
}

.author {
  font-size: 18px;
  margin-left: 0.125rem;
  color: white;
  display: flex;
}

.title {
  border-radius: 0.3125rem;
  width: 12.5rem;
}

.photo-animated {
  transition: all 0.5s;
}

.list-animation-enter,
.list-animation-leave-to {
  opacity: 0;
  transform: translateY(1.875rem);
}

#list-group {
  height: max-content;
  width: 100%;
  background-color: #231933;
  flex-direction: column;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.result-item {
  transition: all 0.5s;
  margin-left: 1rem;
  width: 20rem;
  background-color: #231933;
  padding: 1rem 0rem 1rem 0rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.527);
}

.result-item-header {
  display: flex;
  background-color: none;
  justify-content: flex-start;
  padding: 0rem 1rem 0rem 0rem;
}

.result-item-footer {
  display: flex;
  justify-content: flex-start;
  padding: 0rem 1rem 0rem 0rem;
  margin-top: 0.5rem;
}

#lib-btn {
  padding-left: 1rem;
}

.list-animation-leave-active {
  position: absolute;
}
img {
  max-width: 5rem;
}
ul {
  background: #3b2460;
  padding: 5%;
  overflow-y: scroll;
  height: 50rem;
}

.mini-text {
  margin: 0px;
  font-size: 11px;
  color: #00ddff;
}

.inactive {
  display: none !important;
}

.active {
  display: flex;
}

@media only screen and (min-width: 770px) {
  #list-group[data-v-7a642ec3] {
    width: 24rem;
    background-color: #231933;
    flex-direction: column;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    margin-top: 0rem;
    top: 3rem;
    position: fixed;
    border: 1px solid white;
  }
}

@media only screen and (max-width: 48.125rem) {
  #app-instasearch {
    margin-left: 0;
    width: 100%;
  }
  .input-container {
    margin-right: 0;
    padding-left: 0rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    max-width: 100%;
    height: 3rem;
    width: 100vw;
    padding-right: 1rem;
    padding-left: 1rem;
  }
  img {
    width: 2rem;
    height: 100%;
  }
  #list-group {
    max-width: 100%;
    position: relative;
  }
  .author {
    font-size: 11px;
    text-align: center;
    margin-left: 0.125rem;
    display: table-cell;
    color: white;
  }

  .exitSearchBar {
    display: flex;
    justify-content: center;
    align-items: center;
    width: max-content;
    height: max-content;
    color: white;
    cursor: pointer;
  }
}

@media only screen and (max-width: 48.125rem) {
  #list-group[data-v-7a642ec3] {
    max-width: 100%;
    position: relative;
    z-index: 50;
    height: max-content;
    display: flex;
    justify-content: flex-start;
    overflow: scroll;
    padding-bottom: 2rem;
  }

  .input-field {
    width: 90%;
  }

  #app-instasearch {
    place-self: center;
    width: 100%;
    position: fixed;
    top: 0;
    z-index: 50;
    height: auto;
    top: 0;
    border-bottom: 1px solid white;
    display: flex;
    flex-direction: column;
  }

  .input-container {
    border-left: 0px;
  }
}
</style>
