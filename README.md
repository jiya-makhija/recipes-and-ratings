Authors: Anna Doan and Jiya Makhija

##  Introduction
In this project, we analyze a dataset of recipes and their ratings to explore what factors contribute to higher ratings. Specifically, we aim to predict recipe ratings based on features such as the number of ingredients, preparation time, number of steps, and nutritional content. Hence, our project centers around the question: What factors contribute to a highly-rated recipe? 

However, to make the findings of this project more significant, we made the question more specific. Thus, a key aspect of our analysis involves testing whether the number of ingredients in a recipe influences its rating. Specifically, **we examine whether recipes with 9 or fewer ingredients receive different average ratings than those with more than 9 ingredients**. This matters because we think that recipe ratings influence what people cook and how they choose recipes online. Understanding which factors correlate with higher ratings can help recipe creators optimize their content and improve user experience on food platforms.

To investigate this question, we analyze two datasets. The first dataset, `RAW_recipes.csv`, contains 83,782 unique recipes, each with attributes such as preparation time, ingredients, and nutritional information. The second dataset, `interactions.csv`, includes 731,927 user interactions in the form of ratings and reviews.

Several key variables from these datasets are neccessary for our analysis. From `RAW_recipes.csv`, the `ingredients` column lists the components used in each recipe, while `n_ingredients` quantifies the total number of ingredients. Additionally, the `minutes` column specifies the time required to prepare a dish, providing insight into the role of convenience in recipe popularity. From `interactions.csv`, the `rating` column captures user evaluations of a recipe, serving as our primary measure of recipe success.



---




## Data Cleaning and Exploratory Analysis

### Data Cleaning

The dataset underwent a multi-step cleaning and transformation process to enhance its quality for analysis. The original dataset contained raw recipe information along with user interactions, which required refinement to ensure consistency, remove errors, and enable meaningful analysis. One of the most critical data cleaning steps involved processing the `nutrition` column, which originally stored values as a single string representing a list. We extracted individual nutritional components, such as calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates, and converted them into numeric format. This step ensured that the nutritional data could be analyzed quantitatively rather than as raw text. By structuring the nutrition data into distinct numerical fields, we enabled meaningful statistical computations, including correlations with recipe ratings and complexity.

To analyze recipes alongside user engagement, we merged the `recipes` and `interactions` datasets using a left join. This ensured that all recipes were retained, even those without user interactions. The merge provided a comprehensive view of recipe popularity and quality by combining metadata with user feedback, allowing for deeper analysis of how recipe characteristics influence user ratings. The `submitted` column (date of recipe submission) and `date` column (date of user interaction) were converted from string format to a datetime format. This transformation allowed for time-based analysis, such as tracking trends in user engagement over time or assessing how older recipes perform compared to newer ones.

Some users assigned a rating of zero, which likely represented missing data rather than a valid rating. These values were replaced with NaN to avoid skewing the analysis. This improved the accuracy of the rating distribution and ensured that statistical measures, such as mean and standard deviation, reflected actual user sentiment. For each recipe, we computed an average rating by aggregating user ratings. The average rating per recipe provided a more reliable measure of quality compared to individual ratings, which may be subjective and inconsistent.

Recipes were categorized into "Simple" (≤9 ingredients) and "Complex" (>9 ingredients) to explore whether ingredient count influenced user ratings. This classification allowed us to compare user preferences based on complexity and analyze whether simpler or more complex recipes received higher ratings. After cleaning the dataset, we performed deeper numerical analysis. The dataset contained recipes with a wide range of ratings, with a central tendency around 4.5, indicating that most recipes received favorable reviews. Simpler recipes generally had slightly higher ratings than complex ones, though the difference was not substantial.

Further correlation analysis revealed that ratings were weakly correlated with nutritional factors, meaning that users did not consistently rate healthier or unhealthier recipes higher. However, calories, fat, sugar, and protein were highly correlated with each other, suggesting that recipes with high fat content also tended to be high in calories and sugar. Preparation time showed little correlation with rating, indicating that users do not necessarily prefer shorter or longer recipes in terms of satisfaction.

