The task for this project was to collect movie data from imdb with the goal of building a random forest model to predict imdb Ratings for movies, and then examine the feature importances in order to get an understanding of the factors that contribute to those ratings. To get the data for this project I needed to use scraping as well as API requests to get the information I needed, and the data I used was of the top 250 movies according to imdb.com.

# Risks and Assumptions

For this project I was working with a small sample size and those samples are all of the highest rated movies in imdb. In order to build a better model that would work well on all movies, I would need a bigger sample size that includes movies of all ratings to train my model on.

# Obtaining the Data

As I mentioned earlier, I needed to use a combination of scraping and API requests to get the information I was looking for. To begin, I needed to scrape the imdb page that listed the top 250 movies of all time (according to imdb rating) in order to get the imdb for each movie on the list. I needed those IDs so that I could insert them into the API get requests that would help me gain the data. After obtaining all of the data for the movies on the top 250 list, I created a dataframe with it, which is what I used to build my model.

# Data Cleaning / Mining

Before I could start building the model, I first needed to clean the data and then mine it for the features that I needed. The original dataframe obtained from the data returned by the API requests needed a lot of work before any models could be made.

The first thing I needed to do was deal with the 'N/A's in the dataframe. For cells with missing values, the string 'N/A' was placed. This is not the same as a null value, which is indicated by NaN in a pandas dataframe. So I needed to replace those 'N/A' strings with actual null values. Now I could start taking care of mmissing values. The first column with a missing value was for the languages of each movie. One movie did not have a language listed, so I did some research on the movie and found that it was a silent film from the 1920s and therefore no languages were actually spoken in the film. For this reason I imputed the value of 'None' for this movie. I then had to repeat the research process for 3 movies that had no rating listed (as in R or PG). After searching for a long time and being unable to find ratings for these movies myself, I decided to impute a value of 'NOT RATED' for these movies. 'NOT RATED' is a value that is listed for other movies where there is not rating. There were two movies that were missing release dates, so I needed to research values for those. For one of them I successfully found a release date, but for the other I could not find the original release date so I used the American release date instead. From the released date, I created variables for month and day released. Year was not necessary since a year column already existed. After that I formatted the numerical variables into their proper formats. For the columns that contained text in the cells, I used a combination of count vectorizers, TFIDF vectorizers, and pd.get_dummies to get features that I needed for my model. After getting all of my features and making sure everything was formatted properly, I was able to start building my model

# The Random Forest Model

In the original dataset, the target variable was the imdbRating. This rating was number ranging from 8.0 to 9.3 in this sample set. I found that about half of the movies scored 8.2 or less, so I decided to use that as a cut off point and have my target variable be a binary classification of whether or not a movie was rated 8.3 or higher. 

After standard scaling my training set, I was able to build the Random Forest model. Using cross_val_score, I ended up with a cross validated accuracy score of about 62 percent. I then tried a gradient boosting classifier to see of that would get better results. The cross valiation score for that model was about 66, so I was abe to get a small improvement.

Next, I checked the feature importances to see which factors contribute to the higher ratings. I found that the most important factors were the number of imdb votes, runtime, release year and day, metascore, and number of oscar nominations. Although some of those factors made sense to me, I thought it was interesting that the movie runtime and release year and day ended up being important.

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/importances.png?raw=true">

# Visualizations

I was able to make histograms of some of the variables in order to understand the data better. Here are some insights I found...

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/ratinghist.png?raw=true">

Most of the movies in the imdb top 250 of all time list have an imdbRating between 8.0 and 8.5. I'm guessing that movies that score higher than that are the few that can be regarded as some of the absolute best of all time.

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/yearhist.png?raw=true">

When it comes to release years of movies, each progressive year has more and more movies in the top 250. This could be due to any combination of factors. As time goes on, more and more movies are getting made. Also, one could say that over the years film makers keep getting better in the art form. You could also, say that people probably tend to put in votes for movies they have seen, and especially in the current era of technology, not too many people are likely to have seen older movies. Whatever the case, I found it interesting that the top 250 list is populated by primarily more relatively recent movies. There was a spike in the number of movies from the 1950s though. They must have made some good stuff in that decade.

I was also able to create a pairplot of some of the variables involved. I wanted to see if there were any visible relationships in the data that I could notice, but the only one that stood out to me was that the more recent a movie was, the more imdb votes it tended to get. This makes sense to me since, as I mentioned before, people voting today likely have not ever seen the older movies.

# Next Steps

In terms of next steps when it comes to this model, I think that the obvious thing to do would be to collect more data. I would try to collect data on as many movies as possible and try to get movies with ratings ranging from the lowest all the way to the highest. I think that this would help in creating a model that would more accurately predict the rating of a movie.