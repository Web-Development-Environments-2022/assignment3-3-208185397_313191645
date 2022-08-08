<template>
  <div class="container"> <br>
    <h1 class="title">Main Page</h1>
    <div class="wrapper">
      <RecipePreviewList title="Randome Recipes" source="/recipes/random" isVertical="true" class="RandomRecipes center" />
    <!--
 <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
    --> 
    <div class="loginWrapper" v-if="!$root.store.username">
      <LoginPage class="loginPage"></LoginPage>
    </div>    
      <RecipePreviewList v-else
        title="Last Viewed Recipes" source="/users/LastSeen"
        isVertical="true"
        :class="{
          RandomRecipes: true,
          blur: !$root.store.username,
          center: true
        }"
        disabled
      ></RecipePreviewList>
    </div>    
    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import LoginPage from "../pages/LoginPage.vue";
export default {
  components: {
    RecipePreviewList,
    LoginPage
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
.title{
  font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
.wrapper{
  display: grid;
  grid-template-columns: 1fr 1fr; 
}
.loginPage{
    border: 2px solid black;
  border-radius: 5px;
  color: black;
  max-height: 350px;
}
.loginWrapper{
  padding-top: 100px;
  padding-left: 100px;
}
</style>