Through structured data cleaning and analysis, we transformed the raw dataset into a well-structured format suitable for statistical exploration. The refined dataset allows for deeper insights into how recipe complexity, nutrition, and preparation time influence user ratings. While ingredient complexity plays a role in user preferences, nutritional content appears to have minimal impact on ratings. 

Here's what the Markdown table looks like for the first 5 rows of our cleaned dataset. *Note*: the code for this table was generated automatically from a DataFrame, using

``` py 
print(merged_df.head().to_markdown(index=False))
```

| name                                 |     id |   minutes |   contributor_id | submitted           | tags                                                                                                                                                                                                                        |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                    |   n_ingredients |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |          user_id |   recipe_id | date                |   rating | review                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |   average_rating | ingredient_complexity   |   is_missing_rating |   time_per_step |   ingredients_per_step |
|:-------------------------------------|-------:|----------:|-----------------:|:--------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|-----------------:|------------:|:--------------------|---------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------:|:------------------------|--------------------:|----------------:|-----------------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 | 386585           |      333281 | 2008-11-19 00:00:00 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                                                                                                                                                        |                4 | Simple                  |                   0 |         3.63636 |               0.818182 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 | 424680           |      453467 | 2012-01-26 00:00:00 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe!                                                                                                                                      |                5 | Complex                 |                   0 |         3.46154 |               0.846154 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |  29782           |      306168 | 2008-12-31 00:00:00 |        5 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!   The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.   Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :) |                5 | Simple                  |                   0 |         5.71429 |               1.28571  |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |      1.19628e+06 |      306168 | 2009-04-13 00:00:00 |        5 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                                                                                                                                                                    |                5 | Simple                  |                   0 |         5.71429 |               1.28571  |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | 768828           |      306168 | 2013-08-02 00:00:00 |        5 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                                                                                                                                                          |                5 | Simple                  |                   0 |         5.71429 |               1.28571  |


### Univariate Analysis

<iframe
  src="assets/distribution_cooking_time.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The majority of recipes have a cooking time of less than 100 minutes, with a sharp decline in frequency for longer times. However, a few extreme outliers with cooking times exceeding 100,000 minutes distort the scale. These long durations likely represent data entry errors or slow-cooking methods (e.g., fermentation).

<iframe
  src="assets/distribution_n_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Most recipes have between 5 and 15 steps, suggesting that online recipes generally aim to be easy to follow. The number of steps follows a right-skewed distribution, meaning there are a few recipes with significantly more steps, likely representing more complex or gourmet dishes.

<iframe
  src="assets/distribution_n_ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The number of ingredients in most recipes falls between 5 and 15, with very few recipes using more than 20 ingredients. This suggests that most recipes balance simplicity and flavor by using a moderate number of ingredients, while only a handful of recipes require an extensive list of ingredients.


<iframe
  src="assets/distribution_ratings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The majority of recipes have received high ratings (4 or 5 stars), indicating that users tend to rate recipes favorably. There are fewer low ratings (0-2), which may reflect a bias where users only rate recipes they enjoyed, rather than leaving negative feedback.



### Bivariate Analysis

<iframe
  src="assets/cooking_time_vs_ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

There is a weak positive correlation between cooking time and the number of ingredients. Recipes with more ingredients tend to require longer cooking times, but the relationship is not strong. This suggests that while complex recipes may take longer to prepare, many simple recipes also require extended cooking times, possibly due to slow-cooking or baking techniques.


<iframe
  src="assets/steps_vs_rating.html"
  width="500"
  height="400"
  frameborder="0"
></iframe>

The box plot shows that recipe ratings are fairly consistent across different numbers of steps. While the median rating remains high (around 4.5) for all step counts, there is a slight increase in variability for recipes with very few or very many steps. This suggests that users do not necessarily prefer simpler or more complex recipes based on the number of steps alone.


### Interesting Aggregates

<iframe
  src="assets/ingredient_complexity_pivot.html"
  width="500"
  height="200"
  frameborder="0"
></iframe>

This table summarizes the number of recipes and their average ratings based on ingredient complexity. We observe that recipes classified as "Simple" (≤9 ingredients) tend to receive slightly higher average ratings compared to "Complex" recipes (>9 ingredients). This suggests that users might prefer recipes that require fewer ingredients, possibly because they are easier to prepare.

