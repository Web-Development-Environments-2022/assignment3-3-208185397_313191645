<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <div v-if="isVertical">
      <div v-if="recipes.length>0">
      <b-col v-for="(lst, index) in recipes" :key="index" >
      <b-row v-for="(r) in lst" :key="r.id">
        
        <RecipePreview class="recipePreview" :recipe="r" />

      </b-row>
      </b-col>
      </div>
      
      <div v-else>
          No recipes to show...
      </div>
    </div>
    <div v-else>
      <div v-if="recipes.length>0">
      <b-row v-for="(lst, index) in recipes" :key="index" >
      <b-col v-for="(r) in lst" :key="r.id">
        
        <RecipePreview class="recipePreview" :recipe="r" />

      </b-col>
      </b-row>
      </div>
      
      <div v-else>
          No recipes to show...
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
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();    
  },
  methods: {
    async updateRecipes() {
      try {
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
        let recipesFives = [];
        let fives = -1;
        for(const recipe of recipes){
          let index = recipes.indexOf(recipe);
          if(index%5 == 0){
            fives+=1;
            recipesFives.push([])
          }
          recipesFives[fives].push(recipe);
        }
        this.recipes = recipesFives;
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  },
  
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
.recipePreview{

  max-height: 400px;
  max-width: 280px;
  min-height: 370px; // this to align images
}
.recipePreview:hover {
  transform: scale(1.07); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
}
</style>
