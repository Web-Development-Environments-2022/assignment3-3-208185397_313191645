<template>
  <div class="container"> <br>
    <h1 class="title">Favorites</h1>
    <RecipePreviewList title="Favorite Recipes" source="/Users/FavoriteRecipes" class="RandomRecipes" />        
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  components: {
    RecipePreviewList
  }
};
</script>

<style lang="scss" scoped>
.title{
  font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
</style>
