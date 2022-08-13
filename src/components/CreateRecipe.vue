<template>
<div class="container">      
    <div class="left">                    
      <b-form @submit.prevent="onAdd" @reset.prevent="onReset">
      <b-form-group
        id="input-group-title"
        label-cols-sm="3"
        label="Recipe Title:"
        label-for="title"
      >
        <b-form-input
          id="title"
          v-model="created_recipe.recepiePreview.title"
          type="text"
          :state="checkTitleState()"
        ></b-form-input>
      </b-form-group>
    
    <b-form-group
        id="input-group-time"
        label-cols-sm="3"
        label="Preperation Time:"
        label-for="time"
      >
        <b-form-input
          id="time"
          v-model="created_recipe.recepiePreview.prepTime"
          type="text"
          :state="checkTimeState()"
        ></b-form-input>

      </b-form-group>

    <b-form-group
        id="input-group-dishes"
        label-cols-sm="3"
        label="Amount Of Dishes:"
        label-for="dishes"
      >
        <b-form-input
          id="dishes"
          v-model="created_recipe.numberOfDishes"
          type="text"
          :state="checkDishesState()"
        ></b-form-input>

      </b-form-group>

      <b-form-group
        id="input-group-gluten"
        label-cols-sm="3"
        label="Is GlutenFree:"
        label-for="gluten"
      >
        <b-form-select
          id="gluten"
          v-model="created_recipe.recepiePreview.glutenFree"
          :options="[true, false]"
        ></b-form-select>
      <small v-if="created_recipe.recepiePreview.glutenFree">please make sure your recipe is indeed gluten free</small>
      </b-form-group>

    <b-form-group
        id="input-group-vegan"
        label-cols-sm="3"
        label="Is Vegan:"
        label-for="vegan"
      >
        <b-form-select
          id="vegan"
          v-model="created_recipe.recepiePreview.vegan"
          :options="[true, false]"
    ></b-form-select>
    <small v-if="created_recipe.recepiePreview.vegan">please make sure your recipe is indeed vegan</small>

      </b-form-group>
    <br>
    <b-form-group>
        <b-form-input
          id="instructions"
          v-model="created_recipe.prepInstructions"
        placeholder="Insert Your Recipe Instructions"
        
        ></b-form-input>
        <br>
        <div style="display: inline-flex;">
        <div style="padding-right:50px;"><b>Insert Ingredients:</b></div>
            <b-form-input placeholder="ingredient name" id="ingredient name" v-model="ingredientName"></b-form-input>
        <b-form-input placeholder="ingredient amount" id="ingredient amount" v-model="ingredientAmount"></b-form-input>
        </div>        
        <b-button @click="addIngredient">Add Ingredient</b-button>        
    </b-form-group>    
    
    <!--------------------------------------------------------------------->
      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Add Recipe</b-button
      >
      <br><br>
    </b-form>
        

    </div>
    
    <div class="right"><br>
      <h5>Added ingredients:</h5>
      <b-list-group>  
        <b-list-group-item v-for="(i, index) in created_recipe.ingredients.split('&')"
          :key="index + '_' + i"
        >{{i}}</b-list-group-item>
      </b-list-group>
    </div>               

</div>
</template>

<script>
export default {
    name: "CreateRecipe",
    components :{
        
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
            this.viewing = !this.viewing;
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
            if(this.ingredientName=="" || this.ingredientAmount=="") return;
            this.created_recipe.ingredients += " " + this.ingredientName + " | " + this.ingredientAmount + " & ";
            this.ingredientName = "";
            this.ingredientAmount = "";
        }
    }
}
</script>

<style>
.container{
  display: grid;
  grid-template-columns: 1fr 1fr; 
}
.right{
  margin-left: 4px;
}
.left{
  margin-right: 4px;
}
</style>