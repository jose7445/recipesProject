<template>
  <div id="app">
    <div class="header">
      <img class="logo" alt="UOC logo" src="./assets/uoc-logo.png" />
      <div class="app-name">Recipe book</div>
    </div>
    <SearchBar v-on:showForm="toggleForm" v-on:search="setSearchTerm" />
    <RecipeList :items="recipeListFiltered" v-on:deleteRecipe="deleteRecipe" />
    <RecipeForm
      v-on:addRecipe="addRecipe"
      v-on:closeModal="toggleForm"
      v-if="showModal"
    />
  </div>
</template>

<script>
import { defineComponent } from "vue";
import RecipeForm from "./components/RecipeForm.vue";
import RecipeList from "./components/RecipeList.vue";
import SearchBar from "./components/SearchBar.vue";
import axios from "axios";

export default defineComponent({
  name: "App",
  components: {
    RecipeList: RecipeList,
    RecipeForm,
    SearchBar,
  },
  data() {
    return {
      items: [],
      showModal: false,
      searchTerm: "",
    };
  },

  async created() {
    try {
      const response = await axios.get("http://localhost:3000/recipes");
      this.items = response.data.recipes;
      console.log(response);
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    addRecipe(recipe) {
      this.items = [...this.items, recipe];
      this.toggleForm();
    },
    deleteRecipe(recipeId) {
      this.items = this.items.filter((recipe) => recipe.id !== recipeId);
    },
    toggleForm() {
      this.showModal = !this.showModal;
    },
    setSearchTerm(searchTerm) {
      this.searchTerm = searchTerm;
    },
  },

  computed: {
    recipeListFiltered() {
      if (!this.searchTerm) {
        return this.items;
      }
      return this.items.filter((recipe) => {
        return (
          recipe.title.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
          recipe.ingredients
            .map((ingredient) => ingredient.toLowerCase())
            .includes(this.searchTerm.toLowerCase())
        );
      });
    },
  },
});
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-wrap: wrap;
  height: 100%;
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
}
.header {
  width: 100%;
  padding: 15px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #ccc;
}
.header .logo {
  max-height: 50px;
}
.header .app-name {
  margin-left: 25px;
  font-weight: bold;
  font-size: 20px;
}
</style>
