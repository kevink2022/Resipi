---
Ingredients:
  - Almond Flour
  - Baking Powder
  - Basil
  - Bread Flour
  - Butter
  - Chicken Breasts
  - Chocolate
  - Cocoa Powder
  - Cornmeal
  - Cream
  - Cream Cheese
  - Crushed Tomatoes
  - Dried Elbow Pasta
  - Dried Herbs
  - Dried Large Pasta Shells
  - Dry Yeast
  - Egg Whites
  - Eggs
  - Finishing Salt
  - Flour
  - Fresh Basil
  - Fresh Cilantro
  - Fresh Herbs
  - Garlic
  - Garlic Powder
  - Hard Grating Cheese
  - Honey
  - Large Onions
  - Milk
  - Mini Semisweet Chocolate Chips
  - Mozzarella Cheese
  - Oil
  - Olive Oil
  - Onion
  - Oregano
  - Panko Breadcrumbs
  - Parmesan
  - Parsley
  - Pecorino Romano
  - Pepper
  - Powdered Sugar
  - Red Wine
  - Ricotta Cheese
  - Salt
  - Semi-Firm Cheese
  - Sodium Citrate
  - Spaghetti
  - Sugar
  - Tomato Paste
  - Tomato Puree
  - Vanilla Extract
  - Water
  - White Wine
  - Whole Wheat Flour
  - Yogurt
  - Cake Flour
  - Corn Syrup
  - Cornstarch
  - Cream of Tartar
  - Egg Yolks
  - Greek Yogurt
  - Ice Cream
GroceryList:
  - Almond Flour
  - Baking Powder
  - Basil
  - Bread Flour
  - Butter
  - Chicken Breasts
  - Chocolate
  - Cocoa Powder
  - Cornmeal
  - Cream
  - Cream Cheese
  - Crushed Tomatoes
  - Dried Elbow Pasta
  - Dried Herbs
  - Dried Large Pasta Shells
  - Dry Yeast
  - Egg Whites
  - Eggs
  - Finishing Salt
  - Flour
  - Fresh Basil
  - Fresh Cilantro
  - Fresh Herbs
  - Garlic
  - Garlic Powder
  - Hard Grating Cheese
  - Honey
  - Large Onions
  - Milk
  - Mini Semisweet Chocolate Chips
  - Mozzarella Cheese
  - Oil
  - Olive Oil
  - Onion
  - Oregano
  - Panko Breadcrumbs
  - Parmesan
  - Parsley
  - Pecorino Romano
  - Pepper
  - Powdered Sugar
  - Red Wine
  - Ricotta Cheese
  - Salt
  - Semi-Firm Cheese
  - Sodium Citrate
  - Spaghetti
  - Sugar
  - Tomato Paste
  - Tomato Puree
  - Vanilla Extract
  - Water
  - White Wine
  - Whole Wheat Flour
  - Yogurt
  - Cake Flour
  - Corn Syrup
  - Cornstarch
  - Cream of Tartar
  - Egg Yolks
  - Greek Yogurt
  - Ice Cream
Recipes:
  - Mrs. K Red Sauce
  - Cannoli Dip
  - Pizza Dough
  - Chocolate Macaroons
  - Stuffed Shells and Garlic Bread
  - Chicken Parm
  - Pan Pizza
  - Stovetop Mac & Cheese
  - Focaccia
  - Oven Grate Naan
  - Baked Alaska
---
Global List contains all of the Recipes and their Ingredients in your RecipeBook. This provides autofill capabilities when using properties elsewhere such as creating new recipes and using the [[Kitchen/Planner]], while also helping prevent duplicate items. For example, when you have "Cheddar" but the recipe calls for "Cheddar Cheese," so the recipe doesn't show up as a recipe you can make. 

Basically, if all the queries here and in [[Projects/Portfolio/Git Repos/KitchenProject/Kitchen/Config/ConfigChecks]] return nothing, everything should be working fine. 

## Missing From Global Lists

```dataviewjs
const recipeBook = dv.pages('"Kitchen/RecipeBook"');
const ingredientGlobalList = dv.page("Kitchen/Config/GlobalList").Ingredients;
const ingredientBookList = dv.pages('"Kitchen/RecipeBook"').file.frontmatter.Ingredients;
const missingIngredients = ingredientBookList.filter(name => !ingredientGlobalList.includes(name));
let missingIngredientsList = [];

missingIngredients.forEach(ingredient => {
  
  let recipesIn = recipeBook.filter(recipe => recipe.file.frontmatter.Ingredients.includes(ingredient));
  
  if (missingIngredients.length > 0) {
    missingIngredientsList.push([ingredient, recipesIn.file.link]);
  }
});

dv.table(["Missing from Ingredients", "Recipes Included In"], missingIngredientsList);
```

```dataviewjs
const recipeConfigList = dv.page("Kitchen/Config/GlobalList").Recipes;
const recipeBookList = dv.pages('"Kitchen/RecipeBook"').file.name
const missingRecipes = recipeBookList.filter(name => !recipeConfigList.includes(name));
dv.table(["Missing from Recipes"], missingRecipes.map(i => [i]));
```

```dataviewjs
const ingredientGlobalList = dv.page("Kitchen/Config/GlobalList").Ingredients;
const groceryGlobalList = dv.page("Kitchen/Config/GlobalList").GroceryList;
const missingIngredients = ingredientGlobalList.filter(name => !groceryGlobalList.includes(name));
dv.table(["Missing from GroceryList"], missingIngredients.map(i => [i]));
```

## Erroneous Values
This checks for list entries that don't have a matching value elsewhere. Ingredients isn't tracked, allowing you to have ingredients that aren't in any recipes.
```dataviewjs
const recipeConfigList = dv.page("Kitchen/Config/GlobalList").Recipes;
const recipeBookList = dv.pages('"Kitchen/RecipeBook"').file.name
const missingRecipes = recipeConfigList.filter(name => !recipeBookList.includes(name));
dv.table(["Erroneous Recipes"], missingRecipes.map(i => [i]));
```

```dataviewjs
const ingredientGlobalList = dv.page("Kitchen/Config/GlobalList").Ingredients;
const groceryGlobalList = dv.page("Kitchen/Config/GlobalList").GroceryList;
const missingIngredients = groceryGlobalList.filter(name => !ingredientGlobalList.includes(name));
dv.table(["Erroneous GroceryList Items"], missingIngredients.map(i => [i]));
```

## Missing Ingredient Pages
*Not yet implemented, so don't worry about this one.* To support substitutions and macros, each ingredient needs its own page. This is going to look at the `Ingredients` property above, so un-configured ingredients will not appear here. 
```dataviewjs
const ingredientConfigList = dv.page("Kitchen/Config/GlobalList").Ingredients;
const ingredientPageList = dv.pages('"Kitchen/Cupboard"').file.name;
const missingPageList = ingredientConfigList.filter(name => !ingredientPageList.includes(name));

dv.list(missingPageList);
```

#### *Tips and Tricks*  
- The easiest way to add ingredients it just by pressing enter over and over again, as their presence in the Recipes will allow them to auto fill. 
- For adding Recipes and GroceryList, you can switch into "Source Mode" to see the properties in plain text form (formatted as [YAML Frontmatter](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/03+Linking+and+organizing/YAML+Frontmatter)). You can then just paste the entire list of missing items. Take care to match the formatting exactly, as having exact indenting in necessary in YAML.
- If you want to sort these lists in alphabetical orders, you can create a new file with these properties, and autofill will automatically be in alphabetical order. Then, you can just copy and paste that files frontmatter over this files frontmatter using source mode.

