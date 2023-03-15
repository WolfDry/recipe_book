<script>

import RecipeBook from './components/RecipeBook.vue'

export default {
  data() {
    return {
      recipes: [
        {
          title: 'Winter vegetables soup',
          description: "A delicious soup made with winter vegetables.It's a great way to use up leftover vegetables.To prepare this soup, you will need a large pot, a knife, a cutting board, a spoon, a ladle, and a vegetable peeler.Peel the carrots and the potatoes.Cut the carrots and the potatoes into small cubes.Cut the onion into small cubes.Cut the celery into small cubes.Cut the leek into small cubes.Cut the garlic into small cubes. ...",
          ingredients: [
            {
              name: '1 onion'
            },
            {
              name: '2 carrots'
            },
            {
              name: '3 potatoes'
            }
          ]
        },
        {
          title: 'Burgundy beef stew',
          description: "A delicious soup made with winter vegetables.It's a great way to use up leftover vegetables.To prepare this soup, you will need a large pot, a knife, a cutting board, a spoon, a ladle, and a vegetable peeler.Peel the carrots and the potatoes.Cut the carrots and the potatoes into small cubes.Cut the onion into small cubes.Cut the celery into small cubes.Cut the leek into small cubes.Cut the garlic into small cubes. ...",
          ingredients: [
            {
              name: '1 onion'
            },
            {
              name: '2 carrots'
            },
            {
              name: '3 potatoes'
            }
          ]
        },
        {
          title: 'Nassi goreng',
          description: "A delicious soup made with winter vegetables.It's a great way to use up leftover vegetables.To prepare this soup, you will need a large pot, a knife, a cutting board, a spoon, a ladle, and a vegetable peeler.Peel the carrots and the potatoes.Cut the carrots and the potatoes into small cubes.Cut the onion into small cubes.Cut the celery into small cubes.Cut the leek into small cubes.Cut the garlic into small cubes. ...",
          ingredients: [
            {
              name: '1 onion'
            },
            {
              name: '2 carrots'
            },
            {
              name: '3 potatoes'
            }
          ]
        },
      ],
      newRecipe: {},
      updateRecipe: {},
      indexEdit: '',
      newIngredient: {},
      shoppingList: [],
      checkIngredients: []
    }
  },
  methods: {
    addRecipe: function () {
      if (this.newRecipe.title === ' ' || this.newRecipe.title === ' ' || this.newRecipe.title === undefined)
        return
      this.newRecipe = {
        title: this.newRecipe.title,
        description: '',
        ingredients: []
      }
      this.recipes.push(this.newRecipe)
      this.newRecipe = {}
    },
    deleteRecipe: function (recipe) {
      const newRecipes = this.recipes.filter(element => element != recipe)
      this.recipes = newRecipes
      this.updateRecipe = {}
    },
    displayEdit: function (index) {
      this.updateRecipe = this.recipes[index]
      this.indexEdit = index
    },
    editRecipe: function () {
      this.recipes[this.indexEdit] = this.updateRecipe
      this.updateRecipe = {}
    },
    deleteIngredient: function (ingredient) {
      const recipe = this.updateRecipe
      const newIngredients = recipe.ingredients.filter(element => element != ingredient)
      for (let index = 0; index < this.recipes.length; index++) {
        const element = this.recipes[index];
        if (element === recipe) {
          element.ingredients = newIngredients
        }
      }
    },
    addIngredient: function () {
      const recipe = this.updateRecipe
      if (this.newIngredient.name === ' ' || this.newIngredient.name === ' ' || this.newIngredient.name === undefined || !recipe)
        return
      for (let index = 0; index < this.recipes.length; index++) {
        const element = this.recipes[index];
        if (element === recipe) {
          element.ingredients.push(this.newIngredient)
        }
      }
      this.newIngredient = {}
    },
    addShoppingList: function () {
      const ingredients = this.updateRecipe.ingredients
      if (ingredients) {
        for (let index = 0; index < ingredients.length; index++) {
          const element = ingredients[index];
          this.shoppingList.push(element)
        }
      }
    },
    deleteAllShoppingList: function () {
      this.shoppingList = []
      this.checkIngredients = []
    },
    checkAll: function () {
      this.checkIngredients = []
      for (let index = 0; index < this.shoppingList.length; index++) {
        this.checkIngredients.push(index)
      }
    },
    deleteCheckIngredients: function () {
      const ingredients = this.checkIngredients
      if (this.shoppingList) {
        const newShoppingList = this.shoppingList.filter(function (value, index) {
          return !(index in ingredients)
        })
        this.shoppingList = newShoppingList
        this.checkIngredients = []
      }
    }
  },
  components: {
    RecipeBook
  }
}

</script>

<template>
  <div className="main">

    <h1 className="main-title">Recipe Book</h1>

    <!-- Recipe creation -->
    <form @submit.prevent="addRecipe" className="new-recipe-form">
      <input v-model="newRecipe.title" type=" text" className="recipe-title-input" placeholder="Add a new recipe..." />
      <button className="recipe-create-button">Add</button>
    </form>

    <!-- Recipe book -->
    <RecipeBook :deleteRecipe="deleteRecipe" :displayEdit="displayEdit" :recipes="recipes" />

    <!-- Recipe edit -->
    <div className="recipe-edit-form">
      <input type="text" className="recipe-edit-title-input" v-model="updateRecipe.title" />
      <textarea rows={10} className="recipe-edit-description-input" v-model="updateRecipe.description" />
      <h2 className="recipe-ingredients-title">Ingredients</h2>
      <ul className="recipe-ingredients-list">
        <li v-for="(ingredient, index) in updateRecipe.ingredients" className="recipe-ingredient">
          <input type="text" className="recipe-ingredient-input" v-model="ingredient.name" />
          <button @click="deleteIngredient(ingredient)" className="recipe-ingredient-delete-button">🗑</button>
        </li>
        <form @submit.prevent="addIngredient" className="recipe-new-ingredient">
          <input v-model="newIngredient.name" type="text" className="recipe-new-ingredient-input"
            placeholder="Add a new ingredient..." />
          <button className="recipe-new-ingredient-button">Add</button>
        </form>
      </ul>
      <div className="recipe-edit-actions">
        <button @click="editRecipe" className="recipe-edit-save-button">Save</button>
        <button @click="addShoppingList" className="recipe-edit-cart-button">Add to shopping list</button>
      </div>
    </div>
    <!--  Shopping list -->
    <div className="shopping-list">
      <h2 className="shopping-list-title">Shopping list</h2>
      <ul className="recipe-ingredients-list">
        <li v-for="(ingredient, index) in shoppingList" className="recipe-ingredient">
          <label className="shopping-list-item">
            <input v-model="checkIngredients" :value="index" type="checkbox" />
            {{ ingredient.name }}
          </label>
        </li>
      </ul>
      <div className="shopping-list-actions">
        <button @click="checkAll" className="shopping-list-clear-button">Check all</button>
        <button @click="deleteCheckIngredients" className="shopping-list-clear-button">Clear checked items</button>
        <button @click="deleteAllShoppingList" className="shopping-list-clear-button">Clear all</button>
      </div>
    </div>
  </div>
</template>