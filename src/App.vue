<template>
  <div id="app">
    <nav>
      <router-link to="/">
        Users
      </router-link> |
      <router-link to="/table">
        Table
      </router-link>
    </nav>
    <router-view />
    <AppFooter />
  </div>
</template>

<script>
import axios from 'axios';
import AppFooter from './components/AppFooter.vue';

export default {
  components: {
    AppFooter,
  },
  created() {
    const post = {
      username: 'sarah',
      password: 'connor',
    };

    axios.post('http://localhost:8081/auth', post).then((result) => {
      window.localStorage.setItem('token', result.data.token);
    });
  },
};
</script>

<style lang="scss">

// Global styles
@import "@/scss/main.scss";

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-image: url(./assets/background.svg);
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100vh;
  width: 100vw;
  font-family: 'Poppins';
  box-sizing: border-box;
  nav {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 50px;
    padding: $padding30;
    color: $mainColor;
    font-weight: 600;
    box-sizing: border-box;
    a {
      color: $mainColor;
      font-size: $textM;
      margin: 10px;
      text-decoration: none;
      font-weight: 600;
      &:hover {
        color: $secondaryColor;
      }
    }
  }
}

</style>
