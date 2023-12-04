
## Recipe Config

#### Required Properties
- Ingredients
- tags
#### Recipes with broken config
```dataviewjs
const recipeBook = dv.pages('"Kitchen/RecipeBook"');
let brokenRecipes = [];

recipeBook.forEach(recipe => {
    let missingProperties = [];
    if (!recipe.hasOwnProperty('Ingredients')) {
        missingProperties.push("Ingredients");
    }
     if (!recipe.hasOwnProperty('tags')) {
        missingProperties.push("tags");
    }

    if (missingProperties.length > 0) {
        brokenRecipes.push([recipe.file.link, missingProperties]);
    }
});

dv.table(
    ["Recipe", "Missing Properties"],
    brokenRecipes
);
```

