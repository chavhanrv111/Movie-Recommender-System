                                           Movie-Recommender-System
                                                                  
Recommender systems, also labelled recommendation systems, are statistical algorithms that recommend products to users based on similarities between the buying trends of various user or similarities between the products.In this project you will see how to create a simple movie recommendation system, which recommends movies to a user using item-based collaborative filtering.Collaborative Filtering- The process used to calculate similarities between the buying trends of various user or similarities between products is called collaborative filtering. Collaborative filtering on the whole, classified into two types- User-based collaborative filtering Item based collaborative filtering. User-Based collaborative filtering is dependent on user choices. For example, in a recommender system based on user-based collaborative filtering, if two user X and Y, like products A and B and there is another user Z who likes product A, the product B will be recommended to user Z.One of the main disadvantages of user-based collaborative filtering is that user choices are evolved over time.In addition, the number of users is higher than products. Hence, creating user based collaborative filtering becomes a complex task statistically.

Item-Based collaborative filtering- In item based collaborative filtering, products are recommended based on similarities between themselves. For instance, if a user likes product A and product A has properties X and Y, another product B with properties X and Y will also be recommended to the user. Item-based collaborative filtering is eliminating user dependency and even f user choice changes over time, the properties of products remain unchanged.

## libraries :

- matplotlib
  - pyplot
- numpy
- pandas
- seaborn

### Load movies.csv and ratings.csv
<img width="408" alt="image" src="https://user-images.githubusercontent.com/85149334/202867856-84baf596-b142-4760-9b99-747ea9c4d033.png">
<img width="337" alt="image" src="https://user-images.githubusercontent.com/85149334/202867880-c9b47ca4-ced0-42c5-928d-d38b559ec72f.png">

 
 ### Data Preprocessing
 We need a dataframe that consists of userId, movieId, title and ratings. We cna create such a dataframe by mergind movie_ids_titles and move_ids_ratings dataframe using the movieid column since the movieid column is the same between the two dataframes
 
 <img width="240" alt="image" src="https://user-images.githubusercontent.com/85149334/202867958-199febb0-3c5b-4dd4-a5fb-1920a5fc71f3.png">

### Data Visualisation
Let's first group the dataset by title and see what information we can get regarding the ratings of movies

<img width="940" alt="image" src="https://user-images.githubusercontent.com/85149334/202868124-3b3b9c6b-1ed5-43d3-97b2-7bb01d6cff88.png">

Let's sort the movie titles by the descending order of the average user ratings

<img width="257" alt="image" src="https://user-images.githubusercontent.com/85149334/202868165-cc921c3c-96d3-4d78-b650-26a01e7d18b2.png">

The output shows the names of some not so famous movies. This is possible because some unknown movies might have got high ratings but only by a few users. Hence, we can say that average rating alone is not a good criterion to judge a movie. The number of times a movie has been rated is also important  Let's now print the movies in the descending order of their rating counts

<img width="238" alt="image" src="https://user-images.githubusercontent.com/85149334/202868213-6626ab76-7f15-465b-b95c-c3c85b6c7507.png">

A movie which is rated by large number of people is usually a good movie 
Let's create a new datafrae that shows the title, mean rating and the rating counts

<img width="311" alt="image" src="https://user-images.githubusercontent.com/85149334/202868259-1d4f86ff-1be3-4283-92ee-99c964bf67e8.png">

The above dataframe contains movie title, average ratings (ratings_mean), and the number of rating counts (rating_count) We will plot a histogram to see how the average ratings are distributed.
Most of the mopvies have an average rating between 3 and 4

<img width="455" alt="image" src="https://user-images.githubusercontent.com/85149334/202868287-ef6f28db-5220-445d-a01f-fc79abafb62e.png">

### Distribution for rating counts

There are around 7,000 movies with less than 10 rating counts. The number of movies decrease with an increase in ratings counts. Movies with more than 50 ratings are very few.

<img width="468" alt="image" src="https://user-images.githubusercontent.com/85149334/202868327-7677d6a4-6598-4faf-96c8-8e55cb56d1ce.png">

It is also interesting to see the relationship between mean ratings and rating counts of  a movie

<img width="480" alt="image" src="https://user-images.githubusercontent.com/85149334/202868360-8e265b13-148f-4754-a971-a8f94e88f9ea.png">

From the above graph in the top right portion, you can see that the movies with a higher number of rating counts tend to  have higher mean ratings as well.
Let's sort our dataset by rating counts and ee the average ratings of the movies with the top 5 highest number of ratings.

