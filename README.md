# Recommendation System

## Business Case

Before Disney launched Disney Plus, the long awaited streaming service to rival Netflix and Amazon Prime Video, Disney built anticipation by promoting popular films from Star Wars, Marvel, and Pixar that would become easily accessible through the platform. But when it launched, it was clear that Disney's strategy was quality over quantity and the surprising lack of breadth couldn't fully satisfy users' desire to binge, an industry expectation set by Netflix.

To compete with Netflix, Disney Plus has hired us, a media analytics firm, to develop a recommendation system to guide users from the popular blockbusters to the potentially popular content in the long tail of Disney's existing intellectual property. This long tail includes older classics as well as more recent movies that are highly rated but not well known.

## Project Summary

This project aims to create a movie recommendation system using two different techniques. The first being Collaborative filtering or recommending movies based on similarities between user ratings. This was done by training a model that can predict a given users' ratings of all movies in a dataframe based on their new ratings and their similarity to other users. The second technique used was Content Based Filtering which aims to recommend movies similar to a user's favorite movies based on the descriptions. These two techniques were used in concert to output a list of movie recommendations to a user.

## Repository

This repository contains 5 notebooks with the following contents:
1. Data cleaning and EDA
2. API Calls
3. Modeling for Collaborative Filter
4. Modeling for Cosine Similarity Content-Based
5. Modeling for Combination of Collab Filter and Content-Based

## Data Cleaning and EDA

During EDA, we identified the most popular movies as well as the "Sleeping Giants," films that could be more popular if promoted to users. We used these subsets to help Disney Plus meet the industry expectation of a higher quantity of long tail content.
 
## Modeling

We evaluated several models based on the rmse metric. The models evaluated were as follows: Gridsearch CV for SVD, then SVDpp, NormalPredictor, KNNBaseline, KNNBasic, KNNWithMeans, and KNNWithZScore. The model that performed the best was SVDpp. SVDpp is a matrix factorization method that takes into account both implicit and explicit ratings. The hyperparameters were tuned to acheive the lowest rmse possible. Rmse was evaluated using a training and testing dataset.
![model_rmse](https://github.com/bpbull/recommendation_system/blob/brendan-wip/images/model%20rmse.png)

# Recommendation Systems

## Collaberative Filtering (CF) with SVDpp

The first recommendation technique utilized in this project was collaborative filtering with SVDpp. This recommednation method essentially predicts a user's rating for every movie in the dataset based on their existing rating similarity to other users. It is sometimes refered to as a user-user recomendation system. The system takes in a new users ratings of 4 movies then returns a list of movies that the model predicts the user would rate highly. This technique performed poorly due to its tendency to predict that any user will rate certain movies highly - those that have a high number of high ratings. 
![ratings:movie](https://github.com/bpbull/recommendation_system/blob/brendan-wip/images/ratings:movie.png)
To combat this, we took a random sample of the top 30 movies the system would recommend so that it wasn't always recommending the same top 5. To further improve our recommendations, we decided to utilize a content based recommendation system, as this will recommend movies based off of actual similarities between the movies, not user similarities. This is referred to as an item-item system and performed much better with the movielens dataset.


## Content Based System

### Importing new features for Content Based System

Several additional features were added to the movie dataframe (description, etc.) using a tmdb api call in order to increase the specifity of content based recommendations. The new features were cleaned, stopwords removed, and tokenized using the nltk library.
![most_commonwords](https://github.com/bpbull/recommendation_system/blob/brendan-wip/images/most_commonwords.png)

### Creating the system

With a new dataframe ready, the cosine similarity between descriptions was calculated using the TF-IDF Vectorizer. This resulted in a similarity matrix between movies which we used to make recommendations.

## Hybrid System

We merged the two recommendation systems to output a list of ten movies, five from each system. This Collaborative Filter system tended to recommend older movies which was beneficial for the Disney+ business case.
![Screen Shot 2020-05-08 at 9.21.15 AM](https://github.com/bpbull/recommendation_system/blob/brendan-wip/images/Screen%20Shot%202020-05-08%20at%209.21.15%20AM.png)


# Conclusions and Further Actions

Collaborative Filter System: For our business case of Disney+, the CF system is useful as it recommends old, highly rated movies, however, if the business case was to recommend more relevant and currently popular movies, then filtering by movies made after a certain year, say 2010 would be more ideal. 

Content Based System: The content based system worked well and ouput movies that were similar to a user's favorite film.  However, sometimes the recommendations were too similar but in a different genre or had a different suitability rating. For example, the system would recommend R-rated movies when a PG movie was input. This was because the suitability rating (PG, PG-13, etc.) was not included in the tmbd data. We can improve this system by including conditional statements in the recommendation function to filter out inapproproate or unrelated recommendations based on their genre, release year, and suitability rating.

