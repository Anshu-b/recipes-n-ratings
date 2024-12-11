# Do Higher-Calorie Recipes recieve Higher Ratings on food.com? Why?

## Introduction
Food, a necessity in our day-to-day lives, has long throughout history been a cornerstone in human society. From keeping humanity alive and healthy, food is also the center of many familial, cultural, and religious traditions which still exist to unite communities of people all around the world today. With advances in modern communication technology, it has become much easier for people to publically share and review recipes via the internet, thanks to websites such as food.com. To get a better understanding of the relationships between recipes people share online and the reviews they recieve from other users, we decided to investigate 2 datasets, which were scraped from food.com. 

#### Recipes
The first dataset is `recipes`, which contains 12 columns (of information) and 83,782 rows (recipes). Here is a breakdown of what each column represents:

| Column             | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| `'name'`           | Name of the recipe [object]                                                |
| `'id'`             | Recipe ID [int]                                                           |
| `'minutes'`        | Time needed to prepare the recipe (in minutes) [int]                      |
| `'contributor_id'` | User ID of the recipe poster [int]                                         |
| `'submitted'`      | Date of recipe submission (YYYY-MM-DD) [object]                           |
| `'tags'`           | Food.com tags for the recipe [object]                                     |
| `'nutrition'`      | Nutrition information about the recipe (number of calories, total fat PDV, sugar PDV, sodium PDV, protein PDV, saturated fat PDV, carbohydrates PDV) [object] |
| `'n_steps'`        | Number of steps in the recipe [int]                                       |
| `'steps'`          | Detailed steps for the recipe [object]                                    |
| `'description'`    | User-written recipe description [object]                                  |
| `'ingredients'`    | Detailed ingredients for the recipe [object]                              |
| `'n_ingredients'`  | Number of ingredients for the recipe [int]                                |

<p></p>

#### Interactions
The second dataset is `interactions`, which contains 5 columns (of information) and 731,927 rows (reviews). Here is a breakdown of what each column represents:

| Column       | Description                                                   |
|--------------|---------------------------------------------------------------|
| `'user_id'`  | User ID of the person interacting [int]                       |
| `'recipe_id'`| ID of the recipe being reviewed [int]                         |
| `'date'`     | Date of the interaction [object]                              |
| `'rating'`   | Rating of the recipe (1 - 5, whole numbers only) [int]        |
| `'review'`   | Detailed review of the recipe [object]                        |

<p></p>

#### Identifying a Research Question
Kick-backed and relaxed while feasting on a combo plate from Punjabi Tandoor, Jeronimo & Ansh pondered what questions they could ask based on the available data. Hands lathered in delicious, creamy curry and buttery garlic naan, they asked: "Do high calorie foods also tend to have higher ratings?" 

#### Background
Before investigating the relationship betwen high-ratings and high calorie counts, it is important to identify what type of foods would likely be considered highly-rated. To do this, we researched online for survey-based results of America's most popular food items. As shown by [YouGov]('https://today.yougov.com/ratings/consumer/popularity/american-dishes/all)'s popularity-based rankings, we can see that the top 5 most popular American dishes include burgers, fries, grilled-cheese sandwiches, and other foods which are well-known to be very calorically dense. 

#### Hypothesis
We hypothesize that there is a positive correlation between calories and ratings for recipes of food.com. More specifically, we think that as the number of calories in a recipe increases, so does the average rating for that recipe. Furthermore, since calorie counts are dependent on individual macronutrients such as sugars, proteins, and carbohydrates, we predict that fat specifically has the highest correlation with higher ratings. 

#### Impact
Finding the results to this question can have significant impacts for companies such as food.com who profit off of user engagement, as well as the users of these types of sites. For the company, they can use the results of this study to change their reccomendation algorithms to prioritize recipes with certain nutrition trends. Additionally, it is important for users to beware of trends of like these, especially those with health problems, so they are aware of the fact that the most popular and highly-rated recipes on the website may not necessarily fit their dietary needs and restrictions.



## Data Cleaning & Exploratory Data Analysis
This section will cover the steps we took to make our data more manageable, and also the observations we made during the EDA process.

#### Merging the DataFrames
For ease in working with the data, we decided to left-merge the interactions dataframe onto the recipes dataframe. We specifically used left-merge to ensure all recipes in the dataset are preserved, regardless of the number of reviews it recieved. Merging the dataframe also allows us to map recipes to their reviews. Correspondingly, we also drop the duplicate column of 'id' and set 'recipe_id' to the index of our new DataFrame.

#### Adding Average Ratings Column
The first step replaces missing ratings (initially set as 0) with NaN to accurately handle missing data. It then calculates the average rating for each recipe by grouping the data by recipe ID. This average rating is mapped back to the main DataFrame as a new column. 

#### Optimizing DataFrame Indexing
The redundant column id is dropped, and the DataFrame index is set to recipe_id to streamline data operations.

#### Converting Timestamp Columns
The 'submitted' and 'date' columns are converted into datetime objects from strings. 

