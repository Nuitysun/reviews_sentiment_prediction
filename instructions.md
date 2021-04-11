# Technical requirements and implemented scripts description

### Technical requirements 

* Install Jupiter notebook (for this project version 6.0.3 was used)
* Check Python version (for this project version 3.8.3 was used)
* Install required Python modules via Jupiter notebook:
<pre>
!pip install dateparser==1.0.0
!pip install advertools==0.10.7
!pip install git+https://github.com/amaiya/eli5@tfkeras_0_10_1
</pre>

### Scripts description 

* **Scraping_reviews.ipynb** scrapes recent reviews data of Smartphones product category from the site https://rozetka.com.ua/. The script can be executed to scrape recent reviews data.
    * Script saves collected data to .csv files in <span style="color:grey">reviews_data</span> folder with the name of a file “smartphone_reviews _{current_date}.csv”. 

* **Predicting_reviews_sentiment_with_Bert_model.ipynb** contains script to read all smartphone_reviews_*.csv files from <span style="color:grey">reviews_data</span> folder, manipulate the data, build prediction models and evaluate their performance. 
    * Script saves:
        * .png files to <span style="color:grey">images</span> folder – images of reviews sentiment are used in README.md. 
        * data_clean.csv in <span style="color:grey">reviews_data</span> folder - this is the cleaned reviews data which can be used for conducting experiments and training different versions of models in separate scripts. 
    * Script uses pretrained BERT model – version 1. Model can be uploaded by link: https://files.fm/u/2msz9eexf – file “sentiment_prediction.data-00000-of-00001”.

* **Build_Bert_model_v2.ipynb** – contains script to read "data_clean.csv" from <span style="color:grey">reviews_data</span> folder, manipulate the data, load and train BERT model – version 2. This model is used in "Predicting_reviews_sentiment_with_Bert_model.ipynb". 
    * Script uses pretrained BERT model – version 2. Model can be uploaded by link: https://files.fm/u/2msz9eexf – file “sentiment_prediction_v2.data-00000-of-00001”.

>Note: All mentioned .csv files are provided in repository in <span style="color:grey">reviews_data</span> folder.