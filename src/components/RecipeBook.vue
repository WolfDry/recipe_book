<script>
export default {
    props: {
        recipes: Array,
        deleteRecipe: Function,
        displayEdit: Function
    }
}
</script>

<template>
    <!-- Recipe book -->
    <div className="recipe-book">
        <div class="recipe" v-for="(recipe, index) in recipes">
            <div className="recipe-title">{{ recipe.title }}</div>
            <div className="recipe-actions">
                <button @click="displayEdit(index)" className="recipe-edit-button">Edit</button>
                <button @click="deleteRecipe(recipe)" className="recipe-delete-button">Delete</button>
            </div>
        </div>
    </div>
</template>