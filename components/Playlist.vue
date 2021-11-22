<template>
  <div class="home-container">
    <h1>Latest playlists last 24 hours</h1>
    <ul id="v-for-object">
      <li
        v-for="(value, index) in playLists"
        :key="index"
        class="playlist-item"
      >
        <article class="playlist-preview card">
          <a class="playlist-redirect"  v-bind:href="value.shorturl" target="_blank">
           Go to playlist <img src="~/assets/arrow-right_white.svg" />
          </a>
          <a class="playlist-redirect" v-bind:href="value.zip" target="_blank">
            Download <img src="~/assets/downloadbutton_white.svg" />
          </a>
        </article>
        <footer class="playlist-info">
          <p class="mini-text">{{ value.name | textTruncate() }}</p>
          <p class="mini-text">{{ value.user_name | textTruncate() }}</p>
        </footer>
      </li>
    </ul>
    <footer class="transparent-footer"></footer>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      playLists: {},
    }
  },
  filters: {
    textTruncate: function (str) {
      let length = str.length
      let ending = '...'

      if (length > 25) {
        let newStr = str.slice(0, 25)
        return newStr.concat(ending)
      } else {
        return str
      }
    },
  },
  methods: {
    getPlaylist() {
      var vm = this
      let yesterday =
        new Date().getFullYear() +
        '-' +
        ('0' + (new Date().getMonth() + 1)).slice(-2) +
        '-' +
        ('0' + (new Date().getDate() - 1)).slice(-2)

      let today =
        new Date().getFullYear() +
        '-' +
        ('0' + (new Date().getMonth() + 1)).slice(-2) +
        '-' +
        ('0' + new Date().getDate()).slice(-2)

      if (today[9] == 1 && today[8] == 0) {
        yesterday =
          new Date().getFullYear() +
          '-' +
          ('0' + new Date().getMonth()).slice(-2) +
          '-' +
          30
      }

      // if (yesterday[8] && yesterday[9] == 0) {
      //   yesterday[8] = 3
      // }

      let url =
        'https://api.jamendo.com/v3.0/playlists/?client_id=a31f0360&format=jsonpretty&datebetween=' +
        yesterday +
        '_' +
        today

      axios
        .get(url)
        .then(function (response) {
          // handle success
          console.log(response)
          vm.playLists = response.data.results
        })
        .catch(function (error) {
          // handle error
          console.log(error)
        })
    },

    goToPlayList() {
      let url = axios
        .get(url)
        .then(function (response) {
          // handle success
          console.log(response.data)
        })
        .catch(function (error) {
          // handle error
          console.log(error)
        })
    },
  },
  mounted() {
    this.getPlaylist()
  },
}
</script>

<style scoped>
.home-container {
  margin-bottom: auto;
  padding-top: 2rem;
}

.title-header {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  padding: 1rem;
}

ul {
  margin: 0px;
  padding: 0px;
  margin-top: 2rem;
  display: grid;
  grid-row-gap: 2rem;
  grid-template-columns: repeat(2, auto);
  justify-content: space-between;
}

article {
  display: flex;
  flex-direction: row;
  padding: 0rem 0rem 01rem 0rem;
  justify-content: space-around;
}

.playlist-item {
  list-style: none;
  height: 10rem;
  width: 9rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.playlist-preview {
  height: 80%;
  width: 100%;
  list-style: none;
  margin: 0px;
  padding: 0.5rem;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
}

.playlist-redirect {
  border-radius: 0.2rem;
  border: none;
  font-size: 14px;
  background-color: transparent;
  color: white;
}

button {
  border-radius: 0.2rem;
  border: none;
  font-size: 14px;
  background-color: transparent;
  color: white;
}

.playlist-info {
  height: 20%;
  width: 97%;
  padding-top: 0.2rem;
  background-color: transparent;
}

.mini-text {
  margin: 0px;
  font-size: 11px;
  color: #00ddff;
  text-align: start;
  width: max-content;
}


.transparent-footer {
  background-color: transparent;
  color: #00ddff;
  padding-left: 1.5rem;
  margin-bottom: auto;
  height: 9rem;
}


@media only screen and (max-width: 770px) {
  b-card {
    margin-top: 2rem;
  }

  h1 {
    font-size: 1.5rem;
  }
}

#main {
  display: flex;
  flex-direction: row;
}

h1 {
  color: white;
  display: flex;
}
</style>
