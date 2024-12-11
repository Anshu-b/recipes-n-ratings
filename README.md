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

<p></p>

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

<p></p>


#### Univariate Analysis
# **PLOT**

#### Bivariate Analysis
# **PLOT**

#### Interesting Aggregates

# **OPTIONAL PLOT FOR ANY OF BELOW AGGREGATES**

**Ranking Top Contributors by Number of Recipes Posted**
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

<p></p>

**Recipe Rating Compared to Recipe Complexity Factors**
The relationship between recipe ratings and complexity factors like preparation time (minutes), the number of steps, and the number of ingredients is grouped. For each rating, the mean and median values of these factors are computed. This shows whether high-rated recipes are simpler or more complex. It also aids in understanding the trade-offs between convenience and quality from a user perspective.

| Rating | Avg Minutes | Median Minutes | Avg Steps | Median Steps | Avg Ingredients | Median Ingredients |
|--------|-------------|----------------|-----------|--------------|------------------|---------------------|
| 1.0    | 99.67       | 40.0           | 10.63     | 9.0          | 8.91            | 8.0                 |
| 2.0    | 98.02       | 40.0           | 10.70     | 9.0          | 9.23            | 9.0                 |
| 3.0    | 87.50       | 40.0           | 9.99      | 9.0          | 9.20            | 9.0                 |
| 4.0    | 91.59       | 35.0           | 9.58      | 9.0          | 9.10            | 9.0                 |
| 5.0    | 106.92      | 35.0           | 9.98      | 9.0          | 9.05            | 9.0                 |

<p></p>


**Recipe Rating Versus Nutrition Facts**
This aggregate plots the relationship between recipe ratings and their nutritional content. By grouping the dataset by rating and calculating both the mean and median of various nutrition-related columns (such as calories, fat, sugar, sodium, protein, saturated fat, and carbohydrates), we can learn how the nutritional content varies across different rating levels. This allows us to identify whether higher-rated recipes tend to be healthier or have specific nutritional characteristics (e.g., lower sugar or fat content) compared to lower-rated ones, which is our main research question. 

| Rating | Num Calories (Mean) | Num Calories (Median) | Total Fat PDV (Mean) | Total Fat PDV (Median) | Sugar PDV (Mean) | Sugar PDV (Median) | Sodium PDV (Mean) | Sodium PDV (Median) | Protein PDV (Mean) | Protein PDV (Median) | Saturated Fat PDV (Mean) | Saturated Fat PDV (Median) | Carbohydrates PDV (Mean) | Carbohydrates PDV (Median) |
|--------|---------------------|-----------------------|-----------------------|------------------------|-------------------|---------------------|--------------------|----------------------|---------------------|------------------------|--------------------------|----------------------------|--------------------------|----------------------------|
| 1.0    | 486.60              | 316.0                 | 37.06                 | 19.0                   | 46.68             | 22.0                | 16.40              | 9.0                  | 2.0                 | 3.0                    | 0.00                     | 0.00                       | 13.50                    | 9.0                       |
| 2.0    | 446.60              | 326.3                 | 32.77                 | 20.0                   | 42.88             | 24.0                | 15.04              | 10.0                 | 3.0                 | 5.0                    | 8.0                      | 0.00                       | 11.65                    | 8.0                       |
| 3.0    | 425.79              | 309.6                 | 31.64                 | 20.0                   | 40.09             | 21.0                | 13.68              | 9.0                  | 1.0                 | 4.0                    | 9.0                      | 1.0                       | 15.47                    | 6.0                       |
| 4.0    | 405.05              | 302.0                 | 29.94                 | 19.0                   | 36.43             | 20.0                | 12.83              | 9.0                  | 6.0                 | 7.0                    | 5.0                      | 6.0                       | 12.84                    | 9.0                       |
| 5.0    | 415.21              | 298.2                 | 31.79                 | 20.0                   | 39.23             | 22.0                | 13.04              | 8.0                  | 2.0                 | 1.0                    | 8.0                      | 7.0                       | 13.57                    | 8.0                       |

<p></p>


**Pivot Table of Tags vs. Ratings**
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

<p></p>

## Assessment of Missingness

#### Not-Missing-At-Random (NMAR) Analysis
We believe that the missingness of the 'review' column is Not Missing at Random (NMAR). This inference is based on the fact that submitting a review was not a required action when interacting with a recipe. Therefore, we assume that the absence of a review is influenced by user satisfaction with the recipe and whether they felt compelled to leave feedback. For example, users who give very low or high ratings may be more likely to leave a strong written review to expand more on why they passionately liked or disliked the recipe. Users who enjoyed a recipe but didn't have a strong opinion on it are less liekly to leave a reflective review.

#### Assessment of Missingness
First, we assessed the missingness of data within the dataset to understand how it might affect our analysis. Several columns have significant missing values including `'rating'`, `'review'`, and `'description'`, but the `'rating'` column had 6.41% missing data, making it the most important column to examine. We believe the missingness of the 'rating' is Missing at Random (MAR), meaning that the absence of ratings does not depend on the rating values themselves but rather might be influenced by other variables. 