<iframe
  src="assets/time_rating_pivot.html"
  width="500"
  height="200"
  frameborder="0"
></iframe>

This table summarizes the average cooking time for recipes grouped by their user ratings. We observe that recipes with a 5-star rating tend to have a slightly higher average cooking time (~107 minutes) compared to lower-rated recipes. Interestingly, 3-star recipes have the shortest average cooking time (~87 minutes), suggesting that recipes requiring less effort might not always lead to higher satisfaction. This could indicate that users value recipes that take more time to prepare, potentially because they result in better flavor or texture. However, the difference across ratings is not very large, meaning other factors—such as ingredients or cooking techniques—might play a more significant role in recipe ratings.

---


## Assessment of Missingness
• Addressed NMAR question (4 points)
• Performed permutation tests for missingness (8 points)
• Interpreted missingness test results (8 points)
### NMAR Analysis 
- State whether you believe there is a column in your dataset that is NMAR.
- Explain your reasoning and any additional data you might want to obtain that could explain the missingness (thereby making it MAR).
- Make sure to explicitly use the term “NMAR.”

### Missingness Dependency 
- Present and interpret the results of your missingness permutation tests with respect to your data and question.
- Embed a `plotly` plot related to your missingness exploration; ideas include:
    - The distribution of column Y when column X is missing and the distribution of column Y when column X is not missing, as was done in Lecture 8.
    - The empirical distribution of the test statistic used in one of your permutation tests, along with the observed statistic.







---

## Hypothesis Testing

This study examines whether the number of ingredients in a recipe has a significant impact on user ratings. To investigate this, we conduct a permutation test comparing the average ratings of recipes categorized as "Simple" (those with 9 or fewer ingredients) and "Complex" (those with more than 9 ingredients). The goal is to determine whether any observed difference in ratings is due to an actual effect of ingredient count or merely a result of random variation. <br>


To formally assess this question, we define our hypotheses as follows:

- **Null Hypothesis**: The distribution of recipe ratings is the same for recipes with 9 or fewer ingredients and recipes with more than 9 ingredients. Any observed difference in mean ratings is due to random chance.
- **Alternative Hypothesis**: The distribution of recipe ratings differs between recipes with 9 or fewer ingredients and those with more than 9 ingredients, suggesting that ingredient count influences user ratings.

<br>
The dataset contains several variables relevant to this analysis. Specifically, we focus on:

- **`rating`**: The numerical rating assigned by users to a recipe, which serves as the dependent variable.
- **`ingredient_complexity`**: A categorical variable that classifies recipes into two groups:  
  - **"Simple"**: Recipes with 9 or fewer ingredients.  
  - **"Complex"**: Recipes with more than 9 ingredients.  

The two groups `Simple` and `Complex` are naturally independent, allowing for a valid between-group comparison. The test statistic, defined as the mean difference in ratings between these two groups enables a clear assessment of whether the observed difference is statistically significant. These variables allow us to compare the mean ratings between the two groups.

A permutation test was selected because it does not rely on parametric assumptions regarding the distribution of ratings, unlike traditional tests such as the t-test, which require normality and equal variance. Given that these assumptions may not hold in this dataset, the permutation approach provides a more flexible and reliable method for comparing group means by constructing a null distribution directly from the observed data. Additionally, since the research question focuses on whether recipes with fewer ingredients receive different ratings than those with more ingredients, a comparison of independent groups is the most appropriate approach.

The test is conducted as follows:

1. Compute the observed difference in mean ratings between the two groups.
2. Randomly permute the `rating` column while keeping the `ingredient_complexity` group labels fixed.
3. Recalculate the mean difference between the two groups after each shuffle.
4. Repeat this process 1,000 times to generate a null distribution of mean differences under the assumption that ingredient count does not affect ratings.
5. Compute the p-value as the proportion of shuffled differences that are equal to or more extreme than the observed difference.

The use of 1,000 permutations ensures that the null distribution is well-approximated, leading to a more accurate p-value estimation and reducing the influence of random variation.