<img width="366" alt="image" src="https://user-images.githubusercontent.com/85149334/202868404-0c011358-42f2-4d41-81b0-bd37451361c3.png">

### Item Based Collaborative Filtering

In item based colloborative filtering, products are recommended based on common characteristics. For, instance in a movie recommender system based on item-based collaborative filtering, a common characteristic could be the director of a movie, the actors in the movie, and so on.

In our dataset we have average ratings for different movies given by different users. We will use the average ratings as the common characteristic of the collaborative filtering of movies
The first step is to create a dataframe where each movie is represented by a column and rows contain user ratings for  movies

<img width="927" alt="image" src="https://user-images.githubusercontent.com/85149334/202868461-4e0b1ed6-a234-4230-8821-3c4319e6f8c7.png">

Dataset contains 610 unique users and 9,719 unique movies
Now, we will find movie recommendation based on a single movie and then based on multiple movies

Finding recommendation based on a single movie- Suppose we want to find recommendation based on the movie Pulp Fiction (1994)
- First, we will filter the column that contains the suer ratings for the movie
- Next, we will find the correlation between the user ratings of all the movies and the user ratings for the movie pulp fiction.

<img width="380" alt="image" src="https://user-images.githubusercontent.com/85149334/202868586-c021319e-58a4-4ba8-84f9-710ac88eb7c3.png">

Correlatio itself is not giving meaningful results, one solution to this probelm can be that in addition to the correlation between the movies, we also use rating counts, for the correlated movie as a criterion for finding the best recommendation

<img width="388" alt="image" src="https://user-images.githubusercontent.com/85149334/202868626-19a0c1c2-8b93-41b4-987c-b4a819a06190.png">

The pf_corr column contains some NaN values. This is because there can be movies that are rated by users who did not rate Pulp Fiction (1994). In such cases, correlation will be null.
We will remove all the movies with null correlation with Pulp Fiction (1994)

<img width="455" alt="image" src="https://user-images.githubusercontent.com/85149334/202868675-0b834ccd-a3ba-41d9-943d-bb247f5038c7.png">

A better way is to find the movies with rating counts of atleast 50 and having the highest correlation with Pulp Fiction (1994)

<img width="282" alt="image" src="https://user-images.githubusercontent.com/85149334/202868711-8073d9d7-03eb-4689-a280-d433f737c8cf.png">

### Finding Recommendations based on multiple movies
In this we will see how to recommend movies to a user based on his  ratings of multiple movies. The first step is to create a dataframe, which contains a correlation between all the movies in  our dataset in the form of a matrix.
The min_periods attribute value specifies the minimum number of observations required per pair of columns to have a valid result. A min_periods value of 50 specifies calculating correlation for only those pairs of movies that have been rated by atleast 50 same users. For, the rest of the movie pairs, the correlation will be null

<img width="938" alt="image" src="https://user-images.githubusercontent.com/85149334/202868770-1fefd579-b896-43eb-b677-49abea41d6f2.png">

Now suppose a new user logs into you website. The user has already watched three movies and has givn rating  to those movies.

<img width="200" alt="image" src="https://user-images.githubusercontent.com/85149334/202868796-b844c204-80fe-4db8-ba89-6eb62e9542ec.png">

From the all_movie_correlations dataframe, let's obtain correlation values for the movies related to Forest Gump (1994)

<img width="304" alt="image" src="https://user-images.githubusercontent.com/85149334/202868839-86ef5453-cd55-461e-894f-e9c3f5d469cd.png">

Now, you know how to obtain names and ratings of movies from the test_movie dataframe and how to obtain correlations  of all the movies with a single movie using the movie title
- Next, we will iterate through the three movies in the test_movie dataframe, find the correlated movies, and then multiply the correlation of all the correlated movies with the ratings of the input movie.
- The correlated movies, alongwith the weighted correlation (calculated by multiplying the actual correlation with the raitngs of the movies in the test_movies dataframe) are appended to an empty series named recommended movies

<img width="300" alt="image" src="https://user-images.githubusercontent.com/85149334/202868900-108d42da-f2d0-4f73-baef-62c199238bc7.png">

- To get a final recommendation, you can sort the movies in the descending order of the weighted correlation

<img width="299" alt="image" src="https://user-images.githubusercontent.com/85149334/202868929-545300f6-6185-4ee5-b52f-6f637e1d8a2f.png">









