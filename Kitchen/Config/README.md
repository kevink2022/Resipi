# Resipi
Track which recipes you can make, and which ingredients you need to make them, using Obsidian and the dataview plugin. While there is a lot of cool features and automations here, this will require some manual work from the user to keep everything working.

## To get started
1. Install [Obsidian](https://obsidian.md/)
2. Install the [Dataview](https://github.com/blacksmithgu/obsidian-dataview) plugin through Obsidian  \[Settings] -> \[Community Plugins] and search for Dataview
3. Allow Dataview javascript queries in Obsidian settings through \[Settings] -> \[Dataview] -> \[Enable Javascript Queries]
4. Download the `Kitchen` folder in this repository and add it to your obsidian vault.

### Adding New Recipes
1. **Create new recipes** by creating files in the `Kitchen/RecipeBook` directory. Feel free to organize your recipes into subfolders, they will still be picked up by the queries.
2. **Configure new recipes** by checking the `Kitchen/Config/ConfigurationChecks` file, which will list all the required and optional properties, along with any 'broken' Recipes which are missing properties. Broken recipes may break queries, so if any queries are returning error messages, check the ConfigChecks file. 
3. **Set up lists** by populating the `Ingredient` and `Recipe` properties in the `Kitchen/Config/IngredientsList` and `Kitchen/Config/RecipeList` files. While this isn't strictly necessary, it will ensure all your Recipes and Ingredients will autofill in the future, which on top of being generally helpful, will also help prevent duplication (ex. you have "Parmesan" but the recipe calls for "Parmesan Cheese" and Chicken Parm night is ruined).

# Documentation

#### RecipeBook
The directory where all the recipe files will be contained.
#### Cupboard
The directory where all the ingredient files will be contained.
#### Config
The directory where all the configuration files will be contained. This includes files like [[Kitchen/Config/ConfigChecks]], which will show the files that don't contain required/optional properties, and [[Kitchen/Config/GlobalList]] which are global lists of all the ingredients and recipes in the system.
#### Planner
This file is where you can plan out your future cooking by checking which recipes you can cook, and plan out which ingredients you will buy or recipes you will cook in the future. You can either use the global list of recipes (all the recipes in the RecipeBook directory), or filter by certain recipes, using the recipe names or various properties, such as tags and prep time.
#### *Journal*
*Not yet implemented* - The directory where you will record your *Journal Entries*, which will record your past cooking sessions, with the recipes you made. There you can note things your learned while cooking, and use those to make ingredient *substitutions* 
#### *Calendar*
*Not yet implemented* - This is where you can plan future cooking sessions and view past cooking sessions, codified in journal entries. You will then be able to use your planner to see the preparation that will need to be done to cook said recipes on said days.

## Common Problems
- **Tag not working**: If a tag is not working, check the source mode. Tags in the properties should be text only, but can sometimes be saved as `"#tag"` instead of `tag`. Obsidian will recognize both, and they'll look the same, but the queries won't recognize the former.


# Project Layout

- KITCHEN
	- CUPBOARD
		- *All your Ingredients*
	- RECIPEBOOK
		- *All your Recipes*
	- CONFIG
		- [[Documentation]]
		- **[[Kitchen/Config/ConfigChecks]]**
			- Recipe Config Check
		- **[[Kitchen/Config/GlobalList]]**
			- Missing from Ingredients
			- Missing from GroceryList
			- Missing from Recipes
			- Erroneous Recipes
			- Erroneous GroceryList Items
			- Missing Ingredient Pages
	- **[[Kitchen/Planner]]**
		- Recipes you can make with what you have
		- Recipes you can make with your grocery list
		- Recipes that require the least amount of additional ingredients
		- Single ingredients that unlock the most recipes

### Models

#### Recipe
- **Required Properties** (currently used)
	- Ingredients
	- Tags
- **Optional Properties** (to be used in future)
	- Active prep
	- Total prep
	- Title
	- Author

#### Ingredient
- **Required Properties** (currently used)
	- *None*
- **Optional Properties** (to be used in future)
	- Tags
	- Substitutions
	- Use by date
	- Macros
		- Carbs
		- Protein
		- Fat


## Roadmap
General order in which I plan on implementing things, subject not only to change, but to be totally ignored.
- [x] Recipe/Ingredient Querying
	- [x] Filtered Querying
		- [ ] Saved Filters
	- [ ] Optional Ingredients
	- [ ] Equipment
	- [ ] Prep/Active PrepTimes
- [x] Configuration Checks
	- [ ] Templates based on Config
- [ ] Quantities+Measurements
	- [ ] Weight/Volume Translations
	- [ ] Macros
- [ ] Calendar
- [ ] Substitutions
- [ ] Cooking Sessions
	- [ ] Photos + Videos
	- [ ] Timers?


