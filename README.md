Authors: Anna Doan and Jiya Makhija

##  Introduction
In this project, we analyze a dataset of recipes and their ratings to explore what factors contribute to higher ratings. Specifically, we aim to predict recipe ratings based on features such as the number of ingredients, preparation time, number of steps, and nutritional content. Hence, our project centers around the question: What factors contribute to a highly-rated recipe? 

However, to make the findings of this project more significant, we made the question more specific. Thus, a key aspect of our analysis involves testing whether the number of ingredients in a recipe influences its rating. Specifically, **we examine whether recipes with 9 or fewer ingredients receive different average ratings than those with more than 9 ingredients**. This matters because we think that recipe ratings influence what people cook and how they choose recipes online. Understanding which factors correlate with higher ratings can help recipe creators optimize their content and improve user experience on food platforms.

To investigate this question, we analyze two datasets. The first dataset, `RAW_recipes.csv`, contains 83,782 unique recipes, each with attributes such as preparation time, ingredients, and nutritional information. The second dataset, `interactions.csv`, includes 731,927 user interactions in the form of ratings and reviews.

Several key variables from these datasets are neccessary for our analysis. From `RAW_recipes.csv`, the `ingredients` column lists the components used in each recipe, while `n_ingredients` quantifies the total number of ingredients. Additionally, the `minutes` column specifies the time required to prepare a dish, providing insight into the role of convenience in recipe popularity. From `interactions.csv`, the `rating` column captures user evaluations of a recipe, serving as our primary measure of recipe success.


*To do:* 
~~- clearly state the one question your project is centered around~~
- Why should readers of your website care about the dataset and your question specifically?
~~- Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns.~~








## Data Cleaning and Exploratory Analysis
• Cleaned data (8 points)
• Performed univariate analyses (8 points)
• Performed bivariate analyses and aggregations (8 points)
### Data Cleaning
*Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).* <br>

The dataset underwent a multi-step cleaning and transformation process to enhance its quality for analysis. The original dataset contained raw recipe information along with user interactions, which required refinement to ensure consistency, remove errors, and enable meaningful analysis. One of the most critical data cleaning steps involved processing the `nutrition` column, which originally stored values as a single string representing a list. We extracted individual nutritional components, such as calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates, and converted them into numeric format. This step ensured that the nutritional data could be analyzed quantitatively rather than as raw text. By structuring the nutrition data into distinct numerical fields, we enabled meaningful statistical computations, including correlations with recipe ratings and complexity.

To analyze recipes alongside user engagement, we merged the `recipes` and `interactions` datasets using a left join. This ensured that all recipes were retained, even those without user interactions. The merge provided a comprehensive view of recipe popularity and quality by combining metadata with user feedback, allowing for deeper analysis of how recipe characteristics influence user ratings. The `submitted` column (date of recipe submission) and `date` column (date of user interaction) were converted from string format to a datetime format. This transformation allowed for time-based analysis, such as tracking trends in user engagement over time or assessing how older recipes perform compared to newer ones.

Some users assigned a rating of zero, which likely represented missing data rather than a valid rating. These values were replaced with NaN to avoid skewing the analysis. This improved the accuracy of the rating distribution and ensured that statistical measures, such as mean and standard deviation, reflected actual user sentiment. For each recipe, we computed an average rating by aggregating user ratings. The average rating per recipe provided a more reliable measure of quality compared to individual ratings, which may be subjective and inconsistent.

Recipes were categorized into "Simple" (≤9 ingredients) and "Complex" (>9 ingredients) to explore whether ingredient count influenced user ratings. This classification allowed us to compare user preferences based on complexity and analyze whether simpler or more complex recipes received higher ratings. After cleaning the dataset, we performed deeper numerical analysis. The dataset contained recipes with a wide range of ratings, with a central tendency around 4.5, indicating that most recipes received favorable reviews. Simpler recipes generally had slightly higher ratings than complex ones, though the difference was not substantial.

Further correlation analysis revealed that ratings were weakly correlated with nutritional factors, meaning that users did not consistently rate healthier or unhealthier recipes higher. However, calories, fat, sugar, and protein were highly correlated with each other, suggesting that recipes with high fat content also tended to be high in calories and sugar. Preparation time showed little correlation with rating, indicating that users do not necessarily prefer shorter or longer recipes in terms of satisfaction.

Through structured data cleaning and analysis, we transformed the raw dataset into a well-structured format suitable for statistical exploration. The refined dataset allows for deeper insights into how recipe complexity, nutrition, and preparation time influence user ratings. While ingredient complexity plays a role in user preferences, nutritional content appears to have minimal impact on ratings. 

