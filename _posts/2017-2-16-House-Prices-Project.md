Let's say that you somehow ended up in Ames, Iowa. Yes, Iowa. Just go with it. You're working with a real estate firm there and they asked you to build a model that predicts house prices. The firm also wants to know where the most sales take place, where the most expensive houses are, and if those change over time. How do you go about doing all of that? Well that's what I explored in this project.

Truth be told, this situation and its related dataset is pulled from a Kaggle competition. However, for the purposes of this project i've modified the parameters.

# The Data

The original dataset contains 82 features but for this project, I only worked with 19 of them. All of the data in the dataset is from the years 2006 through 2010. In my scenario, the real estate firm wants to record less data to reduce costs and therefore uses less features. For this project I read in only the columns of the dataset that i needed. Those columns were:

- Lot.Area
- Utilities
- Neighborhood
- Bldg.Type
- House.Style
- Overall.Qual
- Overall.Cond
- Year.Built
- Year.Remod.Add
- Roof.Style
- Roof.Matl
- Gr.Liv.Area
- Full.Bath
- Half.Bath
- Bedroom.AbvGr
- Kitchen.AbvGr
- Mo.Sold
- Yr.Sold
- SalePrice

# Data Cleaning

The dataset for this project was actually pretty clean. All of the column names were short and easy to understand and all of the values were properly formatted with no quirks to remove. All of the datatypes looked correct and i found no missing values. I only needed to create dummy variable columns for the columns with categorical values.

# Outliers

In order to build a good model I wanted to remove outliers from the dataset. I created a boxplot to show me any outliers in the SalePrice column, and I removed rows from the dataset where the sale price was less than the 5th percentile approximately and greater than approximately the 95th percentile. I also went through the columns with categorical values and removed rows from the dataset were the category appeared less than 10 times, as those would not help my model very much.

# Exploring the Data

Since the focus of analysis was based around the neighborhoods, I created a number of charts grouped by neighborhood in order to get an idea of how the market looks for each one. I created a chart for total number of sales per neighborhood. This showed me that the neighborhoods that with the most sales were NAmes and CollgCr. To answer the question of how sales look over time, I created another bar chart that shows the number of sales by neighborhood per year. Over our timespan of 5 years, those two neighborhoods maintained their status as the neighborhoods with the most sales. In general every neighborhood hovered around the same amount of sales year over year with some amount of fluctuation.

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/numsalesovertime.png?raw=true">

I also made similar charts that showed average sale price per neighborhood. From these I concluded that although there were a few fluctuations, the most expensive neighborhoods were NoRidge and NridgHt.

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/avgpriceovertime.png?raw=true">

Next, I created bar charts that showed the numbers of each building type and house style sold by neighborhood. In general, most houses in Ames seem to be 1 family homes that are either 1 or 2 stories.

In order to try to find correlations between the variables in the dataset, I created a pairplot of all the variables as well as 2 correlation matrices. The first correlation matrix did not include the categorical values while the second one did. However the results were the same as the variables with the strongest correlation to sale price were OverallQual, YearBuilt, YearRemodAdd, GrLivArea, and FullBath. Other correlations include GrLivArea with FullBath, HalfBath, and BedroomAbvGr. 

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/corrmatrix.png?raw=true">

# Linear Regression Models

In order to predict the housing prices for the market in Ames, I needed to build a model that would do just that. I decided to try out a variety of different models to see how they performed and then compared their results in order to select the best one. I started with a normal linear regression that included all of the variables we had in our dataset. This one did pretty well, having an r squared value of 0.8465. That means that the model predicted about 85 percent of the sale prices in our dataset correctly. The following chart shows the predicted SalePrice values against the true values in the dataset. The line represents the predictions.

<img src="https://github.com/crtogonon/crtogonon.github.io/blob/master/images/lr1regplot.png?raw=true">

Then I tried out another linear regression model. Only this time I used just the 5 variables with the strongest correlations to SalePrice as identified in my correlation matrix. Although it didn't do as well as my previous model, it still predicted values adequately and came in with an r squared value of 0.7475.

# Regularization

Next I decided to create some regularization models to see how they would perform. I started by build a lasso regression model with an alpha value of 1. The alpha values in regularized models help constrain the coefficients of variables in order to try to avoid making our model too specific to the data that we're working with. This model ended up with an r squared value of 0.846496, which is just slightly lower than our first model.

I wanted to see what would happen if I ran the lasso regression with a different alpha value, so I built another one with an alpha value of 40. This model gave me an r squared value of 0.8451.

Finally I decided to try to implement a different kind of regularization. For the last model, I built a ridge regression model to see how that would do. This model gave an r squared value of 0.8463.

# Cross Validation

For the final part of this project, I used cross validation to evaluate which model I created worked the best. To do this I implemented the cross validation method known as the k-fold method on all of the models that I built. The model that produced the heighest cross validation score was the lasso regression with an alpha value of 40. This is the most effective model that I built and would be the model that I would use to predict the housing prices in Ames, Iowa.

