# Data-Analysis-For-Recipes-And-Ratings
A data analysis of a recipes and ratings data set as a final project for EECS 398 at the University of Michigan

## Introduction
//need to include hook//  
We were given two datasets for this project, RAW_recipes.csv and RAW_interactions.csv. RAW_recipes.cvs contains information relating to recipes from food.com, while RAW_interactions.csv contains information about the corresponding reviews and ratings for these recipes. The interactions dataset has 731,927 rows and the recipes dataset has ___ rows. //describe the featueres that are relevant// 
We want to investigate what factors would lead to higher average ratings for recipes. There are 83,781 rows in the 

## Data Cleaning and Exploratory Data Analysis
Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).
### Data Cleaning
The first part of our data cleaning process is to combine both the recipes and interactions datatsets into one dataframe, called the interactions dataframe. This ensures that we can easily perform operations and compare different features from both datasets.

The first step for our data cleaning involved reading in the two csv files, interactions and recipes, as dataframes such that we can modify them.

The second step for data cleaning involved reformatting the interactions dataframe such that each recipe corresponds to only one row in the data frame. This process required for us to combine all of the reviews, ratings, user_ids for the reviews, and the dates for the reviews into lists such that they fit within a row. This was done so that all the ratings and reviews for one recipe could be accessed just by looking up the recipe. All ratings of zero were inputed with np.nan, as a rating of zero in this dataset represents recipes with no ratings. We created an average rating column using the list of our ratings and ignored np.nan values in our mean calculations so that each recipe gets the correct average rating value. Note this average rating column will be extensively used in the analysis as it is a good indicator of what people think of a recipe and what is a singular value for each recipe.   

The third step for data cleaning involved creating an 'hours' column in our combined dataframe by dividing the 'minute' column by 60. Additionally, 'calories' and 'protein' columns were created in the combined dataframe by extracting these values from the 0th and 4th posititions from the lists stored in the nutrition column.   

We also ensured that all columns in the combined dataframe had no np.nan values because it would cause problems in the executing our models later on. To do this, we replaced all np.nan values with 0 as this made the most sense in context.

# Outliers
Here we were trying to investigate some of the very extreme outliers we saw in the bivariate scatter plots. We found the index and looked at the name of the recipe, and we found the title of the review was "How to preserve your husband", and it had an egregious cooking time of 17520 hours. This does not make sense at all so we removed it.  
The other two outliers also had quite high cooking times but when we looked at the name of the recipes, their names were "how to make homemade vanilla" and "how to make homemade fruit liquers". When looking up the standard time for these recipes, they ranged from a few weeks to a few months to even years, so the high cooking times made sense. Therefore we decided to keep these datapoints in the dataset.

# Univariate Plot Analyses

# Bivariate Plot Analyses



## Framing a Prediction Problem

## Baseline Model

## Final Model
