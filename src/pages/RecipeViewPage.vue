<template>
<div>
  <div v-if="recipe">
    <div class="imgTitle">
      <h1 style="padding-bottom: 1%; padding-top:3px">{{ recipe.title}}</h1>
      <img :src="recipe.image" class="img"/>
    </div>    
    <div class="content">
      <div class="watchLikes">        
        <div style="float:left;">
          <div>{{ recipe.aggregateLikes }} likes</div>
          <div @click="handleLike" id="like">
            <img src="./../assets/likes.png" style="width: 60%; "/><br>
            <small v-if="!recipe.isLiked">press to like</small>
            <small v-else>already liked <br>press to remove like</small>
          </div>            
        </div>
        <div style="float:right;">
          <div>{{ recipe.readyInMinutes }} min</div>  
          <img src="./../assets/watch.jpg" style="width: 60%; padding-top: 3px;"/>
        </div>        
      </div>
      <div class="ingredInstruc">
        <div class="ingred">
          <h3 style="padding-left: 6px; text-decoration: underline;">Ingredients:</h3>
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
export default {
  data() {
    return {
      recipe: null
    };
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
    handleLike(){
      if(!this.recipe.isLiked){
        console.log(this.recipe)
        this.axios.post(this.$root.store.server_domain + "/Users/FavoriteRecipes",
          {
            "recipeId": this.recipe.id
          }
        );      
      }
      this.recipe.isLiked = !this.recipe.isLiked;
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
      instructions = instructions.replaceAll('.', '.<br>');
      

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
        id
      };
      console.log(_recipe)
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
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  width: 10%;
  display: flex;
  font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}
.ingredInstruc{
  font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
  padding-left: 15%;
  padding-top: 3%;
  font-size: large;
  display: flex;  
}
.instruc{
  margin-left: auto;
  margin-right: auto;
  padding-right: 3%;
  
  
  text-align: left;
  padding-left: 200px; 
  
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
/* .recipe-header{

} */
</style>