#### Processing Categorical Lists
Columns containing lists ('tags,' 'steps,' and 'ingredients') are cleaned by stripping unnecessary characters and converting them into Python lists. Additionally, a new column n_tags calculates the number of tags for each recipe.

#### Parsing Nutritional Information:
The 'nutrition' column, originally stored as a string, is transformed into a list of numeric values. A custom function separates the list of nutrition facts into individual columns, including calories, protein, and fat values. These new columns replace the original 'nutrition' column.

#### New & Updated Columns

| Column               | Description                                                                                 |
|-----------------------|---------------------------------------------------------------------------------------------|
| `'submitted'`        | Date the recipe was submitted [datetime object]                                             |
| `'tags'`             | List of descriptive tags associated with the recipe [list of strings]                       |
| `'steps'`            | Step-by-step instructions for preparing the recipe [list of strings]                        |
| `'ingredients'`      | List of ingredients used in the recipe [list of strings]                                    |
| `'date'`             | Date of the recipe review or interaction [datetime object]                                  |
| `'average_rating'`   | Average rating for the recipe across all reviews [float]                                    |
| `'n_tags'`           | Number of tags associated with the recipe [int]                                             |
| `'num_calories'`     | Total calorie content of the recipe [float]                                                 |
| `'total_fat_PDV'`    | Total fat as a percentage of daily value (PDV) [float]                                      |
| `'sugar_PDV'`        | Sugar content as a percentage of daily value (PDV) [float]                                  |
| `'sodium_PDV'`       | Sodium content as a percentage of daily value (PDV) [float]                                 |
| `'protein_PDV'`      | Protein content as a percentage of daily value (PDV) [float]                                |
| `'saturated_fat_PDV'`| Saturated fat as a percentage of daily value (PDV) [float]                                  |
| `'carbohydrates_PDV'`| Carbohydrates content as a percentage of daily value (PDV) [float]                          |


#### Numerical Column Statistics

| Statistic           | Minutes      | N_Steps   | N_Ingredients | N_Tags   | Rating    | Average_Rating | Num_Calories | Total_Fat_PDV | Sugar_PDV | Sodium_PDV | Protein_PDV | Saturated_Fat_PDV | Carbohydrates_PDV |
|----------------------|--------------|-----------|---------------|----------|-----------|----------------|--------------|---------------|-----------|------------|-------------|-------------------|-------------------|
| Count           | 234,000      | 234,429   | 234,429       | 234,429  | 219,393   | 231,652        | 234,429      | 234,429       | 234,429   | 234,429    | 234,429     | 234,429           | 234,429           |
| Mean            | 107.00       | 10.02     | 9.07          | 16.91    | 4.68      | 4.68           | 419.53       | 31.92         | 63.84     | 29.26      | 33.17       | 39.50             | 13.25             |
| Std             | 3293.00      | 6.44      | 3.82          | 6.90     | 0.71      | 0.50           | 583.22       | 55.39         | 210.47    | 129.56     | 47.78       | 76.01             | 25.75             |
| Min             | 0.00         | 1.00      | 1.00          | 1.00     | 1.00      | 1.00           | 0.00         | 0.00          | 0.00      | 0.00       | 0.00        | 0.00              | 0.00              |
| 25%             | 20.00        | 6.00      | 6.00          | 12.00    | 5.00      | 4.50           | 170.70       | 8.00          | 8.00      | 5.00       | 6.00        | 7.00              | 3.00              |
| 50% (Median)    | 35.00        | 9.00      | 9.00          | 16.00    | 5.00      | 4.86           | 301.10       | 20.00         | 22.00     | 15.00      | 18.00       | 22.00             | 8.00              |
| 75%             | 60.00        | 13.00     | 11.00         | 21.00    | 5.00      | 5.00           | 491.10       | 39.00         | 58.00     | 33.00      | 50.00       | 49.00             | 15.00             |
| Max             | 1,050,000.00 | 100.00    | 37.00         | 49.00    | 5.00      | 5.00           | 45,609.00    | 3464.00       | 30,260.00 | 29,338.00  | 4,356.00    | 6875.00           | 3007.00           |


#### Univariate Analysis

#### Bivariate Analysis

#### Interesting Aggregates
1. Ranking Top Contributors by Number of Recipes Posted
This step identifies the most active contributors on the platform by counting the number of recipes they have submitted and calculating the average rating and average number of ingredients per recipe per contributor. This provides insights into the activity levels of top users and their potential influence on the food.com recipe platform. Understanding top contributors can help identify trends in recipe popularity and quality linked to these users.

| Contributor ID | Recipe Count | Avg Rating | Avg Ingredients |
|----------------|--------------|------------|-----------------|
| 37449          | 3060         | 4.79       | 9.65            |
| 226863         | 2754         | 4.85       | 10.90           |
| 424680         | 2503         | 4.80       | 7.83            |
| ...            | ...          | ...        | ...             |
| 85414          | 1            | 5.00       | 13.00           |
| 1132508        | 1            | 4.00       | 7.00            |
| 2002289981     | 1            | 1.00       | 21.00           |
| **Total Rows** | **15,443**   |            |                 |