Here's what the Markdown table looks like for the first 5 rows of our cleaned dataset. *Note*: the code for this table was generated automatically from a DataFrame, using <br>

``` py 
print(merged_df.head().to_markdown(index=False))
```

```markdown:GFM
| name                                 |     id |   minutes |   contributor_id | submitted           | tags                                                                                                                                                                                                                        |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                    |   n_ingredients |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |          user_id |   recipe_id | date                |   rating | review                                                                                                                                                                                                                                                                                                                                           |   average_rating | ingredient_complexity   |
|:-------------------------------------|-------:|----------:|-----------------:|:--------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|-----------------:|------------:|:--------------------|---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------:|:------------------------|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 | 386585           |      333281 | 2008-11-19 00:00:00 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |                4 | Simple                  |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 | 424680           |      453467 | 2012-01-26 00:00:00 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |                5 | Complex                 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |  29782           |      306168 | 2008-12-31 00:00:00 |        5 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |                5 | Simple                  |
|                                      |        |           |                  |                     |                                                                                                                                                                                                                             |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |            |             |         |          |           |                 |                 |                  |             |                     |          | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |                  |                         |
|                                      |        |           |                  |                     |                                                                                                                                                                                                                             |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |            |             |         |          |           |                 |                 |                  |             |                     |          | Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)                                                                                                                                                                                                                          |                  |                         |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |      1.19628e+06 |      306168 | 2009-04-13 00:00:00 |        5 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |                5 | Simple                  |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | 768828           |      306168 | 2013-08-02 00:00:00 |        5 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                     |                5 | Simple                  |
```

### Univariate Analysis
- Embed at least one `plotly` plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions)
- Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present.
- Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.

### Bivariate Analysis
- Embed at least one `plotly` plot that displays the relationship between two columns.
- Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present.

### Interesting Aggregates
- Embed at least one grouped table or pivot table in your website and explain its significance







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









## Hypothesis Testing
• Selected relevant columns for a hypothesis or permutation test (4 points)
~~• Explicitly stated a null hypothesis (4 points)~~
~~• Explicitly stated an alternative hypothesis (4 points)~~
• Performed a hypothesis or permutation test (8 points)
• Used a valid test statistic (4 points)
• Computed a p-value and made a decision (4 points)

- Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion.
- Justify why these choices are good choices for answering the question you are trying to answer.

Optional: Embed a visualization related to your hypothesis test in your website.

Tip: When making writing your conclusions to the statistical tests in this project, never use language that implies an absolute conclusion; since we are performing statistical tests and not randomized controlled trials, we cannot prove that either hypothesis is 100% true or false.

Our **null hypothesis** is "The average rating of recipes with 9 or fewer ingredients is the same as recipes with more than 9 ingredients. Any observed difference is due to random chance."

Our **alternative hypothesis** is "The average rating of recipes with 9 or fewer ingredients is different from recipes with more than 9 ingredients."

Our relevant features were the `insert column names here` from the `RAW_recipes.csv` file. 
We thought a permuation test was best due to . . .

We performed a permutation test by...

We thought a valid test statistic for the permuation test was by difference of means. 

Thus, from our permutation test, we got an observed difference in mean ratings as 0.003649, and calculated a p-value of 0.216. Since p = 0.216, which is greater than 0.05, we fail to reject the null hypothesis. This suggests that there is not strong statistical evidence that the number of ingredients affects the ratings.








## Framing a Prediction Problem
• 15 pts

Clearly state your prediction problem and type (classification or regression). If you are building a classifier, make sure to state whether you are performing binary classification or multiclass classification. Report the response variable (i.e. the variable you are predicting) and why you chose it, the metric you are using to evaluate your model and why you chose it over other suitable metrics (e.g. accuracy vs. F1-score).

Note: Make sure to justify what information you would know at the “time of prediction” and to only train your model using those features. For instance, if we wanted to predict your final exam grade, we couldn’t use your Final Project grade, because the project is only due after the final exam! Feel free to ask questions if you’re not sure.








## Baseline Model 
• 35 pts
Describe your model and state the features in your model, including how many are quantitative, ordinal, and nominal, and how you performed any necessary encodings. Report the performance of your model and whether or not you believe your current model is “good” and why.

Tip: Make sure to hit all of the points above: many projects in the past have lost points for not doing so.










## Final Model
• 35 pts
State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.









## Fairness Analysis
• 15 pts
Clearly state your choice of Group X and Group Y, your evaluation metric, your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion.

Optional: Embed a visualization related to your permutation test in your website.






