# REMA_Leo_BILLANT_SCIA_2025
REMA Project of Léo Billant

Movie Recommendation System for Couples
This project implements a movie recommendation system for couples using the MovieLens and IMDb datasets. The system combines collaborative filtering with content-based features to suggest movies that both users in a couple might enjoy.

1 - Data Collection and Preprocessing

Loading of MovieLens and IMDb datasets using pandas
Merging of these datasets based on movie titles and years
Filtering of movies with less than 500 votes on IMDb
Filtering of movies with less than 50 ratings in MovieLens
Standardization of movie titles using a custom function standardize_title
Combination of genres from both MovieLens and IMDb datasets
Extraction of year information from movie titles

The preprocessing steps ensure data quality and relevance for the recommendation system.

2 - Feature Engineering

Feature engineering techniques:

Creation of a user-item interaction matrix using Spark's ALS algorithm
Extraction of genres as features
Utilization of release year as a feature
Incorporation of IMDb average rating and number of votes

These features are used to enhance the recommendation quality beyond simple collaborative filtering.

3 - Model Development

The model uses Spark's ALS (Alternating Least Squares) algorithm for collaborative filtering.

Why ALS ?

Implemented in Spark MLlib, ALS offers the scalability required to handle the large MovieLens and IMDb datasets efficiently. It integrates seamlessly with the PySpark-based data processing pipeline, maintaining consistency throughout the project. Moreover, ALS is designed to work effectively with sparse datasets, which is crucial given the typical sparsity of movie rating data, even after filtering for movies with a minimum number of ratings.

StringIndexer is used to encode user and movie IDs
The ALS model is trained with specific parameters (maxIter=10, regParam=0.1, rank=20, nonnegative=True)

maxIter : 10
A value of 10 is relatively low, suggesting a balance between computational efficiency and model performance. Increasing it would give better results.
However putting more causes the spark to crash.

regParam : 0.1
A value of 0.1 is moderate. Lower values allow more complex models but risk overfitting, while higher values create simpler models that might underfit.

rank : 20
A rank of 20 means the model is trying to capture 20 different latent features. This is a moderate value, allowing for some complexity while keeping computational requirements manageable.

nonnegative : True
This constraint ensures that all learned user and item factors are non-negative.

These parameters represent a balance between model complexity, training time, and potential for overfitting.

4 - Recommendation Algorithm

Algorithm for couple recommendations for a balanced recommendation that considers both users' preferences :

0.6 - Combines ALS predictions for both users
0.2 - Takes into account the IMDb average rating
0.1 - Incorporates genre preferences of both users
0.1 - Considers the preferred movie era based on the average year of rated movies

5  - Evaluation

Data is split into 80% training and 20% testing sets

Root Mean Square Error (RMSE) : 0.8670
Mean Absolute Error (MAE) : 0.6958

Scale context: 
Given that movie ratings typically range from 1 to 5, these error metrics suggest moderate accuracy. An error of about 0.7-0.9 on this scale indicates that, on average, the model's predictions are off by less than one rating point.

RMSE vs MAE: 
The RMSE is higher than the MAE, which is expected. RMSE penalizes larger errors more heavily due to the squaring of differences. The relatively close values suggest a consistent error distribution without too many extreme outliers.

Interpretation in context: 
For a movie recommendation system, these results are reasonably good. They indicate that the model can predict user ratings with an average error of less than one star, which is often sufficient for generating useful recommendations.

Dependencies :

- PySpark
- Pandas
- NumPy

Possible Improvements :

- Incorporating more features (directors, actors, ...)
- Improving the method for combining user preferences
- Experimenting with different algorithms
- Performing Hyperparameter tuning
