# Predicting reviews sentiment  from eCommerce marketplace Rozetka.ua with BERT model

## Technical requirements
**This project was implemented using:**
- Jupiter notebook-6.0.3
-  Python
- Python modules installed:
<pre>
!pip install dateparser 
!pip install advertools
!pip install git+https://github.com/amaiya/eli5@tfkeras_0_10_1
</pre>


## Short description of the project 
The project is focusing on exploration of smartphone reviews data from Ukrainian eCommerce marketplace Rozetka.ua and building model to predict sentiment of reviews.

After inspecting the web-site https://rozetka.com.ua/ I noticed that not all reviews have rating. 
If the amount of such reviews without rating is significant - it may be useful to build a prediction model to fill in missing ratings which will allow to see more complete picture of overall sentiment of Ukrainian consumers toward specific product categories, brands, products within specific price range, etc. Also, it can be useful information for marketplace to get more complete statistics on customers satisfaction with products/services. 
The purpose of this project is to check the feasibility of the idea and if it's possible to build a model to predict a sentiment of the  reviews with the high enough accuracy. Here we will focus on reviews of Smartphones product category.
It's also possible to apply the same approach to other product categories, but it's advisable to train separate models for different categories so a model can learn specifics in reviews of particular product category.

**Few words about marketplace Rozetka.ua which data we will explore** :
Rozetka.ua is a Ukrainian online store and marketplace that was founded in 2005. As of August 2020, the site ranks 7th among the most visited in Ukraine. 
Initially, the store sold household appliances and electronics, but today you can find there many more categories of products - from clothing to food. A total of 3.9 million products are presented on the site. It is visited daily by 2.5 million people. Such success was achieved through the involvement of third-party sellers - they generate 25% of sales at Rozetka. In addition to the marketplace, Rozetka has launched its own network of retail stores, delivers to its post offices and mobile delivery points, and entered the international market.
Being one of the biggest marketplaces in Ukraine, Rozetka.ua also became a site with the biggest number of reviews from Ukrainian consumers on wide range of products. This combined with fast and seamless service is allowing Rozetka to retain leading positions in Ukrainian eCommerce retail market.

## Data collection
The first step is to retrieve the product reviews data. 
The search for possibility to export reviews from web-site or to use exciting browser extension/plugin didn't bring results, so it was decided to collect the data using web-scraping (scrappy module in python).
The script which retrieves necessary data and writes it into the “smartphone\_reviews _{current_date}.csv” file is located in separate jupiter notebook **“Scraping_reviews.ipynb”**.

*Note: the developed  web-scraping script does not retrieve all available reviews. It is caused by the fact that web-pages are generated dynamically with “Show more” button. To solve this problem, it’s necessary to use tools like Selenium, which can interact with web-pages to get full data previously to parsing html. 
Here we will proceed with the data generated with the scrapy module functionality as it should be enough for purposes of this project.*

## Data preparation/analysis and predicting sentiment with BERTmodel
Script related to this section  is located in separate jupiter notebook **"Predicting reviews sentiment with Bert model.ipynb”**.
### Step 1: Read dataset and perform basic preprocessing
In this section following steps are performed:
- read datasets from the root folder, 
- fix data format and parsing issues, 
- perform check for duplicates,
- check content of review_text column to drop rows with not enough information,
- create column full_text which contains all text related  to the review,
- handle missing values.

### Step 2: Exploratory data analysis
In this section we explore:
- review_rating variable,
> &nbsp;&nbsp;&nbsp;&nbsp; Below is the plot with review_ratings values. We can see from it that significant proportion, about 32%, of reviews, don't have filled rating (0 value). So the task of predicting sentiment for 1/3 of all reviews that currently are not classified can be useful. 
Another thing that we can conclude – is that rating distribution in imbalanced – there are much more positive reviews than negative.

> &nbsp;&nbsp;&nbsp;&nbsp; ![review_rating values](/review_rating_values.png)

- product_price VS review_rating correlation,
- review_date VS review_date correlation,
- distribution of review_text by language in which they are written,
- TOP10 Unigrams, Bigrams, Trigrams of review_text variable,
- meta Features analysis of review_text variable,
- meta Features analysis of product_advatages variable,
- meta Features analysis of product_disadvantages variable.

### Step 3: Biulding prediction model with BERT
In this section we do necessary preparation of data for modeling:
- drop rows without ratings and without review text, 
- prepare target variable ‘review_sentiment, 
- perform train/test split,
- perform preprocessing with Bert mode.

