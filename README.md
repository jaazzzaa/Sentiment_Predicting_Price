# Predicting Stock Price Movement with and without Sentiments - Classification Problem

## Introduction
This project explores the relations and patterns between stock price movements and sentiment expressed in tweets about Microsoft between January 1, 2019 and December 31, 2019. The goal is to determine whether or not sentiments can be used to predict stock price movements (up, down or no change). Also, the tweets dataset will be labelled using sentiment analysis.

Original datasets were collected from Kaggle which can also be found in the Datasets folder. The Datasets folder also contains financial vocabulary used to augment VADER lexicon for sentiment analysis.

## Steps

### 1] Data Collection: Import Datasets
Original datasets were collected from Kaggle: <br>
https://www.kaggle.com/omermetinn/tweets-about-the-top-companies-from-2015-to-2020<br>
https://www.kaggle.com/omermetinn/values-of-top-nasdaq-copanies-from-2010-to-2020

Financial vocabularies (created by expert linguistics) were collected from the following links (found in the Dictionary folder): <br>
https://sraf.nd.edu/loughranmcdonald-master-dictionary/ <br>
https://web.archive.org/web/20210421221737/http://www.wjh.harvard.edu/~inquirer/spreadsheet_guide.htm

In the Dataset folder, the following can be found:
-	Random sampling of Microsoft tweets and related stock market data between January 1, 2019 and December 31, 2019.
-	Final dataset to be used in models for training, testing and validation.

### 2] Data Understanding: Dataset Descriptions
-	There are 64,027 tweets and 365 records of stock market data in the aforementioned timeframe. There are no duplicate records but 341 missing values in the ‘writer’ column.

### 3] Data Preparation: Cleaning Datasets
- This included cleaning the tweets extensively in preparation for sentiment analysis.
- It also involved dealing with duplicate values, converting data types, etc.

### 4] Exploratory Data Analysis (EDA)
-	Descriptive statistics, correlation matrices, box plots, word clouds, histograms, etc. were assessed. It was found that some variables that have high correlations where one of them can be removed as it would be redundant to avoid running into a multicollinearity problem. 
-	The following features were added: ‘prev_close’, ‘return’, ‘price_change’, ‘price_class’, ‘engagement’, ‘len_modify_body’, ‘comp_clean’, ‘pos_clean’, ‘neg_clean’, ‘neu_clean’ and ‘sent_class_clean’.

### 5] Text Classification: Sentiment Analysis and Price Trend Classification
-	Since the sentiments were not determined in the original tweets dataset, sentiment analysis was conducted to label the tweets into three classes: negative (-1), neutral (0) and positive (1). VADER sentiment analysis tool was used to calculate the compound scores and used a ±0.5 threshold for class labelling (standard practice threshold).
- To improve the class balance for tweet sentiment, the VADER lexicon was augmented by incorporated two other vocabularies there were created by expert linguistics as well as words from my own experience.
-	The target variable, ‘price_class’, was created in predicting stock price movement. There are 3 classes: up (1), down (-1) and no change (0). ±0.5 threshold was also used to label the price trend.
-	A quantitative class imbalance measure was used to statistically assess the distribution of relevant and positivity. It was found that both the ‘price_class’ and ‘sent_class_clean’ were close to 0 (perfectly balanced data). We will proceed with the dataset without dealing with imbalanced dataset as both classifications are close to 0. 

### 6 & 7] Feature Importance, Data Modelling, Validation and Evaluation
-	Before training any models, feature importance methods were performed (Correlation, ANOVA and RFE). In both methods, it was found that the top ranking features were related to the stock market data rather than sentiments. For that reason, sentiment related features were kept for the purpose of this project. However, the top ranking stock market features were taken to predict stock price movement without sentiments.
-	Correlation was used as we want to remove features that have very high correlation values to reduce biases.
- Analysis of Variance (ANOVA) was used as we are predicting a categorical feature with numerical features.
- Recursive Feature Elimination (RFE) was also used for comparison. The algorithms used for this method was logistic regression and random forest. Cross validation was applied.
-	Then various models will be trained (such as Decision Tree Classifier, Random Forest Classifier, Support Vector Machine and XGBoost) to predict stock price trend based on sentiments only as well as just the stock market data (based on the feature importance).
-	Hyperparameter tuning is performed on these models to find the optimal set of combinations to run them for potentially better performances.
-	5-fold cross validation is used when training the models to evaluate how well they generalize.
-	The models are evaluated using confusion matrix, accuracy, precision, recall, f1-score and auc score (including roc curves).

### 8] Results
- The high level objective of this project was to determine the relationship between stock price of Microsoft and related tweets on a given day. With the experimentations conducted in this project, the statistical relationship can be concluded as weak. It was difficult to produce a stable and reliable model for predicting stock price movement trends from just using sentiments. Running the eight different models from using four different algorithms resulted in no more than 42% average 5-fold accuracy. Moreover, the models could have performed better if the sentiments were predetermined by experts manually rather than preprocessed the way they were to significantly reduce any errors and noises.
- Using only sentiments to predict stock price movement trends is analogous to flipping a coin since the AUC score is close to 50%. It is not any better than predicting using stock market features such as volume. In addition, predicting stock price movements with just volume resulted in very high accuracy and cross-validation (92% and 91% respectively) across most models. Since these values are very high, there could have been issues with the data itself since pipeline was used to avoid data leakage when running the models. Also, if more time was allocated to work on this project, additional fundamental and technical indicators would have been constructed to determine further effects of predicting without sentiments. The reason is that it would have been more beneficial to go through feature selection when there are more features to choose from. Three methods of feature selection were combined to ensure that the research question could be answered because all of the sentiment features ranked very low in feature importance.
- In addition, there were none to small performance improvements when undergoing randomized grid search to find optimal values of the models. It appeared that it was not worthwhile going through this step because the results were very minor.
- In conclusion, the impact of Twitter sentiments on stock prices alone was not significant that these models can be used to predict stock price movement trends. It would be better to make trading or investment decisions that combine sentiments with other market indicators.
- Lastly, it is recommended to explore other machine learning algorithms and potentially explore the research question as a regression problem rather than a classification problem.
