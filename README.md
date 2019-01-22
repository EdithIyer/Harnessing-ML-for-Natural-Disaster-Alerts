# Harnessing Machine Learning for Natural Disaster Alert Systems

## Tunde Duro-Iadipo, Edith Iyer-Hernandez, Kevin Aguiar

### Problem Statement:
Large scale alert systems come in a variety of forms and are based on a variety of informational sources. Normal google alerts tend to be depended on official sources like USGS. While these official sources are effective, it seems like a relatively untapped source of information could be social media posts - people tend to post on twitter, facebook, instagram and snapchat when there is some sort of event. In this project we investigate harnessing social media to help for rapid alert and notification in the event of a natural disaster, with a long term goal of integrating a machine learning model into a web app.
<br>
<br>
In order to better understand when tweets are about an alert worthy event like an earthquake or wildfire in their area, we decided to utilize natural language processing of tweets. This way, we have created a workflow that can be modified very simply depending on needs.  We were able to build a model that can quickly analyze a tweet and predict whether or not it can bee classified as 'alert worthy' or 'not alert worthy'. In the first iteration, we focused solely on one type of natural disaster, tsunamis, in order to ensure success before expanding. We \ developed the model building workflow to be easily scalable to larger datasets, increased variety of natural disasters or modified to work with a different specific disaster. We then created a function to make predictions on incoming tweets and returns either "alert worthy" or "not alert worthy". This function could easily be integrated into a web app or twitter bot to automatically send out alerts.

***

