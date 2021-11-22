<template>
  <div class="playlist-form card">
    <div class="header">
      <button class="close-btn" @click="closeMe()">
        <svg viewBox="0 0 300 500" width="25" height="25" class="exitIcon">
          <path
            fill="currentColor"
            d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"
          ></path>

          />
        </svg>
      </button>
      <h4 class="title">Select a playlist</h4>
    </div>
    <ul class="playlists-container">
      <li v-for="(item, index) in playlists" :key="index">
        <p @click="addTrackToPlaylist(item, index)">{{ item.playlist }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  props: {
    track: Object,
    playlists: Array,
  },
  data: () => {
    return {
      playlist: '',
      user: {},
    }
  },
  methods: {
    ...mapActions({
      addTrackAction: 'addTrackToPlaylist',
    }),

    async checkUser() {
      let vm = this
      await this.$fire.auth.onAuthStateChanged(async function (user) {
        let obj
        if (user) {
          console.log(user.email)
          obj = { user: true, email: await user.email }
          console.log(obj)
          vm.user = obj
        } else {
          console.log('no user')
          return (window.location.href = '/login')
        }
      })
    },

    closeMe() {
      return this.$emit('closeModal')
    },

    async addTrackToPlaylist(playlistObj, index) {
      let obj = {
        email: this.user.email,
        position: index,
        track: this.track,
      }
      console.log(obj)
      await this.addTrackAction(obj)
      return this.closeMe()
    },
  },
  mounted() {
    this.checkUser()
  },
}
</script>

<style scoped>
.playlist-form {
  width: 17rem;
  height: 20rem;
  max-height: 20rem;
  position: fixed;
  z-index: 50;
  top: 40%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  padding: 0.5rem 0.8rem 0.5rem 0.8rem;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.header {
  border-bottom: 1px solid #645777;
}

.title {
  font-weight: 900;
  text-align: center;
}

.close-btn {
  color: white;
  display: block;
  margin-left: auto;
  background-color: transparent;
  border: none;
  margin-right: -0.7rem;
  margin-top: -0.5rem;
}

.playlists-container {
  list-style: none;
  padding: 0;
  margin: 0;
  margin-top: 1rem;
  padding-left: 0.5rem;
  padding-right: 0.5rem;
  overflow-y: auto;
}

li {
  cursor: pointer;
}

p {
  color: #03cbee;
}
</style>
