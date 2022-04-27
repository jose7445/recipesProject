<template>
  <div id="recipe-list" class="recipe-list">
    <div v-for="recipe in items" :key="recipe.id">
      <recipeCard :recipe="recipe" v-on:delete-recipe="deleteRecipe" />
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import RecipeCard from "./RecipeCard.vue";
//import axios from "axios";
export default defineComponent({
  name: "RecipeList",
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  components: { RecipeCard },
  methods: {
    deleteRecipe(recipeId) {
      this.$emit("delete-recipe", recipeId);
    },
  },
});
</script>

<style scoped>
.recipe-list {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr 1fr 1fr;
}
</style>