To test this hypothesis, we perform a permutation test to evaluate if the missingness of ratings is dependent on other columns, namely `'num_calories'`, and `'minutes'`. We use difference in means since most it directly measures how the average values of a given continuous numerical column differ between the groups of missing and non-missing ratings. 

**Null hypothesis (H0):** The missingness of `'ratings'` does not depend on `'num_calories'`, and `'minutes'`. 
<p></p>
**Alternative hypothesis (H1):** The missingness of `'ratings'` does depend on `'num_calories'`, and `'minutes'`.  
<p></p>
**Test Statistic:** Difference in Means 
<p></p>
**Significance Level (α):** 0.05 
<p></p>
If the p-value is less than α, we reject H0, indicating that the missingness of the 'rating' column is dependent on the specific column. Otherwise, we fail to reject H0.

| Column         | p-value   | Dependent on Missingness (p < 0.05)? |
|----------------|-----------|--------------------------------------|
| num_calories   | 0.0       | True                                 |
| minutes        | 0.134     | False                                |

<p></p>

# **PLOT**
# **PLOT**

Based on the results of the permutation test, we identify that the missingness of `'rating'` is likely influenced by `'num_calories'` but not by `'minutes'`. 

## Hypothesis Testing
We decided to test whether there is a significant relationship between the number of calories in a recipe and its mean rating. We chose to use a permutation test because we do not have enough information to assume that the ratings or calories follow a normal distribution. 

**Null Hypothesis (H0):** There is no relationship between the number of calories and the mean rating of a recipe 
<p></p>
**Alternative Hypothesis (H1):** Higher-calorie recipes tend to have higher mean ratings than lower-calorie recipes. 
<p></p>
**Test Statistic:** Difference in Means 
<p></p>
**Significance Level (α):** 0.05 
<p></p>

Since the hypothesis is directional with numerical data, a difference in means test-statistic is the most appropriate. We reasoned that foods with higher calorie content, often rich in fats, sugars, and carbohydrates, are likely to be rated more favorably. For example, popular calorically-dense foods like pizza, cookies, and ice cream are often people's favorites, thus supporting the hypothesis that they would have higher ratings.

To perform the test, we first separated the recipes into two groups based on the median number of calories: high-calorie recipes (above the median) and low-calorie recipes (below the median). We then conducted the permutation test by calculating the difference in mean ratings between the two groups, using 1,000 permutations to generate a distribution of the test statistic under the null hypothesis. 

The **observed difference in means was found to be 0.158**, and the **p-value was 0.00**, which is less than the significance level. Therefore, we reject the null hypothesis, concluding that **there is a statistically significant relationship between higher calorie content and higher mean ratings for recipes**. This result suggests that, at least for this dataset from food.com, calorie-dense recipes are more likely to receive higher ratings, supporting the idea that certain ingredients contributing to higher calorie counts may influence overall recipe satisfaction.


## Framing a Prediction
We plan to predict the average rating of a recipe using a regression approach. Regression is appropriate here because the average rating is a continuous variable that is not distinctly catgeorical. To make predicitons, we will build a **linear regression model** that uses the **TF-IDF scores of the recipe's tags** and the **number of calories**. to predict the average rating. 

We chose the average rating as the response variable since it represents the consensus of user satisfaction with a recipe from those who reviewed it. Incorporating TF-IDF and calorie information is useful since our earlier analysis with hypothesis testing and EDA showed potential relationships between rating trends and these variables. 

To evaluate the model’s performance, we will use the **mean squared error (MSE)** as the metric. MSE is suitable because it penalizes larger prediction errors more heavily, ensuring that our model minimizes substantial deviations from actual ratings. We actually also used the **R-squared value** to measure how well the features collectively explain the variance in average ratings.

The features available for improving our prediction model include all columns in the dataset and potentially engineered features from these existing columns through transformers such as TF-IDF and StandardizedScaler. These features are intrinsic to the recipes themselves and can be accessed regardless of whether the recipe has received user ratings or reviews.

To challenge ourselves further, we additionally decided to run a classification task with out data using the RandomForestClassifier.

## Baseline Model

Our baseline model aims to predict the average rating of a recipe using **basic linear regression**. The features used in this model are **number of calories,** which is a quantitative variable, and the **tags associated with each recipe,** a nominal variable encoded using TF-IDF. We passed the number of calories directly into the model without any transformation, while the tags were processed using a custom tokenizer to preserve multi-word tags (e.g., "quick-recipe") before applying TF-IDF encoding. The default toknizer for TF-IDF splits up more complex tags, which defeats the purpose of modeling by tag in the first place. The TF-IDF approach converted the nominal strings in `"tags"` into a numerical representation, ensuring meaningful textual patterns were captured as predictive signals.

To preprocess the data, we used a `ColumnTransformer` to handle the data types: the calorie feature was left untouched (passthrough), and the tags underwent tokenization and vectorization by TF-IDF. The dataset was split into a standard 80/20 train-test split, ensuring the model was evaluated on unseen data as well.

