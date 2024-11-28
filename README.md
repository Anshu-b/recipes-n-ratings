# Do Higher-Calorie Recipes recieve Higher Ratings on food.com? Why?

## Introduction
Food, a necessity in our day-to-day lives, has long throughout history been a cornerstone in human society. From keeping humanity alive and healthy, food is also the center of many familial, cultural, and religious traditions which still exist to unite communities of people all around the world today. With advances in modern communication technology, it has become much easier for people to publically share and review recipes via the internet, thanks to websites such as food.com. To get a better understanding of the relationships between recipes people share online and the reviews they recieve from other users, we decided to investigate 2 datasets, which were scraped from food.com. 

#### Recipes
The first dataset is `recipes`, which contains 12 columns (of information) and 83,782 rows (recipes). Here is a breakdown of what each column represents:

| Column             | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| `'name'`           | Name of the recipe [string]                                                |
| `'id'`             | Recipe ID [int]                                                           |
| `'minutes'`        | Time needed to prepare the recipe (in minutes) [int]                      |
| `'contributor_id'` | User ID of the recipe poster [int]                                         |
| `'submitted'`      | Date of recipe submission (YYYY-MM-DD) [string]                           |
| `'tags'`           | Food.com tags for the recipe [string]                                     |
| `'nutrition'`      | Nutrition information about the recipe (number of calories, total fat PDV, sugar PDV, sodium PDV, protein PDV, saturated fat PDV, carbohydrates PDV) [string] |
| `'n_steps'`        | Number of steps in the recipe [int]                                       |
| `'steps'`          | Detailed steps for the recipe [string]                                    |
| `'description'`    | User-written recipe description [string]                                  |
| `'ingredients'`    | Detailed ingredients for the recipe [string]                              |
| `'n_ingredients'`  | Number of ingredients for the recipe [int]                                |

#### Interactions
The second dataset is `interactions`, which contains 5 columns (of information) and 731,927 rows (reviews). Here is a breakdown of what each column represents:

| Column       | Description                                                   |
|--------------|---------------------------------------------------------------|
| `'user_id'`  | User ID of the person interacting [int]                       |
| `'recipe_id'`| ID of the recipe being reviewed [int]                         |
| `'date'`     | Date of the interaction [string]                              |
| `'rating'`   | Rating of the recipe (1 - 5, whole numbers only) [int]        |
| `'review'`   | Detailed review of the recipe [string]                        |

#### Identifying a Research Question
Kick-backed and relaxed while feasting on a combo plate from Punjabi Tandoor, Jeronimo & Ansh pondered what questions they could ask based on the available data. Hands lathered in delicious, creamy curry and buttery garlic naan, they asked: "Do high calorie foods also tend to have higher ratings?" 

#### Background
Before investigating the relationship betwen high-ratings and high calorie counts, it is important to identify what type of foods would likely be considered highly-rated. To do this, we researched online for survey-based results of America's most popular food items. As shown by [YouGov]('https://today.yougov.com/ratings/consumer/popularity/american-dishes/all)'s popularity-based rankings, we can see that the top 5 most popular American dishes include burgers, fries, grilled-cheese sandwiches, and other foods which are well-known to be very calorically dense. 

#### Hypothesis
We hypothesize that there is a positive correlation between calories and ratings for recipes of food.com. More specifically, we think that as the number of calories in a recipe increases, so does the average rating for that recipe. Furthermore, since calorie counts are dependent on individual macronutrients such as sugars, proteins, and carbohydrates, we predict that fat specifically has the highest correlation with higher ratings. 

#### Impact
Finding the results to this question can have significant impacts for companies such as food.com who profit off of user engagement, as well as the users of these types of sites. For the company, they can use the results of this study to change their reccomendation algorithms to prioritize recipes with certain nutrition trends. Additionally, it is important for users to beware of trends of like these, especially those with health problems, so they are aware of the fact that the most popular and highly-rated recipes on the website may not necessarily fit their dietary needs and restrictions.


## Data Cleaning & Exploratory Data Analysis

## Hypothesis Testing

## Framing a Prediction

## Baseline Model

## Final Model

## Fairness Model

