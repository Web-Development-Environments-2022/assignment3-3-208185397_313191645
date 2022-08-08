<template>
  <div id="app">    
  <div id="nav">

  <b-navbar fixed="top" type="light" variant="dark">
    <b-navbar-nav>
      <b-nav-item :to="{ name: 'main' }">Home</b-nav-item>
      <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
    </b-navbar-nav>
    <b-navbar-nav class="ml-auto">
      <!-- guest -->
        <b-nav-text style="padding-right: 6px;" id="guestLabel" right v-if="!$root.store.username">Hello Guest</b-nav-text>
        <b-nav-item v-if="!$root.store.username" :to="{ name: 'register' }">Register</b-nav-item>
        <b-nav-item v-if="!$root.store.username" :to="{ name: 'login' }">LogIn</b-nav-item>
      <!-- user -->
        <b-nav-item-dropdown style="padding-right: 6px;" id="userLabel" v-if="$root.store.username" :text="concatHello" right>
          <b-dropdown-item :to="{ name: 'favorites' }">Favorites</b-dropdown-item>
          <b-dropdown-item :to="{ name: 'personal' }">Personal Recipes</b-dropdown-item>
          <b-dropdown-item :to="{ name: 'family' }">Family Recipes</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item v-if="$root.store.username" @click="Logout">LogOut</b-nav-item>
    </b-navbar-nav>    
      </b-navbar>
    </div>

    <router-view />    

        <b-modal ref="my-modal" hide-footer title="System Message">
      <div class="d-block text-center">
        <h3>{{modalMessage}}</h3>
      </div>
      <b-button class="mt-3" variant="outline-danger" block @click="hideModal">Close Me</b-button>      
    </b-modal>
  </div>
  
</template>

<script>
export default {
  name: "App",
  data(){
    return {
      modalMessage: ""
    }
  },
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
    // modal
      showModal(message) {
        this.modalMessage = message;
        this.$refs['my-modal'].show()
      },
      hideModal() {
        this.modalMessage = "";
        this.$refs['my-modal'].hide()
      },
      toggleModal() {
        // We pass the ID of the button that we want to return focus to
        // when the modal has hidden
        this.$refs['my-modal'].toggle('#toggle-btn')
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
  margin-bottom: 3%;
}

#nav a {
  font-weight: bold;
  color: burlywood;
  text-align: center;
  
}

#nav a:hover {
  color: rgb(238, 141, 15);  
  
}
#guestLabel{
  color: burlywood;
  font-weight: bold;
  
}

</style>