We judged our performance of the baseline model using mean squared error (MSE) and R-squared (R^2) metrics. The model achieved an **MSE of 0.238**, indicating the average squared deviation of predictions from the actual ratings. However, the **R^2 score was 0.037**, suggesting that only 3.7% of the variance in average ratings was explained by the model 😔. The low R^2 score indicates that the current model struggles to capture the relationship between the features and the target variable. While the number of calories and TF-IDF-encoded tags may have some predictive power (R^2 > 0), they are likely not enough to explain the trends of recipe ratings fully. Since our R^2 at least wasn't 0, we think this model can be considered a reasonable starting point but is not yet good by any standard. 

## Final Model

#### Improving on Linear Regression
To improve for the final model, we expanded the features to include additional numerical and text-based features that we believe are closely related to recipe ratings based on the data-generating process. The added numerical features include the nutritional stats such as total fat, sugar, sodium, protein, saturated fat, carbohydrates PDVs and recipe-specific features like the number of tags (n_tags) and review length (len_review). These recipe-specific features were chosen because they capture important aspects of a recipe’s quality and complexity, which can influence user ratings. Nutritional statistics might incluence users who are health-conscious or boost unhelathier indulgent recipes. The number of tags can reveal more about how food.com may promote recipes with specific tags to tailor the preferences of certian people, while review length can serve as a measurement for user engagement.

We also kept the tags column and incorporated the review column to account for the sentiment and key-words of user feedback. By combining these features, we aimed to better utilize the available data for a better prediction model.

We created the model pipeline with a ColumnTransformer to preprocess numerical features directly and apply TF-IDF to the text-based features. The final model achieved a **Mean Squared Error (MSE) of 0.216** and an **R^2 score of 0.128** on the test set. Compared to the baseline model (MSE = 0.238, R^2 = 0.037), this represents a significant improvement, with the R^2 score increasing by approximately 250%. The better performance suggests that the expanded feature set captures more of the variance in recipe ratings, providing additional predictive power. It is important to recognize though that isn't a great value still, but we can attribute this to the fact that this is a task involving subjective human preferences, and there may be potential for further improvement with more complex regression models or possibly a different combination of features.

Since we used linear regression, no hyperparameter tuning was needed since focused on feature engineering and multi-dimensional linear regression. However, we decided to also use Classification as a bonus exploration of our data, which did involve hyperparamter tuning.

#### Looking at Classification: Random Forest Classifier
Due to our linear regression model's rather poor overall performance, our group explored using multi-class classification to predict the average rating of a recipe. Since average ratings are ordinal, we explicitly converted them to categorical labels by grouping them into discrete bins of 0.5 by rounding the continuous `average_rating` values to the nearest half point (ex: 4.5). This created a new feature, `avg_rating_group`, that became the target variable. 

For the feature set, we used `num_calories`, a numerical feature, and `tags_split`, a text feature represented by TF-IDF. These features were processed with a ColumnTransformer to handle the two types of input data appropriately.

Similar to the prior approach with a baseline and a final model, we initially created a Random Forest Classifier with arbitrary hyperparameters. These 'arbitrary' hyperparamaters were actually the final hyperparameters extracted from another rendition of this project, which aimed to find the relationship between [levels of sugar and average ratings](https://cecilia-lin.github.io/RecipeProject/). Since our features were different, we figured that these wouldn't be our optimal hyperparameters, but thought it would be an interesting place to start. 

This 'baseline' model achieved an **accuracy of 82%** with decent performance across different classes, though the recall and precision for lower-rated categories were notably weak, likely due to fewer avilable data points. The F1 scores for most categories, particularly the higher ratings (4.5 and 5.0), were high, indicating that the model could successfully predict high-rated recipes but struggled with the lower ratings. We also calculated the macro-average (finding unweighted mean of all classes treating them classes) **precision of 95%**, **recall of 33%**, and **f1 of 42%**.

#### Hyperparameter Tuning with GridSearchCV
To improve the classification model, we performed hyperparameter tuning using GridSearchCV. We tested different values for two hyperparameters: `n_estimators`, which is the number of decision trees, and `max_depth`, depth of the trees. The best model hyperparameters turned out to be `n_estimators=200` and interestingly an unrestricted tree depth, `max_depth=None`. This means the trees were allowed to grow deeper and capture more complex patterns in the data.

After this hyperparameter tuning, we recalculated our metric and saw an improvement in **accuracy to 92%** compared to the initial model, while also improving performance in recall and precision for the lower-rated categories. The new macro-average  **precision is 88%**, **recall of 64%**, and **f1 of 70%**. While precision slightly drops, the important thing to notice is that recall significantly jumps along with the f1-score, which is the primary measurement of focus in this case.

Overall, we realized that classification is a valid approach for predicting recipe ratings (better than regression in our case), especially when ratings are treated as categorical labels. The Random Forest Classifier with hyperparameter tuning from GridSearchCV shows very good performance with only a few features, particularly for higher-rated recipes. However, we still couldn't very accurately predict the lower-rated categories due to data imbalance.


## Fairness Model

