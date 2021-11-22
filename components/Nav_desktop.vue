<template>
  <div class="desktop-nav">
    <ul class="nav-ul nav justify-content-center">
      <li class="nav-item">
        <a class="nav-link active" aria-current="page">
          <NuxtLink to="/">Home</NuxtLink>
        </a>
      </li>
      <li class="nav-item">
        <a class="nav-link">
          <NuxtLink to="/library">Library</NuxtLink>
        </a>
      </li>
      <li class="nav-item">
        <a class="nav-link">
          <NuxtLink to="/login">Log in</NuxtLink>
        </a>
      </li>
      <Search />
    </ul>
  </div>
</template>

<script>
import Search from '../components/Search'

export default {
  components: { Search },

  props: {
    showInput: Boolean,
  },
  methods: {
    showSearch() {
      this.$emit('showsearch')
    },
  },
}
</script>

<style scoped>
.desktop-nav {
  background-color: #68687b;
  width: 100%;
  position: fixed;
  z-index: 1;
  height: 3rem;
  display: none;
}

.nav-ul {
  display: none;
  background-color: #271642;
}

a {
  color: white;
  font-size: 18px;
}

@media only screen and (min-width: 770px) {
  .desktop-nav {
    display: flex;
  } 
  .nav-ul {
    display: flex;
    width: 100%;
    justify-content: flex-start !important;
    background-color: transparent;
    margin-left: 3rem;
  }
  .search-icon {
    cursor: pointer;
    margin-top: 0.7%;
    margin-left: 2%;
  }
}
</style>
