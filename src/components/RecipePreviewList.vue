<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <div v-if="isVertical">
      <div v-if="this.recipeFives.length>0">
      <b-col v-for="(lst, index) in this.recipeFives" :key="index" >
      <b-row v-for="(r) in lst" :key="r.id">
        <div>
          <RecipePreview class="recipePreview" :recipe="r" />         
        </div>        
      </b-row>
      </b-col>
      </div>
      
      <div v-else>
          <div v-if="waiting">
            <div>
              <b-spinner class="m-5" label="Busy"></b-spinner>
            </div>
          </div>
          <div v-else>
            No recipes to show...
          </div>          
      </div>
    </div>
    <div v-else>
      <div v-if="this.recipeFives.length>0">
      <b-row v-for="(lst, index) in this.recipeFives" :key="index" >
      <b-col v-for="(r) in lst" :key="r.id">
        
        <RecipePreview class="recipePreview" :recipe="r" />

      </b-col>
      </b-row>
      </div>
      
      <div v-else>
          <div v-if="waiting">
            <div>
              <b-spinner class="m-5" label="Busy"></b-spinner>
            </div>
          </div>
          <div v-else>
            No recipes to show...
          </div>          
      </div>   
    </div>
    
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    source:{
      type: String,
      required: true
    },
    isVertical:{
      type: Boolean,
      required: false
    },
    sort:{
      type: String,
      required: false
    }
  },
  data() {
    return {
      recipes: [],
      waiting: false
    };
  },
  mounted() {
    this.updateRecipes();    
  },
  methods: {
    async updateRecipes() {
      try {
        this.waiting = true;
        const response = await this.axios.get(
          this.$root.store.server_domain + this.source, 
          {withCredentials: true}
        );

        const recipes = response.data;
        if(this.sort == "Popularity (descending)")
          recipes.sort((rec1, rec2) => rec1.popularity < rec2.popularity ? 1 : -1);    
        else if(this.sort == "Preperation time (ascending)")
          recipes.sort((rec1, rec2) => rec1.prepTime > rec2.prepTime ? 1 : -1);    

        this.recipes = [];
        this.recipes.push(...recipes);
        // one big array
        
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
      this.waiting = false;
    },
    async sortRecipes(){
      if(this.sort == "Popularity (descending)")
          this.recipes.sort((rec1, rec2) => rec1.popularity < rec2.popularity ? 1 : -1);    
      else if(this.sort == "Preperation time (ascending)")
        this.recipes.sort((rec1, rec2) => rec1.prepTime > rec2.prepTime ? 1 : -1);   
    },
    async handleLike(r){      
      if(!this.$root.store.username) this.$parent.showModal("You need to log in first!");
      else if(!r.inFavorites){        
        await this.axios.post(this.$root.store.server_domain + "/Users/FavoriteRecipes",
          {
            "recipeId": r.id
          }
        ).then((res)=>{
          r.inFavorites = true;
          r.popularity +=1;
        }).catch((err) => {this.$parent.showModal("Cannot like recipe.. something went wrong")});      
      }            
      else{
        this.$parent.showModal("Recipe already liked!");
      }      
    }
  },
  computed:{
    recipeFives:{
      get: function(){  
        this.sortRecipes();      
        let recipesFives = [];
        let fives = -1;
        for(const recipe of this.recipes){
          let index = this.recipes.indexOf(recipe);
          if(index%5 == 0){
            fives+=1;
            recipesFives.push([])
          }
          recipesFives[fives].push(recipe);
        }
        return recipesFives;
      }
    }
  }
};
</script>

<style lang="scss" scoped>

.recipePreview{

  max-height: 400px;
  max-width: 280px;
  min-height: 370px; // this to align images
}
.recipePreview:hover {
  
}
.likes{
  position: relative;
  left: 75%;
  bottom: 14.5%;
  
}
.likesClick:hover{
  cursor: pointer;
  transform: scale(1.07);
}
</style>