2. Recipe Rating Compared to Recipe Complexity Factors
The relationship between recipe ratings and complexity factors like preparation time (minutes), the number of steps, and the number of ingredients is grouped. For each rating, the mean and median values of these factors are computed. This shows whether high-rated recipes are simpler or more complex. It also aids in understanding the trade-offs between convenience and quality from a user perspective.

| Rating | Avg Minutes | Median Minutes | Avg Steps | Median Steps | Avg Ingredients | Median Ingredients |
|--------|-------------|----------------|-----------|--------------|------------------|---------------------|
| 1.0    | 99.67       | 40.0           | 10.63     | 9.0          | 8.91            | 8.0                 |
| 2.0    | 98.02       | 40.0           | 10.70     | 9.0          | 9.23            | 9.0                 |
| 3.0    | 87.50       | 40.0           | 9.99      | 9.0          | 9.20            | 9.0                 |
| 4.0    | 91.59       | 35.0           | 9.58      | 9.0          | 9.10            | 9.0                 |
| 5.0    | 106.92      | 35.0           | 9.98      | 9.0          | 9.05            | 9.0                 |


3. Recipe Rating Versus Nutrition Facts
This aggregate plots the relationship between recipe ratings and their nutritional content. By grouping the dataset by rating and calculating both the mean and median of various nutrition-related columns (such as calories, fat, sugar, sodium, protein, saturated fat, and carbohydrates), we can learn how the nutritional content varies across different rating levels. This allows us to identify whether higher-rated recipes tend to be healthier or have specific nutritional characteristics (e.g., lower sugar or fat content) compared to lower-rated ones, which is our main research question. 

| Rating | Num Calories (Mean) | Num Calories (Median) | Total Fat PDV (Mean) | Total Fat PDV (Median) | Sugar PDV (Mean) | Sugar PDV (Median) | Sodium PDV (Mean) | Sodium PDV (Median) | Protein PDV (Mean) | Protein PDV (Median) | Saturated Fat PDV (Mean) | Saturated Fat PDV (Median) | Carbohydrates PDV (Mean) | Carbohydrates PDV (Median) |
|--------|---------------------|-----------------------|-----------------------|------------------------|-------------------|---------------------|--------------------|----------------------|---------------------|------------------------|--------------------------|----------------------------|--------------------------|----------------------------|
| 1.0    | 486.60              | 316.0                 | 37.06                 | 19.0                   | 46.68             | 22.0                | 16.40              | 9.0                  | 2.0                 | 3.0                    | 0.00                     | 0.00                       | 13.50                    | 9.0                       |
| 2.0    | 446.60              | 326.3                 | 32.77                 | 20.0                   | 42.88             | 24.0                | 15.04              | 10.0                 | 3.0                 | 5.0                    | 8.0                      | 0.00                       | 11.65                    | 8.0                       |
| 3.0    | 425.79              | 309.6                 | 31.64                 | 20.0                   | 40.09             | 21.0                | 13.68              | 9.0                  | 1.0                 | 4.0                    | 9.0                      | 1.0                       | 15.47                    | 6.0                       |
| 4.0    | 405.05              | 302.0                 | 29.94                 | 19.0                   | 36.43             | 20.0                | 12.83              | 9.0                  | 6.0                 | 7.0                    | 5.0                      | 6.0                       | 12.84                    | 9.0                       |
| 5.0    | 415.21              | 298.2                 | 31.79                 | 20.0                   | 39.23             | 22.0                | 13.04              | 8.0                  | 2.0                 | 1.0                    | 8.0                      | 7.0                       | 13.57                    | 8.0                       |


4. Pivot Table of Tags vs. Ratings
We created a pivot table to explore the relationship between recipe hashtags and ratings. By exploding the tags column and aggregating the count of recipes per tag and rating, this table shows how certain recipe categories are rated. 

| Tags                  | Rating 1.0 | Rating 2.0 | Rating 3.0 | Rating 4.0 | Rating 5.0 |
|-----------------------|------------|------------|------------|------------|------------|
| 0                     | 3          | 2          | 24         | 186        |            |
| 1-day-or-more         | 23         | 25         | 39         | 175        | 1174       |
| 15-minutes-or-less    | 498        | 351        | 1238       | 7526       | 36749      |
| ...                   | ...        | ...        | ...        | ...        | ...        |
| yams-sweet-potatoes   | 9          | 17         | 46         | 263        | 1220       |
| yeast                 | 68         | 38         | 82         | 419        | 2675       |
| zucchini              | 3          | 2          | 10         | 56         | 367        |
| **Total Rows**        | **544**    |            |            |            |            |


## Hypothesis Testing



## Framing a Prediction

## Baseline Model

## Final Model

## Fairness Model