### Data Gathering Process
We wanted to build a supervised learning model to correctly predict if a tweet is 'alert worthy' or 'not alert worthy' regarding a tsunami, so we had to find a way to access twitter data with a target variable. We utilized a combination of a 50 prelabeled tweets from a Kaggle dataset and manually-labeled set of scraped tweets. Additionally, we utilized the twitter API to create a live stream of tweets for unseen data to make predictions on.
<br>
<br>
__Important Notebooks for Data Gathering and Cleaning__
__[Kaggle DataSet Cleaning](https://github.com/EdithIyer/Harnessing-ML-for-Natural-Disaster-Alerts/blob/master/Notebooks/Kaggle%20Dataset%20Import%20and%20Cleaning.ipynb)__
This dataset contained tweets regarding multiple types of Natural Disasters, but only contained 50 tweets regarding tsunamis.
<br>
<br>
__[Scraping](https://github.com/EdithIyer/Harnessing-ML-for-Natural-Disaster-Alerts/blob/master/Notebooks/Hashtag%20Page%20Scrape.ipynb)__
__[Cleaning and Labeling Tweets](https://github.com/EdithIyer/Harnessing-ML-for-Natural-Disaster-Alerts/blob/master/Notebooks/Cleaning%20and%20Labeling%20Tweets.ipynb)__
We were able to web scrape ~1,500 historical tweets from the #tsunami webpage, with the scraped tweets dating back to December 15, 2017.  The tweets were then put into dictionaries containing the handle the tweet was sent from, the tweet itself, the tweet’s timestamp of when it was sent out, and the date it was sent out.  We then labeled 300 tweets with 50 as “alert-worthy” and 250 as “non-alert-worthy”
<br>
<br>
__[Twitter API Stream](https://github.com/EdithIyer/Harnessing-ML-for-Natural-Disaster-Alerts/blob/master/Notebooks/twitter%20API%20livestream.ipynb)__

***

### Preprocessing
Before we began to build our model, we had to preprocess our tweet data so that it could be used with a variety of machine learning models. In order to do this, we have a few steps to go through:
1. Manually removing excessive symbols and numbers from each tweet through the use of regular expressions.
2. Lemmatizing the words within each tweet down to it's *lemma* or stem. This ensures that 'bike', 'bikes', and 'biking' all get cut down to 'bike'.
3. Vectorization: this turns each tweet into a vector of individual words for computation. We created a function that we could use for the training model as well as output a fitted vectorizer model to use to transform any new dataset we might receive.
4. Add target column to vectorized dataframe
***
### Imbalanced classes
When we selected tweets for model building, we ran into the fact that the number of tweets that are considered 'alert worthy' is significantly lower that the number of tweets that are considered 'not alert worthy'. Therefore, only about 20% of our tweets were actually classified as 'alert worthy'. This issue is called __imbalanced classes__. There are several methods to handle imbalanced classes and most come with important cons to take into account. For example, we could just oversample from the minority class, meaning we resample with replacement from the existing data points. This method, however is highly susceptible to overfitting as it increases the concentration of the same datapoints. We chose to utilize a method called SMOTE or Synthetic Minority Oversampling Technique. SMOTE is able to add data points by interpolating new points based on some number of nearest neighbors. We utilized a manual gridsearch to determine the best parameters for our SMOTE transformation before deciding and then used the best parameters to fit and transform the data to SMOTE.

***
### Principal Component Analysis (PCA)
PCA is a method aimed to address the issue of over fitting in models with high complexity. One of the causes of overfitting is having a really large number of features. Additionally, when a dataset is very wide, meaning the number of rows are significantly lower than the number of columns, models have trouble fitting in a way that produces good results. We refer to the process decreasing the number of features as Dimensionality Reduction. Dimensionality reduction leads to increased computational efficiency, can address the issue of multicollinearity, and can make it more feasible create visualizations. In practice, Dimensionality Reduction appears in two forms: feature elimination (removal of features), or feature extraction(some sort of combination of features). Principal Component Analysis is a dimensionality reduction method that is simple to implement in python.
<br>
<br>
The overall process of PCA is:

1. Summarizes how each of the features relate to one another (Linear Combinations)
2. Looks at the importance of how each feature relates to one another (Creates new "principal components" that describe the maximum possible amount of variance in your predictors.
3. Ranks these relationships (Sorts each prinicipal component based on importance)
<br>
<br>

We thought we would include this in our workflow because it might be useful if we ended up using a much larger corpus to train than the one we are currently using. It is also useful to compare the PCA transformed models to the models that were not PCA transformed.

***
### Model Building
#### Cross Validation
It's important to understand which metric to optimize on. Remember that our goal is to find tweets that are alert worthy for tsunami's. A tsunami is a dangerous event that we want as many people to know about as possible. We believe that it is better to predict that there is a dangerous event when there is not than to predict that nothing is wrong but there is actually a tsunami. In this case, we will aim to optimize on f-1 score as it takes into account both precision and recall.

#### Model Evaluation
We compared the following models:
- Logistic Regression
- Decision Tree Classifier
- Bagging Classifier
- Random Forest Classifier
- AdaBoost Classifier
<br>
<br>

We also compared PCA transformed data to non PCA transformed data.

<br>
<br>
#### Final Model:
We chose a model that went through the following workflow:
1. Custom Lemmatization
2. CountVectorization
3. Corrected for imbalanced classes using SMOTE and a k value = 4
4. AdaBoost Classifier with standard hyperparameters
The training f1_score was $0.93$
<br>
The testing f1_score was $0.84$.
<br>
This model showed the least amount of overfitting and has a recall score of $0.97$.
<br><br>
The notebook used for preprocessing and model selection can be found __[here](https://git.generalassemb.ly/edithih/Harnessing-Machine-Learning-for-Natural-Disaster-Alert-Systems/blob/master/Notebooks/Final%20Model%20Selection%20Workflow%20and%20Predictions.ipynb)__
***
### Conclusions
In this project we were able to create a machine learning model that was trained on labeled tweets about whether or not a tweet would be classified as 'alert worthy'. Additionally, we created a function that preprocesses incoming tweets, uses the model to make predictions and output an alert if there are a certain number of 'alert worthy' tweets in the most recent input. This machine learning model could be integrated into a web app that takes in a continuous stream of tweets.
<br>
#### How would the application work?

The application’s workflow, as it stands, would look as follows:
<br><br>
Tweet Gathered using Twitter’s API  Tweet Passed through Pre-processing  Tweet Passed through Classification Model  Return Classification as “alert-worthy” or “non-alert-worthy”  If “alert-worthy”: Send Alert  If “non-alert-worthy”: Do Nothing.
<br><br>
#### Shortcomings

With more time, we could:
1.	Gather/Label more tweets to build our model on.
2.	Build models/applications dedicated to other Natural Disasters, like tornados, earthquakes, mass shootings, etc.
3.	Build models/applications that work with other languages, like Spanish, Chinese, etc., so that the model can use tweets in different languages.
4.	Improve our model!  It is currently optimizing Recall (because we would rather a False Positive over a False Negative); with this said, though, we would like to further reduce the possibility of a False Negative and a False Positive.
5.	Set determining factors with regard to labeling a tweet as “alert-worthy” or “non-alert-worthy”
<br><br>
#### Recommendations

1.	Begin using Twitter’s API ASAP to record tweets containing #tsunami; these tweets can be saved to add even more data to train on.
2.	Search through the web-scraped tweets for more alert-worthy tweets to balance the classes more.
3. Invest time into classifying tweets to have a larger corpus to build a model from.
