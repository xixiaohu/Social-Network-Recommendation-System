# Social-Network-Recommendation-System
This nascent social networking site is focusing on users being able to follow other users based on common declared interests. The social network of followers and followees is available as well as each user's declared interest categories (each user can have zero or more declared interests).
I tried to implement a recommendation engine capable of predicting which user will follow another user. Four approaches are used and results are compared with each other.

Data Preprocessing:
Interest profile of each user is transformed by one-hot encoding, binary indicator for 833 interests:
- 0 means the user does not have a particular interest
- 1 means the user has a particular interest
Since all of algorithms started by finding users with similar interest, I filtered out “follower_id” and “followee_id” in the follows table that are not in the “user_id” list from the interest table. The reason is that the algorithms would not be able to recommend users with no interest profile in database.
The number of followees I recommended for each user depends on the number of followees this user actually followed. For example, if a user”2” followed 22 other users, algorithms would output 22 recommendations for this user”2” to follow.

Algorithms:
1. 'Popular' Method: Recommend most popular people. "Popular.ipynb" 
2. User-Based Collaborative Filtering: Pearson Correlation
  I first found the most similar users to a particular user based on their interests. Then I recommended the followees of those similar users (People who have similar interest as me will also follow ...). I picked the 5 most similar users by the highest Pearson correlation coefficients. Then I assigned weights to each of 5 users based on the correlation coefficients and used the weights to calculate a recommendation score for each followees. At last, I sorted the recommendation score and recommended the n followees of highest score to that user where n is the number of people that user already followed. 
3. Item-Based Collaborative Filtering: KNN
  In item-based filtering, I recommend followees with the most similar profiles, regarding all 883 interests, with the targeted users. I applied two distance metrics, “cosine” and “Jaccard”, to evaluate the model. Cosine metric simply measures the similarity between the two interest vectors, whereas Jaccard metric is more suitable for vectors with only Boolean values. 
4. Bayesian Classifier: supervised learning method
  Compared to lazy learners, Bayesian classifier can quickly build a model given a training dataset and classify the instance faster. In this case, I used Bernoulli NB, the matrix of user_id * interest as X and the matrix of follower_id * each followee_id as Y. For each Y, I trained the model with X. For each followee, I fitted each follower’s interest in the model and returned a score, which is the probability of following this followee. After this, I got a matrix of the probability of each followers following each followee. Then for each specific follower, I sorted the probability to return the followee id with the largest likelihood that this follower will follow. 

Algorithm Evaluation: Precision and F1 scores are used to evaluate the model.
