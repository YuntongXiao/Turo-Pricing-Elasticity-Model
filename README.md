# Turo-Pricing-Elasticity-Model

Turo, the firm, dubbed the "Airbnb of vehicle rentals," began operations in San Francisco and Boston in 2009 and has since spread to more than 5,000 cities across the United States, Canada, Germany, and the United Kingdom. It competes against well-known vehicle rental companies including National, Enterprise, Dollar, Avis, and Hertz, as well as comparable peer-to-peer (P2P) car-sharing services like Getaround. Different from the transition vehicle rental companies, Turo distinguished its position in the market by changing the game of supply side. 
As a platform provider, Turo allows hosts to choose the rental prices for their listings. One question that most hosts might have asked themselves when listing their cars is how to set my price. This is a price optimization problem that can be solved with machine learning modeling. The optimization objective of the model is to maximize the revenues, which requires estimation of demand for a range of prices. There are many factors that might affect the demand. One is vehicle category.  
Looking at the following website of the main top rental companies, what makes the Turo web page look different?  The vehicle category. Unlike other companies catergorizing listings into economy, mini, compact, and standard as shown below in Image 1, Turo lists the model, make, and the year of the car.  Such information fails to indicate the overall quality of vehicles in a straightforward manner. Therefore, we want to combine the car features and create similar vehicle categories that represent the quality of the cars. 

Traditional Approaches
	Following the pricing strategy model conducted from Airbnb dataset, we explore how the model predicts the optimal price for Airbnb listings. Basically, the research article written by Peng et al. (2018) introduces these three components; 1) Predict the booking probability for each night using a binary classification model, 2) Predict the optimal price for each night using a regression model, 3) Provide the final price suggestions applying additional personalization logic into the second model. Since car sharing from Turo is a quite similar business model when compared to Airbnb which provides an online platform for people to list or rent their places, we apply the first and second approaches into our model. 
Peng et al. (2018) use a k-d tree to divide all listings into k-dimensional clusters/nodes setting their booking probability as a dependent variable. Building a k-means clustering model setting renterTripsTaken (which is the number of renters) as our dependent variable as well as considering the geographical density of our listings in the neighborhood, we are able to create different car categories and discover the optimal price of each category.

Model Development
There are three steps involved in building the price model. First of all, we need to cluster cars into different categories based on their ratings, prices, brands and other features. Secondly, we use linear regression to find the price elasticity of each car category and estimate the demand curve. The demand function is denoted as D(p), where p is average daily price. Lastly, we compute the optimal price (p*) that maximizes Revenue = D(p)  p.  
To come up with the final optimal price for each category vehicle, the first step is to build a k-means clustering model to identify the best number of categories to split. Based on the 5 optimal number of categories, we build a log-log regression model based on each category. The regression model aims to evaluate the relationship between demand, which we estimate with the number of rental trips and the price, which we estimate with average daily price. 
Database description and cleaning 
	To solve our business problem and deliver valuable business insights for Turo, we use the data source from Kaggle (Turo Rental Car Pricing Info, API code: kaggle datasets download -d theriley106/turo-rental-car-pricing-info). We extract the data from the json file and transform it into a dataframe with some informative variables that help us formulate the following modeling. Since the difference between states in the US and the variance might highly influence the pricing amongst states, we collect population data from each state from https://www.census.gov/. We use population as a confounding variable to control the variance in the number of rental cars and economic level among states. 
Modeling and result interpretation
K-means Clustering 
To decide the optimal number of categories of the unsupervised classification, we apply 10 folders cross validation to get the SSE-k scree plot. The KneeLocator function shows k=3 is the optimal classification parameter. 

Log-log Regression Model
For the above 3 categories of rental cars, we run a log-log regression model separately to estimate the elasticity of demand and price. The regression model is built to control other variables that influence the relationship between demand and price, where we estimate the demand by the number of trips taken of each car. We choose our independent variables in three aspects:
Owner Features:rating, reviewCount, responseRate, etc.
Temporal Features: seasonality (listing month), ListingDifference (years of listing), population (states population density level and economic level)
Supply and demand dynamics: number of available listings in the neighborhood (number of available rental cars in the 10 miles neighborhood)

The coefficient of the average daily price in the log-log regression model indicates the elasticity of demand and price. For the 3 categories, we can observe the difference between the sensitivity of demand to the price. For category 1, when the price of rental cars increases 1%, the demand of this category will decrease 0.0064%. For category 2, when the price of rental cars increases 1%, the demand of this category will decrease 0.1733%. For category 2, when the price of rental cars increases 1%, the demand of this category will decrease 0.0919%. The demand for Category 2 is most sensitive to the adjustment of the price.
