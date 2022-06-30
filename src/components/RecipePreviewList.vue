<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
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
        this.recipes = [];
        this.recipes.push(...recipes);
        let recipesThrees = [];
        let threes = -1;
        for(const recipe of recipes){
          let index = recipes.indexOf(recipe);
          if(index%3 == 0){
            threes+=1;
            recipesThrees.push([])
          }
          recipesThrees[threes].push(recipe);
        }
        this.recipes = recipesThrees;
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
