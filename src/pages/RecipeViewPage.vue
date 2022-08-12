<template>
<div>
  <div v-if="recipe">
    
    <div class="content">            
      <div class="ingredInstruc">
        <div>
          <div class="previewWrapper">
            <RecipePreview class="recipePrev" :recipe="this.recipePrev" :noExtend="false"></RecipePreview>        
          </div>                    
        </div>      
        <div class="ingred">
          <h3 style="padding-left: 6px; text-decoration: underline;">Ingredients ({{recipe.numOfDishes}} dishes):</h3>          
          <small>Press on an ingredient to find it near by</small>
          <ul style="list-style: none;">
              <li
                v-for="(r, index) in recipe.extendedIngredients"
                :key="index + '_' + r.id"
              >
              <input type="checkbox">
                <a class="ingredItems" :href="getGoogleLink(r)" target=”_blank”>
                  {{ r }}
                </a>
              </li>
            </ul>
        </div>
        <div >
          <div class="instruc">
            <h3 style="text-decoration: underline;">Instructions:</h3>
            <p><span class="ingredItems" v-html="recipe.instructions"></span></p>
          </div>                      
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import RecipePreview from '../components/RecipePreview.vue';
export default {  
  data() {
    return {
      recipe: null,
      recipePrev: null
    };
  },
  components:{
    RecipePreview
  },
  methods:{
    removeTags(str) {
      if ((str===null) || (str===''))
          return false;
      else
          str = str.toString();
            
      // Regular expression to identify HTML tags in 
      // the input string. Replacing the identified 
      // HTML tag with a null string.
      return str.replace( /(<([^>]+)>)/ig, '');
    },
    getGoogleLink(ingredient) {            
      return `https://www.google.com/maps/search/${ingredient.split('|')[0]}+near+me`         
    },
    async showModal(message){
      this.$parent.showModal(message);
    }
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;      
      try {
        response = await this.axios.get(
          this.$root.store.server_domain + "/Recipes/ExtendedRecipes/" + 
            this.$route.params.recipeId          
        );        
        // console.log("response.status", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }      
      let analyzedInstructions = [];
      response.data.prepInstructions.split("<li>").forEach(element => {
        analyzedInstructions.push(this.removeTags(element));        
      })
      analyzedInstructions = analyzedInstructions.filter(a =>{return a!=null && a.length>2});      
      let instructions = response.data.prepInstructions;
      let extendedIngredients = response.data.ingredients.split('&').filter(a =>{return a!=null && a.length>2});      
      let aggregateLikes = response.data.recepiePreview.popularity;
      let readyInMinutes = response.data.recepiePreview.prepTime;
      let image = response.data.recepiePreview.imageUri;
      let title = response.data.recepiePreview.title;
      let isLiked = response.data.recepiePreview.inFavorites;
      let _instructions = analyzedInstructions;
      let id = response.data.recepiePreview.id;
      let numOfDishes = response.data.numberOfDishes;
      instructions = instructions.replaceAll('.', '.<br>');
      this.recipePrev  = response.data.recepiePreview;

      let _recipe = {
        instructions,
        _instructions,
        analyzedInstructions,
        extendedIngredients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
        isLiked,
        id,
        numOfDishes
      };
      console.log(response.data)
      this.recipe = _recipe;      
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
ul li {
  padding: 8px 16px;
  border-bottom: 1px solid rgb(9, 9, 9);
}
.imgTitle{
  text-align: center;
  font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
  padding-top: 2.5%;
}
.img {  
  display: block;
  margin-left: auto;
  margin-right: auto;
  border: 2px solid black;
  border-radius: 5px;
  width: 25%;
}
.watchLikes{
  left: 100px;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  width: 10%;
  display: flex;
  padding-top: 10px;
  font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}
.ingredInstruc{
  font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;  
  padding-top: 3.5%;

  display: flex;  
  
}
.instruc{
  margin-left: auto;
  margin-right: auto;
  padding-right: 3%;
  
  
  text-align: left;
  padding-left: 15%; 
  max-width: 80%;
  float: left;
}
.ingredItems{
  color:rgb(159, 94, 9);
}
.title{
  padding-left: 3px;
}
#like{
  cursor: pointer;  
}
.recipePrev{
  transform: scale(1.25);  
  font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
.previewWrapper{
  display: block;
  margin: auto;
  padding-top: 40px;  
  max-height: 400px;
  max-width: 280px;
  min-height: 370px;
  margin-right: 100px;
  margin-left: 100px;
}

/* .recipe-header{

} */
</style>