After preprocessing is done we build BERT model, explore best learning rate with Learning Rate Finder, set weight decay ant train the model on training data. 

### Step 4: Evaluating model performance
In this section we:
- check confusion matrix, accuracy, precision and recall metrics on **training set**,
>- Accuracy - 89%.
> - Predictions for positive reviews are more precise: 93% of all predicted reviews are actually  positive and 92% of all positive reviews were detected correctly.
> -  Predictions for negative reviews are less precise: only 76% of all predicted reviews are actually negative and 80% of all negative reviews were detected correctly.
- check 5 top losses of the model on training set,
- make predictions and check confusion matrix, accuracy, precision and recall metrics on **testing set**.
>- Accuracy - 89%. 
>- Area Under the Receiver Operating Characteristic Curve on test dataset is 0.87, so there is a 87% chance that the model will be able to distinguish between positive and negative class for reviews.
> - Predictions for positive reviews are quite precise: almost 95% of all predicted reviews are actually positive and 90% of all positive reviews were detected correctly.
> - Predictions for negative reviews are less precise: only 73% of all predicted reviews are truly negative and 84% of all negative reviews were detected correctly.


### Step 5: Predict sentiment for reviews without rating
In this section we:
- predict sentiment for reviews without rating, 
- manually check predicted sentiments for reviews.

### Step 6: Building BERT model v2
Oversample negative class in the initial data to remove class imbalance in target variable and train BERT model version 2. 

### Step 7: Running BERT model v2 and evaluating model performance
In this section we check perfomace of BERT model v2 :
- check confusion matrix, accuracy, precision and recall metrics on **training set**,
> - Accuracy - 89%.
> - Predictions for positive reviews are more precise: 93% of all predicted reviews are actually positive and 92% of all positive reviews were detected correctly.
> -  Predictions for negative reviews are less precise: only 76% of all predicted reviews are actually negative and 80% of all negative reviews were detected correctly.
- make predictions and check confusion matrix, accuracy, precision and recall metrics on **testing set**,
> - Accuracy - 89%. 
>- Area Under the Receiver Operating Characteristic Curve on test dataset is 0.87, so there is a 87.2% chance that the model will be able to distinguish between positive and negative class for reviews.
> - Predictions for positive reviews are quite precise: almost 95% of all predicted reviews are actually positive and 90% of all positive reviews were detected correctly.
> - Predictions for negative reviews are less precise: only 73% of all predicted reviews are truly negative and 84% of all negative reviews were detected correctly.
- predict sentiment for reviews without rating, 
- manually check predicted sentiments for reviews,
- plot distribution of reviews by sentiment (positive/negative) for:
-- only data with filled review_rating: sentiment was calculated from review_rating values

&nbsp;&nbsp;&nbsp;&nbsp; ![review_with_rating](/reviews_sentiment_calculated_from_review_rating.png) 
 
-- for all data: sentiment was calculated from review_rating or predicted by model

&nbsp;&nbsp;&nbsp;&nbsp; ![for_all data](/reviews_sentiment_calculated_from_review_rating_prediction.png) 

## Conclusion

In this project we retrieved and explored reviews data from the web-site Rozetka.ua.
We were able to build a model to predict reviews sentiment that performed quite well on the training set and showed good results on the unseen data: it predicted correct sentiment for 95% of reviews in testing set.
Although, after predicting sentiment for the reviews without rating and inspecting 40 predictions - we can see that accuracy seems to be much less for them, about 70-85%. Content of reviews are often confusing, model doesn’t always identify sentiment correctly and sometimes it’s hard to identify it even with manual check.
Results of sentiment prediction for reviews without rating:
Model predicts that 41% of reviews without rating contain negative sentiment, while for reviews with rating it is only 23% (almost twice less). By adding sentiment classified by the model to the table with sentiment calculated from the rating set by customers, we can see that fraction of negative class in overall data increased from 23% to 29% .

Another aspect that is important to consider is that reviews records sometimes doesn't contain actual review of the product, but rather feedback about shop/service or questions. 
To improve the results, I would explore how to classify reviews into categories: product review, service feedback, question (record can have category product review and service feedback at the same time). 
This way we can refine statistics about sentiment regarding products. Also, it may be useful to analyze what sentiment customers express about marketplace itself and service received and the next step could be classifying feedback about service in the categories: with what customers are satisfied and what they complain about. 
