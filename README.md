# Do Higher-Calorie Recipes recieve Higher Ratings on food.com?

## Introduction
Food, a necessity in our day-to-day lives, has long throughout history been a cornerstone in human society. From keeping humanity alive and healthy, food is also the center of many familial, cultural, and religious traditions which still exist to unite communities of people all around the world today. With advances in modern communication technology, it has become much easier for people to publically share and review recipes via the internet, thanks to websites such as food.com. To get a better understanding of the relationships between recipes people share online and the reviews they recieve from other users, we decided to investigate 2 datasets, which were scraped from food.com. 

#### Recipe
The first dataset is `recipes`, which contains 12 columns (of information) and 83,782 rows (recipes). Here is a breakdown of what each column represents:
| Column       | Description|
|--------------|--------------|
| `'name'`          |  name of the recipe [string]|
| `'id'`            |  recipe id [int]         | 
| `'minutes'`       | time needed to prepare recipe (in minutes) [int]| 
| `'contributor_id'`| user ID of recipe poster [int]| 
| `'submitted'`     | date of recipe submission (YYYY-MM-DD) [string]| 
| `'tags'`          | food.com tags for the recipe [string]| 
| `'nutrition'`     | nutrition information about the recipe (number of calories, total fat PDV, sugar PDV, sodium PDV, protein PDV, saturated fat PDV, carbohydrates PDV) [string]         | 
| `'n_steps'`       |  number of steps in the recipe [int]         | 
| `'steps'`         |  detailed steps for the recipe [string]         | 
| `'description'`   | user-written recipe description [string]        | 
| `'ingredients'`   |  detailed ingredients for the recipe [string]  | 
| `'n_ingredients'` |  number of ingredients for the recipe [int]     | 


The second dataset is `interactions`, which contains 5 columns (of information) and 731,927 rows (review). Here is a breakdown of what each column represents:
| Column       | Description|
|--------------|--------------|
| `'user_id'`  | user id of the person interacting [int]|
| `'recipe_id'`| id of recipe being reviewed [int]         | 
| `'date'`     | date of the interaction [string] |
| `'rating'`   | rating of the recipe (1 - 5, whole numbers only) [int] | 
| `'review'`   | detailed review of the recipe [string] | 



## Data Cleaning & Exploratory Data Analysis

## Hypothesis Testing

## Framing a Prediction

## Baseline Model

## Final Model

## Fairness Model

