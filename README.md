# Harnessing Machine Learning for Natural Disaster Alert Systems

## Tunde Duro-Iadipo, Edith Iyer-Hernandez, Kevin Aguiar

### Problem Statement:
Large scale alert systems come in a variety of forms and are based on a variety of informational sources. Normal google alerts tend to be depended on official sources like USGS. While these official sources are effective, it seems like a relatively untapped source of information could be social media posts - people tend to post on twitter, facebook, instagram and snapchat when there is some sort of event. In this project we investigate harnessing social media to help for rapid alert and notification in the event of a natural disaster, with a long term goal of integrating a machine learning model into a web app.
<br>
In order to better understand when tweets are about an alert worthy event like an earthquake or wildfire in their area, we decided to utilize natural language processing of tweets. This way, we have created a workflow that can be modified very simply depending on needs.  We were able to build a model that can quickly analyze a tweet and predict whether or not it can bee classified as 'alert worthy' or 'not alert worthy'. In the first iteration, we focused solely on one type of natural disaster, tsunamis, in order to ensure success before expanding. We \ developed the model building workflow to be easily scalable to larger datasets, increased variety of natural disasters or modified to work with a different specific disaster. We then created a function to make predictions on incoming tweets and returns either "alert worthy" or "not alert worthy". This function could easily be integrated into a web app or twitter bot to automatically send out alerts.

### Data Gathering Process
We wanted to build a supervised learning model to correctly predict if a tweet is 'alert worthy' or 'not alert worthy' regarding a tsunami, so we had to find a way to access twitter data with a target variable. We utilizing a combination of a Kaggle dataset

Finding Data to train a predictive model on (a model that classifies a new, unseen tweet as ‘alerty-worthy’ or ‘not alert-worthy’) was difficult, at first.  Fortunately, though, we were able to find a labeled Data set on Kaggle’s website, as mentioned above (find the notebook used to clean this __[here](https://git.generalassemb.ly/EdithIyerHarnessing-Machine-Learning-for-Natural-Disaster-Alert-Systems/blob/master/Notebooks/Kaggle%20Dataset%20Import%20and%20Cleaning.ipynb)__.  This dataset contained tweets regarding multiple types of Natural Disasters, but only contained 50 tweets regarding tsunamis.   
We were able to web scrape ~1,500 historical tweets from the #tsunami webpage, with the scraped tweets dating back to December 15, 2017.  The tweets were then put into dictionaries containing the handle the tweet was sent from, the tweet itself, the tweet’s timestamp of when it was sent out, and the date it was sent out.  We then labeled 300 tweets with 50 as “alert-worthy” and 250 as “non-alert-worthy” (find the hastag scrape notebook __[here](https://git.generalassemb.ly/edithih/Harnessing-Machine-Learning-for-Natural-Disaster-Alert-Systems/blob/master/Notebooks/Hashtag%20Page%20Scrape.ipynb)__ and the notebook used to clean __[here](https://git.generalassemb.ly/edithih/Harnessing-Machine-Learning-for-Natural-Disaster-Alert-Systems/blob/master/Notebooks/DF%20Cleaning.ipynb)__. In total, we were able to gather 350 labeled tweets to train out model on.
Additionally, we created a livestream function that searches for #tsunami using twitter's API. The notebook with this process can be found __[here](https://git.generalassemb.ly/edithih/Harnessing-Machine-Learning-for-Natural-Disaster-Alert-Systems/blob/master/Notebooks/twitter%20API%20livestream.ipynb)__.


Pre-processing/Model Building Process IS THERE A SPLIT BEFORE LEMMATIZATION OR DOES LEMMATIZATION INCORPORATE THAT STEP and is PCA still incorporated

The Pre-processing stage included: lemmatizing the tweet (stripping the words down to their root/base), removing non-dictionary text (for example, things like “aaaaand”), “vectorizing” the remaining, lemmatized words, and then running the returned data through our predictive model.
	The Model Building process included:  Which models were tried?  Hyperparameters?  What was our best model and why?  What was our metric of success?

The notebook used for preprocessing and model selection can be found __[here](https://git.generalassemb.ly/edithih/Harnessing-Machine-Learning-for-Natural-Disaster-Alert-Systems/blob/master/Notebooks/Final%20Model%20Selection%20Workflow%20and%20Predictions.ipynb)__


How would the application work?

The application’s workflow, as it stands, would look as follows:

Tweet Gathered using Twitter’s API  Tweet Passed through Pre-processing  Tweet Passed through Classification Model  Return Classification as “alert-worthy” or “non-alert-worthy”  If “alert-worthy”: Send Alert  If “non-alert-worthy”: Do Nothing

Shortcomings

With more time, we could:
1.	Gather/Label more tweets to build our model on.
2.	Build models/applications dedicated to other Natural Disasters, like tornados, earthquakes, mass shootings, etc.
3.	Build models/applications that work with other languages, like Spanish, Chinese, etc., so that the model can use tweets in different languages.
4.	Improve our model!  It is currently optimizing Recall (because we would rather a False Positive over a False Negative); with this said, though, we would like to further reduce the possibility of a False Negative and a False Positive.
5.	Set determining factors with regard to labeling a tweet as “alert-worthy” or “non-alert-worthy”

Recommendations

1.	Begin using Twitter’s API ASAP to record tweets containing #tsunami; these tweets can be saved to add even more data to train on.
2.	Search through the web-scraped tweets for more alert-worthy tweets to balance the classes more.
