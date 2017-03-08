For this project, the focus was on building and optimizing a few different classification models. The dataset that I worked with was that of 1912 Titanic disaster. The goal was to build a Logistic Regression model and then optimize its parameters using gridsearch. Then I needed to repeat that process for a K Nearest Neighbors Classifier and a Decision Tree Classifier.

# Risks and Assumptions

One assumption that I am making in this dataset is that this database perfectly covers everyone on board. I'm assuming that no one was missed and that no one snuck on board at any point. The risk in the dataset comes from the columns that have missing values, which I will adress below. Also, the selection of features for the models may be different from one person to another, depending on one's opinion of a column's importance to the target value.

# Getting the Data

Getting the data for this project was pretty straight forward. It was located on a remote database, so I used PostreSQL to connect to it and then used SQL to query the information that I needed. When I got the table with the information that I needed, I saved it as a dataframe so that I could do my analysis and build my models.

# Cleaning / Mining / Feature Engineering

When I inspected the data that I had in my dataframe, there was some work that needed to be done before I could build my models.

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/df1.png?raw=true">

To start, I decided that I would drop the passenger ID number, Name, and Ticket Number. I believed that none of those columns had any relevance as to whether or not a passenger would survive. To adress the 'Sex' column, I needed to create a dummy variable column to replace it. In this case I create a column indicating whether a passenger was male (indicated by a 1) or female (indicated by a 0). In the cabin column, some passengers had multiple cabins listed. It appeared as if groups of passengers were sharing sets of rooms. In this case, I decided to just make a replacement column that indicated which deck each passenger's cabin was in. These decks went from A through G and 1 passenger had a deck listing of T. After that, I had to make sure that each column was the appropriate value such as integer or float.

Before going any further I checked the dataframe for any null values. I found that the Age column was missing 177 values of the 891 in the dataset. The Embarked column had 2 missing values and the Deck column was missing 687 values. That's a lot. I wanted to keep the Deck column in my model, but I felt that there were too many missing values and I did not think that imputing a vast majority of the values was a good idea. I reluctantly decided to drop the column from my model. Though the Embarked column was missing only 2 values, I ultimately decided to drop it. I felt that the port that a passenger boarded the ship would have little to no impact on whether or not they would survive an incident that they did not see coming. As for the Age column, I felt that it was too important of a variable to drop. For the missing values in that column, I decided to impute those missing values. Instead of using a mean, median, or mode, I felt that it would be better idea to use a linear regression to fill the null values. I built a linear regression and used the predictions from it to complete the rows containing missing age data. In the end, I had a dataset that looked like this:

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/df2.png?raw=true">

# Building / Optimizing models

Now that I had my data ready, it was time to make those models. First on the list was Logistic regression. Using cross_val_score, I found that this model had a mean accuracy score of about 0.79. I ran GridSearchCV to find the optimal parameters, and with those parameters the highest accuracy scored obtained was approximately 0.80. I then tried using a bagging classifier with the logistic regression and I found that the mean accuracy score was also approximately 0.79, which was similar to the original model.

The next model to build was the K Neighbors Classifier model. This one did not perform as well as the Logistic Regression and had a mean cross validation score of 0.71. Once again I used gridsearch to optimize parameters and the best score found using that method was 0.73. Again I tried the bagging classifier, but this one gave me a slightly lower score than the original knn model, coming in at 0.70.

The last model on the list was the decision tree model. This model resulted in a mean cross validation score of 0.78, and after optimization the best score found was 0.82. The bagging classifier gave a score of 0.81.

Comparing the scores of all of the models, I found that the logistic regression and decision tree models performed similarly while the knn model did not perform quite as well.

# Confusion Matrices and ROC curves

For each of the models, I created confusion matrices and ROC curves to see how they would perform. They showed that the models performed fairly well as the numbers for the true positives and negatives were pretty good. Upon inspection the knn model's confusion matrix showed that its predictions were not as good as the other models, which lines up with what I saw with the cross validation scores for each model. The ROC curves for all of the models look pretty good as they show a good amount of area under the curve for each of the models. The following are the confusion matrix and ROC curve for the logistic regression model:

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/confusionmatrix.png?raw=true">

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/ROCcurve.png?raw=true">

# Conclusion

In the end I was able to build three models that performed reasonably well when predicting the likelihood of a passengers survival. Of the three, however, I would use the decision tree classifier first since it gave the best scores. You would not go wrong with a logistic regression either, since its scores were only slightly lower. Although the knn model performed fairly, its results were significantly worse than the other two models. The confusion matrices and ROC curves support the results found from cross validation scores.

