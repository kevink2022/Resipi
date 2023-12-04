---
Ingredients:
  - Cream Cheese
  - Dry Yeast
  - Garlic
  - Mini Semisweet Chocolate Chips
  - Olive Oil
  - Oregano
  - Parsley
  - Pepper
  - Salt
  - Sugar
  - Tomato Paste
  - Vanilla Extract
  - Water
  - Almond Flour
  - Basil
  - Dried Herbs
  - Crushed Tomatoes
  - Chocolate
  - Eggs
  - Flour
  - Parmesan
  - Panko Breadcrumbs
  - Red Wine
  - Butter
  - Sodium Citrate
  - Semi-Firm Cheese
  - Dried Elbow Pasta
  - Honey
  - Hard Grating Cheese
  - Oil
  - Yogurt
  - Baking Powder
GroceryList:
  - Bread Flour
  - Cream Cheese
  - Garlic Powder
  - Onion
  - Pecorino Romano
  - Ricotta Cheese
  - Milk
  - Cocoa Powder
  - Cream of Tartar
  - Powdered Sugar
  - Corn Syrup
  - Cornmeal
  - Cornstarch
Filtered: true
Recipes: 
tags:
  - sipi-dough
---
Above, configure the ingredients you have in the `Ingredients` property, and the ingredients you plan to buy in the `GroceryList` property. 

By default, all the recipes in your RecipeBook will be included in you queries. However, if you check the box of the `Filtered` property above, it will the properties below it can be used to only query certain recipes or certain tags. The tags refer to the tags property your recipes will have. 

The tags can be very powerful, for example, if you want something Italian for dinner, you can filter by #italian  and #dinner  to see which Italian recipes you can make for dinner. Of course, you have to tag your recipes before they will show up in the filters.

### Recipes you can make with what you have
```dataviewjs

//######################################################################
const recipeBookGlobal = dv.pages('"Kitchen/RecipeBook"');
const cupboardHave = dv.page("Kitchen/Planner").Ingredients;
const filtered = dv.page("Kitchen/Planner").Filtered;
let recipeBook = [];

const hasRecipeFilter = dv.page("Kitchen/Planner").Recipes?.length > 0;
const hasTagFilter = dv.page("Kitchen/Planner").tags?.length > 0;
// Add other flags for new filters, like prep time

// First, check if no filters are active
if (!filtered || !hasRecipeFilter && !hasTagFilter /* && other filter flags */) {
    recipeBook = recipeBookGlobal;
} else {
    // Apply filters only if there are any active filters
    recipeBookGlobal.forEach(recipe => {

        console.log(recipe.file.name);
        if ((hasRecipeFilter  && dv.page("Kitchen/Planner").Recipes.includes(recipe.file.name)) ||
            (hasTagFilter     && dv.page("Kitchen/Planner").tags.some(tag => recipe.tags?.includes(tag))) //||
            // Add other filter checks here
            ) {
            recipeBook.push(recipe);
        }
    });
}

//######################################################################
let canMake = [];

recipeBook.forEach(recipe => {
  let recipeLink = recipe.file.link;
  let recipeIngredients = recipe.file.frontmatter.Ingredients;

  // Check if all recipe ingredients are in the cupboard
  let isAvailableInCupboard = recipeIngredients.every(ingredient => cupboardHave.includes(ingredient));

  if (isAvailableInCupboard) {  
    canMake.push([recipeLink]);
  }
});

dv.table(["Recipe"], canMake);

//######################################################################
dv.paragraph(`
### Recipes you can make with your grocery list
`);

const groceryList = dv.page("Kitchen/Planner").GroceryList;
const cupboardAndList = [...cupboardHave, ...groceryList];
let canMakeWithList = [];

recipeBook.forEach(recipe => {
  let recipeLink = recipe.file.link;
  let recipeIngredients = recipe.file.frontmatter.Ingredients;

  // Check if all recipe ingredients are in either the cupboard or grocery list
  let isAvailableInCupboard = recipeIngredients.every(ingredient => cupboardHave.includes(ingredient));
  let isAvailableInList = recipeIngredients.every(ingredient => cupboardAndList.includes(ingredient));

  if (!isAvailableInCupboard && isAvailableInList) {
    canMakeWithList.push([recipeLink]);
  }
});

dv.table(["Recipe"], canMakeWithList);

//######################################################################
dv.paragraph(`
### Recipes that require the least amount of additional ingredients
`);

let couldMake = [];
let oneMore = [];

recipeBook.forEach(recipe => {
  let recipeLink = recipe.file.link;
  let recipeIngredients = recipe.file.frontmatter.Ingredients;
  let missingIngredients = recipeIngredients.filter(name => !cupboardAndList.includes(name));
  let count = missingIngredients.length;

  if (count) {
    couldMake.push([recipeLink, count, missingIngredients]);
  }

  if (count == 1) {
    let oneIng = missingIngredients[0];
    let existingEntry = oneMore.find(entry => entry[0] === oneIng);

    if (existingEntry) {
      existingEntry[1].push(recipeLink);
    } else {
      oneMore.push([oneIng, [recipeLink]]);
    }
  }
});

couldMake.sort((a, b) => a[1] - b[1]); // Ascending
oneMore.sort((a, b) => b[1].length - a[1].length); // Descending

dv.table(["Recipe", "Count", "Ingredients"], couldMake);
dv.paragraph(
`
### Single ingredients that unlock the most recipes
`);
dv.table(["Ingredient", "Recipes"], oneMore);

dv.paragraph("### Filter testing");

//dv.list(recipeBook.file.link);

```



#### *Tips and Tricks*
- 
- You can save filters by using source mode and copying them to another file, then copying them back.
- The tags (along with all the other properties) are global throughout your entire obsidian vault. If you plan on using your vault for other purposes ([you should](https://www.youtube.com/watch?v=DbsAQSIKQXk)), and plan on using tags in your vault (you should), a good way to keep you tags organized is to prefix all your tags. For example, with #food-italian and #food-dinner, just typing #food- will show all of your different tags with autofill.