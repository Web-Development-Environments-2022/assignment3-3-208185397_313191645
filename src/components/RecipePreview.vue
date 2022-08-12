<template>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
    class="recipe-preview"
  >
    <div class="recipe-body">
      <img v-if="image_load" :src="recipe.imageUri" class="recipe-image" />
      <!--
<img v-else src="../assets/unavailableImage.jpg" class="recipe-image">
      -->
    </div>
    <div class="recipe-footer">
      <div :title="recipe.title" class="recipe-title" style="text-align:center">
        {{ recipe.title }}
      </div>
      <ul class="recipe-overview">
        <li>{{ recipe.prepTime }} minutes</li> 
        <img src="./../assets/watch.jpg" style="width: 13%; "/><br>   
        <li>{{ recipe.popularity }} likes</li>
        <img src="./../assets/likes.png" style="width: 15%; "/>        
      </ul>
      <ul class="recipe-overview">
        <li v-if="recipe.vegan">
          <img id="vegan"  src="../assets/vegan.png" width="30px" height="30px">
        </li>
        <li v-else>
          <img id="vegan"  src="../assets/novegan.png" width="30px" height="30px">
        </li>
        <li v-if="recipe.glutenFree">
          <img id="glutenFree"  src="../assets/glutenFree.jpg" width="30px" height="30px">
        </li>
        <li v-else>
          <img id="glutenFree"  src="../assets/noglutenFree.png" width="30px" height="30px">
        </li>
        <li v-if="recipe.alreadyWatched">
          <img id="eye" src="../assets/eye.png" width="30px" height="30px">
        </li>
        <li v-else>
          <img id="eye" src="../assets/noeye.png" width="30px" height="30px">
        </li>
      </ul>
    </div>
  </router-link>
</template>

<script>
export default {
  mounted() {
    this.axios.get(this.recipe.imageUri).then((i) => {
      this.image_load = true;      
    });
  },
  data() {
    return {
      image_load: false
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    }
  },
  methods: {
    like(recipe){
      alert(recipe);
    }
  }
};
</script>

<style scoped>
.recipe-preview {
  display: inline-block;
  width: 95%;
  height: 93%;
  position: relative;
  margin: 10px 10px;
  
  border: 2px solid black;
  border-radius: 5px;
  color: black;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 100%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
  color: black;
}

.recipe-preview .recipe-footer ul.recipe-overview ul.recipe-foodprop{
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: inline-block;
  text-align: center;

}

.recipe-body{
  
}
</style>
