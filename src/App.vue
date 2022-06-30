<template>
  <div id="app">    
  <div id="nav">

  <b-navbar type="dark" variant="dark">
    <b-navbar-nav>
      <b-nav-item :to="{ name: 'main' }">Home</b-nav-item>
      <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
    </b-navbar-nav>
    <b-navbar-nav class="ml-auto">
      <!-- guest -->
        <b-nav-text id="guestLabel" right v-if="!$root.store.username">Hello Guest</b-nav-text>
        <b-nav-item v-if="!$root.store.username" :to="{ name: 'register' }">Register</b-nav-item>
        <b-nav-item v-if="!$root.store.username" :to="{ name: 'login' }">LogIn</b-nav-item>
      <!-- user -->
        <b-nav-item-dropdown id="userLabel" v-if="$root.store.username" :text="concatHello" right>
          <b-dropdown-item :to="{ name: '' }">Favorites</b-dropdown-item>
          <b-dropdown-item :to="{ name: '' }">Personal Recipes</b-dropdown-item>
          <b-dropdown-item :to="{ name: '' }">Family Recipes</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item v-if="$root.store.username" @click="Logout">LogOut</b-nav-item>
    </b-navbar-nav>    
      </b-navbar>
    </div>

    <router-view />    
  </div>
  
</template>

<script>
export default {
  name: "App",
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  },
  computed:{
    concatHello:{
      get: function(){
        return `Hello ${this.$root.store.username}`;
      }
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  
}

#nav {
  position: relative;
}

#nav a {
  font-weight: bold;
  color: #8ae8ef;
  text-align: center;
}

#nav a.router-link-exact-active {
  color: #07a9b5;  
}
#guestLabel{
  color: #8ae8ef;
  font-weight: bold;
}
</style>
