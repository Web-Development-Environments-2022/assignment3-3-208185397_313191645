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
        label="Preperation Time(in minutes):"
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
      <div class="photo">
        <h5>Add a photo:<br><small>Insert a hyperlink</small></h5>        
        <input v-model="created_recipe.recepiePreview.imageUri" placeholder="https://img.etimg.com/thumb/msid-76085858,width-1200,height-900,imgsize-91873,overlay-etpanache/photo.jpg">
      </div>      

      <div class = "ingredients">
        <h5>Added ingredients:</h5>
        <ul v-if="created_recipe.ingredients!=''">  
          <li class="list-group-item borderless" button variant="secondary" v-for="(i, index) in created_recipe.ingredients.split('&')"
            :key="index + '_' + i"
          ><div v-if="index!=(created_recipe.ingredients.split('&').length-1)">
            {{i}}
          <b-button @click="removeIng(i)" size="sm" class="ml-2" variant="danger">x</b-button>
          </div>            
          </li>
        </ul>
        <small v-else>You haven't added ingredients yet</small>      
      </div>      
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
            this.onReset();
            this.$parent.hideRecipeModal();
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
          this.created_recipe = {
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
            };
            this.ingredientName= "";
            this.ingredientAmount= "";
            this.pressed= false;
        },
        addIngredient(){
            if(this.ingredientName=="" || this.ingredientAmount=="") return;
            this.created_recipe.ingredients += " " + this.ingredientName + " | " + this.ingredientAmount + " & ";
            this.ingredientName = "";
            this.ingredientAmount = "";
        },
        removeIng(ing){
          let temp = this.created_recipe.ingredients.split('&');
          let index = temp.indexOf(ing);
          if(index<=-1) return;
          temp.splice(index,1);
          this.created_recipe.ingredients = temp.join('&');
          if(this.created_recipe.ingredients==' ') this.created_recipe.ingredients = ""
          console.log(this.created_recipe.recepiePreview.imageUri);
        }
    }
}
</script>

<style>
.container{
  
}
.right{
  
  float: right;
}
.left{  
  float: left;
}
.photo{
  padding-bottom: 30%;
}
.ingredients{

}
</style>