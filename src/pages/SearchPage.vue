<template>
  <div>
    <div>
      <b-form @submit.stop.prevent> <br>
        <label for="text-password">Insert your search, and the filters you would like</label>
        <b-form-input placeholder="Any search" v-model="selection.search_string" id="querystring" ></b-form-input>
        <b-form-text id="help-block">
          In case of adding filtering the search will only consist of spooncular results!
        </b-form-text>
      </b-form>
    </div>
    <div id="dropdowns">
    <multiselect
      v-model="selection.selected_cuisines"
      :options="options.cuisines"
      placeholder="No cuisines to exclude"
      multiple
      size="lg">      
    </multiselect>
    <multiselect
      v-model="selection.selected_intolerances"
      :options="options.intolerance"
      placeholder="No intolerances"
      multiple>      
    </multiselect>
        <multiselect
      v-model="selection.selected_diet"
      :options="options.diets"
      placeholder="Any diet"
      >      
    </multiselect>
    <multiselect
      v-model="selection.selected_amount"
      :options="[5,10,15]"
      placeholder="select amount of results"
      :allowEmpty="false"
      >      
    </multiselect>
    <div>    
      <b-button @click="Submit" variant="success" >Search</b-button>
    </div>
    </div>    
    <div>
      <RecipePreviewList id="searchResult" v-if="did_search"
      title="Search Results" :source= 'search_query'
      :class="{
        RandomRecipes: true,
        blur: !$root.store.username,
        center: true
      }"
      disabled
    ></RecipePreviewList>
    </div>
  </div>
</template>

<script>
  import Multiselect from 'vue-multiselect'
  import RecipePreviewList from '../components/RecipePreviewList.vue';
  export default {
    components: { Multiselect, RecipePreviewList },
    data () {
      return {      
        options:{
          cuisines:[],
          diets: [],
          intolerance : [],
        },        
        selection:{
            selected_diet: '',
            selected_cuisines:[],
            selected_intolerances:[],
            selected_amount: 5,
            search_string: ''
        },        
        did_search: false,
        search_query: ''     
      }
    },
    mounted () {
        this.getOptions();
        this.search_query = localStorage.getItem("search_query")
        if(this.search_query!=null) this.did_search = true;
        else this.search_query = ""
    },
    methods:
    {
      async getOptions(){
        try {
          const response = await this.axios.get(
            this.$root.store.server_domain + '/Recipes/Options/RecipeSearch', 
            {withCredentials: true}
          );
          const cuisines = response.data.cousines;
          const diets = response.data.diets;
          const intolerance = response.data.intolerances;
          this.options.cuisines = cuisines;
          this.options.diets = diets;
          this.options.intolerance = intolerance;
          // console.log(this.recipes);
        } catch (error) {
          console.log(error);
        }
      },
      Submit(){
        this.did_search = false;
        this.$nextTick(()=>{
          this.search_query = '/Recipes?amount='+this.selection.selected_amount+'&search='+this.selection.search_string;
        for(let cuisine of this.selection.selected_cuisines){
          this.search_query += ('&cousine=' + cuisine);
        }
        if(this.selection.selected_diet!='') this.search_query+=('&diet=' + this.selection.selected_diet)        
        for(let into of this.selection.selected_intolerances){
          this.search_query += ('&intollerance=' + into);
        }
        alert(this.search_query)
        var ls = localStorage.setItem("search_query", this.search_query);
        this.did_search = true;
        });
        
      }
    }    
  }
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css">

</style>