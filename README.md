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
Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).

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






