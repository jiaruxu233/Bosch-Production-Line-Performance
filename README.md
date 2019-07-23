# DNSC 6278 Project -- [Bosch Production Line Performance](https://www.kaggle.com/c/bosch-production-line-performance/overview) 

**Group Members:** Qianying Diao, Jiaru Xu, Xinrong Chen, Shirley Zhang

![Kaggle icon](https://github.com/jiaruxu233/Big-Data-Project/blob/master/Pics/1563903920326.jpg)

## Project Introduction

### Summary

For our project, we focus on performance prediction for **[Bosch production line](https://www.kaggle.com/c/bosch-production-line-performance/overview)**. Our dataset is extremely imbalanced with thousands of features. At data preparation stage, we oversampled the data by SMOTE and did feature engineering. Then we use **Spark SQL, Spark Dataframe, Spark Machine Learning API** and XGBoost to analyze data. We tried four different binary classification models: random forest model, logistic regression model and gradient boosted tree classifier. For evaluation, we use **Matthews Correlation Coefficient** (MCC) to evaluate the model. 

![bosch icon](https://github.com/jiaruxu233/Big-Data-Project/blob/master/Pics/cover.jpg)


### Background and Introduction

Bosch is one of the world's leading manufacturing companies. In its daily routine, one significant task is to ensure that all recipes for the production of its advanced mechanical components are of superior quality and safety standards. 

In detail, company needs to closely monitor production  parts as they progress through the manufacturing processes recording data at stepwise along its assembly lines and apply advanced analytics to improve these manufacturing processes. 

Here in our project, we are challenged to predict internal failures with thousands of measurements made for each component along the production line. With our project, we hope it will help Bosch to bring quality products at lower costs to end users.


### Tools we use
* Loading Data with **Amazon S3 bucket**
* Exploratory Analysis: **Spark SQL, Spark Dataframe**
* Feature Selection and Engineering: **Spark SQL, Spark Dataframe**, UDF in Pandas, visualization with matplotlib
* Modeling: **Spark ML (Logistic Regression, Random Forest, Gradient Boosted Tree)**

### Code Files
To view our detailed codes, you are welcome to click following titles.

* [Step 1 - Exploratory Data Analysis](https://nbviewer.jupyter.org/github/jiaruxu233/Big-Data-Project/blob/master/Code/Step_1_Exploratory_Data_Analysis.ipynb)

* [Step 2 - Feature Selection and Engineering](https://nbviewer.jupyter.org/githubjiaruxu233/Big-Data-Project/blob/master/Code/Step_2_Feature_Engineering.ipynb)

* [Step 3 - Modeling and Evaluation](https://nbviewer.jupyter.org/githubjiaruxu233/Big-Data-Project/blob/master/Code/Step_3_Modeling.ipynb)

### Dataset we use

* train_numeric.csv- the training set numeric features (this file contains the 'Response' variable)
* train_categorical.csv - the training set categorical features
* train_date.csv - the training set date features

### Challenges 

Two major challenges in our project is first too many missing values and second highly imbalanced target. 	When we try to build a model with Spark ML, missing values in our dataframe will cause some error. Therefore, we need to find a way to solve it. After consideration, we set an additional parameter `handleInvalid` in both StringIndexer and VectorAssembler to handle with those missing values.

In addition, through our struggle with Spark, we also notice that although Spark   does a fantastic job with large datasets, it does not have a strong vidualization function. Therefore, when we prefer to have a look at how our dataset, we adopt the matplotlib package. Before plotting the dataset, we need to transform our Spark dataframe into a pandas dataframe. However, such transformation seems to take forever. Without parallel processing in Spark, it takes more than triple the time that Spark needs in order to read data into pandas dataframe.  

Also, in our initial trail, we download data from Kaggle, then upload unzipped data files onto a S3 bucket and finally read data from our S3 bucket. What we realize afterwards is that we could directly download data to our S3 bucket from Kaggle website which proves to take a shorter time.


## Methods
* How you cleaned, prepared the dataset with samples of intermediate data?

We prepared the dataset by oversampling method SMOTE. Meanwhile,for numeric features, we generate features for each station according to maximum and minimum values. For categorical features, we pick columns with largest variance. For date features, we drop duplicate columns.

* Tools you used for analyzing the dataset and the justification (tools, models, etc.)

According to our previous introduction, when reading data, we use **Amazon S3** service. When doing the detailed analysis, we use  **Spark SQL, Spark Dataframe**, UDF in Pandas and visualizing with matplotlib. Last for modeling, we adopt **Spark ML API (Logistic Regression, Random Forest, Gradient Boosted Tree)**.

* How did you model the dataset, what techniques did you use and why?

We run **random forest model, logistic regression model and gradient boosted tree classifier** with Spark Machine Learning API. We choose these models because on one hand they do binary classification, on another hand, they will help predict labels in our splited test dataset.  

* Did you have a hypothesis that you were trying to prove? 

We suppose that different stations will have a different impact on production failure rate. Through our exploration, we find that station 32 has the highest failure rate followed by station 38 and 24. Therefore, we guess that station 32 might be a R&D position which will go through a lot of experiments with relatively high possibility of failure.

* Did you just visualize the dataset, and if so, why?

We looked at the schema to see the feature names and feature types. We visualize failure rate at different stations to achieve some insights and how missing value distributed among three datesets. 

## Results
* What did you find and learn?

First, the data is extremely imbalanced and we find this imbalance problem will influence the accuracy of model prediction especially if we adopt traditional evaluation criteria such as accuracy. Since standard classification algorithms usually consider a balanced training set and this supposes a bias towards the majority class. In this project, we realized that imbalance is a problem we should deal with before we actually start modeling. 

Second, because of the large amount of features, we need to find, select and extract those that are indeed valuable.

Third, through our struggle with Spark, we realize that Spark is extremely great at handling large dataset especially compared with pandas. When we try to read and transform our whole dataset with pandas, it seems never end. Although it does not have a strong function over visualization, it speeds up the process when we are confronted with "Big Data" which is increasingly popular in recent days.

* How did you validate your results?

We adopt Matthews correlation coefficient (MCC) to evaluate the model performance because the prediction classification is imbalanced.

## Future work 
* What would you do differently and what follow-up work would you do?

What we would do differently is that we would open a cluster with larger instances before manipulating the dataset and modeling since we spent most of our time on waiting the results of model training. 

Besides, we would use alternative methods to deal with imbalanced, sparse data and do model tuning.


### Hope you enjoy our work! Thanks! :)
