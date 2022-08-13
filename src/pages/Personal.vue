<template>
  <div class="container"> <br>
    <h1 class="title">My Recipes</h1>
    <div>
        <RecipePreviewList title="My Personal Posts" source="/Users/MyRecipes" class="RandomRecipes" />  
        <br>
        <b-button @click="changeView"  variant="success" >Create New Recipe</b-button>
        <br><br><br><br>
    </div>        
  </div>
</template>

<script>

import RecipePreviewList from "../components/RecipePreviewList";
export default {
  components: {
    RecipePreviewList
  },
  data(){
    return {        
        created_recipe: {
            ingredients: "",
            numberOfDishes: "",
            prepInstructions: "",
            recepiePreview: {
                id: null,
                title: "",
                prepTime: "",
                popularity: null,
                glutenFree: false,
                vegan: false,
                alreadyWatched: null,
                inFavorites: null,
                imageUri: ""
            }
        },
        ingredientName: "",
        ingredientAmount: "",
        pressed: false
    }
  },
  methods: {
    checkTitleState(){
        if(this.pressed)
            return this.created_recipe.recepiePreview.title != '';
        return null;
    },
    checkTimeState(){
        if(this.pressed)
            return this.created_recipe.recepiePreview.prepTime != '' && !isNaN(this.created_recipe.recepiePreview.prepTime);
        return null;
    },
    checkDishesState(){
        if(this.pressed)
            return this.created_recipe.numberOfDishes != '' && !isNaN(this.created_recipe.recepiePreview.prepTime);
        return null;
    },
    changeView(){
        this.pressed = false;
        this.$parent.showRecipeModal();
    },

    async onAdd(){     
        this.pressed = true;
        let title_state = this.checkTitleState();
        let time_state = this.checkTimeState();
        let dishes_state = this.checkDishesState();
        if(!(title_state && time_state && dishes_state)){
            return;
        }
        await this.axios.post(          
          this.$root.store.server_domain + "/Users/MyRecipes",
          this.created_recipe
        );
        this.changeView();
    },
    onReset(){

    },
    addIngredient(){
        this.created_recipe.ingredients += " " + this.ingredientName + " | " + this.ingredientAmount + " & ";
        this.ingredientName = "";
        this.ingredientAmount = "";
    }
  }
};
</script>

<style lang="scss" scoped>
.title{
  font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
</style>
