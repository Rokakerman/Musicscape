<template>
  <div class="playlist-form card">
    <div class="">
      <button class="close-btn" @click="closeMe()">
        <svg viewBox="0 0 300 500" width="25" height="25" class="exitIcon">
          <path
            fill="currentColor"
            d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"
          ></path>

          />
        </svg>
      </button>
      <label class="label">Playlist name</label>
      <input
        v-model="playlist"
        class="form-control"
        placeholder="Enter playlistname"
        required
      />
    </div>
    <div class="button-container">
      <button
        v-if="playlist.length > 0"
        type="submit"
        id="global-button"
        class="globalBtnActive btn"
        @click="newPlaylist()"
      >
        Create
      </button>
      <button
        v-else
        type="submit"
        id="global-button"
        class="btn globalBtnInactive"
      >
        Create
      </button>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  data: () => {
    return {
      playlist: '',
      user: {},
    }
  },
  methods: {
    ...mapActions({
      addNewPlaylist: 'addNewPlaylist',
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
    async newPlaylist() {
      let obj = {
        email: this.user.email,
        tracklist: [],
        playlist: this.playlist,
      }
      console.log(obj)
      await this.addNewPlaylist(obj)
      return this.closeMe()
    },
    closeMe() {
      return this.$emit('closeModal')
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
  position: fixed;
  z-index: 50;
  top: 40%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  padding: 0.5rem 0.8rem 0.5rem 0.8rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
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

.label {
  background-color: #1e1133;
  color: #00ddff;
  margin-bottom: -11px;
  position: relative;
  display: table;
  margin-left: 0.8rem;
  z-index: 2;
}

.form-control {
  background-color: #1e1133;
  z-index: 1;
}

.button-container {
  display: flex;
  width: 100%;
  justify-content: center;
  padding-bottom: 2rem;
}

.btn {
  width: 10rem;
  height: 3rem;
  font-weight: 900;
  font-size: 20px;
  border: none;
}

button.active.focus,
button.active:focus,
button.focus,
button:active.focus,
button:active:focus,
button:focus {
  outline: none;
  box-shadow: none;
}
</style>