Following the execution of the permutation test, we obtain an **observed difference in mean ratings** of 0.003649 and a **p-value** of 0.216. The interpretation of the p-value is as follows:

- If **p &le; 0.05**, we reject the null hypothesis, indicating that the number of ingredients in a recipe has a statistically significant effect on user ratings.
- If **p &ge; 0.05**, we fail to reject the null hypothesis, meaning there is insufficient statistical evidence to conclude that ingredient count impacts ratings. However, failing to reject the null hypothesis does not prove that ingredient count has no effect; rather, it suggests that any potential difference was not statistically significant given the available data.


Since the p-value is 0.216, which is greater than 0.05, we fail to reject the null hypothesis. This suggests that there is not strong statistical evidence that the number of ingredients affects the ratings.


By employing a non-parametric method that does not impose restrictive assumptions, this analysis ensures that the findings remain relevant and generalizable to datasets with diverse distributions.


---


## Framing a Prediction Problem


In this project, we define a supervised learning prediction problem aimed at understanding the factors that influence recipe ratings. The problem is framed as a **regression type task**, where we seek to predict the continuous numerical rating of a recipe based on various features extracted from the dataset.

The response variable in our regression model is the recipe rating, which represents the average user rating for a given recipe. We select this variable because it provides a direct measure of recipe quality and user satisfaction. Predicting recipe ratings allows us to offer better recommendations and insights into what characteristics contribute to highly-rated recipes.

To evaluate the performance of our regression model, we choose the Root Mean Squared Error (RMSE) as the primary metric. RMSE is appropriate because it penalizes larger errors more heavily, making it useful in scenarios where extreme mispredictions should be minimized. While other metrics, such as Mean Absolute Error (MAE) or R-squared, could also be considered, RMSE provides an intuitive measure of how far predicted ratings deviate from actual ratings in the dataset.

A crucial aspect of our model development is ensuring that the features used for training are available at the time of prediction to prevent data leakage. For instance, we include recipe attributes such as ingredient composition, preparation time, and user-generated metadata, while excluding post-publication data like cumulative user ratings or review counts, which would not be known when a new recipe is first introduced.

By framing the problem as a regression task, we aim to develop a predictive model that provides valuable insights into the determinants of recipe success. The findings from this study can help enhance recipe recommendation systems and assist culinary content creators in optimizing their recipes for better engagement and ratings.


---



## Baseline Model 

Our predictive model is a Linear Regression model designed to estimate recipe ratings based on key features extracted from the dataset. The model was implemented using Scikit-learn’s pipeline, incorporating feature scaling to standardize numerical inputs before training.

The model utilizes three quantitative features:

- `minutes`: The total preparation and cooking time for a recipe.
- `n_steps`: The number of steps involved in preparing the recipe.
- `n_ingredients`: The number of unique ingredients required.
  
These features were selected based on their potential influence on user ratings. For instance, recipes with fewer steps or ingredients might be more appealing to users looking for simplicity, while longer preparation times could be indicative of more elaborate dishes that may receive higher ratings.

In regards to data types, there are three quantitative features mentioned above. Additionally, the dataset does not contain any ordinal variables that require ranking-based encoding, and no nominal variables are present in the current feature set. 


The model was trained and evaluated using an 80-20 train-test split. The Root Mean Squared Error (RMSE) was chosen as the primary evaluation metric, as it effectively quantifies the deviation between predicted and actual ratings.


We obtained an RMSE of 0.7139, which indicates that, on average, the model's predicted ratings deviate by approximately 0.71 points from the actual ratings. This suggests that while the model provides a reasonable baseline, there is room for improvement. 

The use of Linear Regression as a baseline model assumes a strictly linear relationship between the selected features and recipe ratings, which may not fully represent the complexity of user preferences. Additionally, the model does not yet incorporate potential categorical variables or nonlinear interactions that could improve predictive performance. While the current model provides a solid foundation, its performance could be enhanced, making it more robust and effective in predicting recipe ratings.



---





## Final Model
State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.






---


## Fairness Analysis
• 15 pts
Clearly state your choice of Group X and Group Y, your evaluation metric, your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion.

Optional: Embed a visualization related to your permutation test in your website.






