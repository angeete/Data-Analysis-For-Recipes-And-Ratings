# Data Analysis For Recipes And Ratings
A data analysis of a recipes and ratings data set as a final project for EECS 398 at the University of Michigan

## Introduction
There are many recipes to choose from all around the world, to the point that choosing one can be overwhelming. Ideally, many people would choose a recipe that has a high rating, but what leads to a recipe being highly rated? This leads us to our driving question: What factors lead to a recipe having a higher average rating?
To solve this question, we used exploratory data analysis to evaluate different factors from our dataset and then used predictive models to see how easily these factors could predict average ratings for recipes.  
We were given two datasets for this project, RAW_recipes.csv and RAW_interactions.csv. RAW_recipes.csv contains information about recipes from food.com, while RAW_interactions.csv contains information about the corresponding reviews and ratings for these recipes. The interactions dataset has 731,927 rows and the recipes dataset has 83,781 rows. //describe the featueres that are relevant//: We created a dataframe for each dataset, named 'interactions' and 'recipes'. From the 'interactions' dataframe, the minutes, hours, protein, calories, n_steps, n_ingredients, average rating, positive_score, and negative_score columns are all columns in the 'interactions' dataframe that were useful in our project. The hours, protein, calories, average rating, positive_score, and negative_score columns are columns we added to the 'interactions' dataframe, and extracted from the minutes, nutrition, ratings, and review columns. The minutes column is the number of minutes it takes to make the recipe, the hours column is the number of hours it takes to make the recipe, the protein column is the percent daily value of protein in a recipe (extracted from the nutrition column).

## Data Cleaning and Exploratory Data Analysis
Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).
### Data Cleaning
The first step for data cleaning involved reformatting the 'interactions' dataframe such that each recipe corresponds to only one row in the data frame. This process required for us to combine all of the reviews, ratings, user_ids for the reviews, and the dates for the reviews into lists such that they fit within a row. This was done so that all the ratings and reviews for one recipe could be accessed just by looking up the recipe. 

The second step for data cleaning involved imputation and adding columns. All ratings of zero were inputed with np.nan, as a rating of zero in this dataset represents recipes with no ratings. We created an average rating column using the list of our ratings and ignored np.nan values in our mean calculations so that each recipe gets the correct average rating value. Note that this average rating column will be extensively used in the analysis as it is a good indicator of what people think of a recipe and what is a singular value for each recipe.   

The third step for data cleaning involved merging the 'recipes' and 'interactions' dataframes into one dataframe, called the 'combined' dataframe. We do this in order to easily perform operations and compare different features from both datasets. We also created an 'hours' column in our 'combined' dataframe by dividing the 'minute' column by 60. Additionally, the 'calories' and 'protein' columns were created in the 'combined' dataframe by extracting these values from the 0th and 4th posititions respectively from the lists stored in the nutrition column.   

The fourth step for data cleaning was to ensure that all columns in the combined dataframe had no np.nan values. This is because having np.nan values in our dataframe would cause problems in the executing our models later on. In order to fix this issue, we imputed all np.nan values in the average rating, protein, calories, n_steps, and n_ingredients column with 0. We chose to replace the np.nan values with 0 because having np.nan values represents an absence of a feature for a specific recipe (for example, having np.nan in the protein column for a specific recipe means that there is no protein in that recipe).

### Outliers
The fifth step for data cleaning was to investigate outlier. Particularly, there were three very extreme outliers we saw in the dataframe. To do this, we found the index of these reviews in the dataframe and looked at the name of these recipes. One of the reviews was titled "How to preserve your husband", and it had an egregious cooking time of 17520 hours. This review does not make sense at all, so we removed this review from the dataframe.  

The other two outlier review also had quite high cooking times. The names of these were recipes are "how to make homemade vanilla" and "how to make homemade fruit liquers". When looking up the standard time needed to makes these recipes, they ranged from a few weeks to a few months to even years, so the high cooking times for these two recipes made sense. Therefore we decided to keep these two recipes in the dataframe. 

### Univariate Plot Analyses
#### Univariate Analysis Plot 1: Average Ratings Distribution 
We used a histogram to showcase the average ratings distribution and from this we can see, that most of the recipies have an average rating of 5 stars. Note that we use average ratings here since ratings are given as a list as there are multiple ratings per recipe.  

<iframe
src="assets/avg_rating_univariate.html"
width="800"
height="600"
frameborder="0"
></iframe>  
#### Univariate Analysis Plot 2: Cooking Time in Hours  
We restricted the range of the hours column when plotting this box plot so that we can clearly see the boxplot. When originally plotting the boxplot without restricting the hours column, the median and quartiles were not visible due to the extremely large scaling on the x axis for hours due to the outliers present.  

<iframe
src="assets/cooking_hours_univariate.html"
width="800"
height="600"
frameborder="0"
></iframe>   
 #### Univariate Analysis Plot 3: Calories  
We restricted the range of the calories column when plotting this box plot so that we can clearly see the boxplot. When originally plotting the boxplot without restricting the calories column, the median and quartiles were not visible due to the extremely large scaling on the x axis for calories due to the outliers present.  

<iframe
src="assets/calories_univariate.html"
width="800"
height="600"
frameborder="0"
></iframe>  
#### Univariate Analysis Plot 4: Protein 
We restricted the range of the protein column when plotting this box plot so that we can clearly see the boxplot. When originally plotting the boxplot without restricting the protein column, the median and quartiles were not visible due to the extremely large scaling on the x axis for protein due to the outliers present.  

<iframe
src="assets/protein_univariate.html"
width="800"
height="600"
frameborder="0"
></iframe>  
### Bivariate Plot Analyses   
#### Bivariate Plot 1: Number of Steps vs Average Rating  
For this first bivariate plot, we took a look at the relation between the number of steps in a recipe and the average rating. From the plot, we can see that higher step ratings tend to be on the extreme ends of the rating distribution, either a 5 or a 2 whereas recipes with an average or low number of steps tend to have a more average-to-high rating (between 3 to 5 usually).  

//embed plot//  
#### Bivariate Plot 2: Number of Ingredients vs Average Rating  
For the second bivariate plot, we took a look at the relation between the number of ingredients in a recipe and the average rating. From the plot, we can see that the number of ingredients doesn't seem to play a large role in determining the average rating of a recipe as the ratings are fairly equally scattered across all numbers of ingredients.  

//embed plot//

### Interesting Aggregates  
We grouped our dataframe by the number of steps in a recipe and then got the statistics for each column (excluding n_steps as this is what we grouped by) in the combined dataframe using the .describe() function.

Using this groupby object, we extracted the cooking time (in minutes) for each recipe and calculated the mean cooking time (in minutes) to complete each recipe. As expected, the general trend was that an increase in the number of steps led to an increase in mean cooking time. It is interesting to note that recipes with 93 steps seem to deviate significantly from the trend, as the mean cooking time drops to 360 minutes from a previous mean time of 1530 minutes for 88 step recipes.  

We also used this groupby object to extract the number of ingredients for each recipe and calculated the mean number of steps for needed to complete these recipes. As expected, the general trend was that an increase in steps lead to an increase in mean number of ingredients for a recipe.



## Framing a Prediction Problem

## Baseline Model

## Final Model
