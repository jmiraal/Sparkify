# Udacity Project: Data Scientist Capstone

## Nanodegree: Data Scientist

## Title: Spark Project: Sparkify

### Table of Contents

<li><a href="#project_motivation">Project Motivation</a></li>
<li><a href="#file_descriptions">File Descriptions</a></li>
<li><a href="#working_proccess">Working Proccess</a></li>
    <ul>
    <li><a href="#etl">ETL</a></li>
    <li><a href="#churn">Definition of the Churn</a></li>
    <li><a href="#eda">Exploratory Data Analysis</a></li>
    <li><a href="#modeling">Modeling</a></li>
    </ul>
<li><a href="#conclusion">Conclusion</a></li>



<a id='project_motivation'></a>
## Project Motivation

The objective of this project is to try to make predictions about the churn of a company called Sparkify. This simulated company, like Spotify offers a service of music online. We have a file of 128MB that contains information about the users and their activity in the service that is part of a bigger dataset of data of 12GB.

Especifically we are interested in making predictions about whether a customer is going to leave the service or not. To do this we have tested some Machine Learning Models on a pySpark environment.

The project was developed as the final project for the Udacity Data Scientist Nanodegree. Its aim was to improve the habilities working with pySpark. A blog with the summary of this work was posted in this [link](https://medium.com/@jesus.mira74/sparkify-analysis-of-churn-predictions-using-pyspark-f6a6a12530ae).


<a id='file_descriptions'></a>
## File Descriptions

We had two files to work with:

**mini_sparkify_event_data.json:** A limited file of 128MB with 286500 registers. This is the file that we have used in this project since it was developed in the workspace that Udacity provide to the students. It has been also executed in the distributed IBM Watson Studio platform (Not included in this github repo).

**sparkify_event_data.json:** A complete file with 12GB. This file was available in the case we wanted to run the notebook in a distributed cloud platform like AWS. This has not been done by the moment (Not included in this github repo)

**Sparkify.ipynb:** This is the notebook with the analysis.

**/LogisticRegression:**  File with the resulted Logistic Regression Model.

**/LogisticRegressionWeighted:**  File with the resulted Logistic Regression Weighted Model.

**/LinearSVC:**  File with the resulted Linear SVC Model.

**/RandomForestClassifier:**  File with the resulted Random Forest Classifier Model.

**/GBTClassifier:**  File with the resulted GBT Classifier Model.

**/DecisionTreeClassifier:**  File with the resulted Decision Tree Classifier Model.


<a id='working_proccess'></a>
## Working Proccess

<a id='etl'></a>
### ETL

Load the data to a spark dataframe, first analysis and clean the necessary rows and columns.

The data contained in the file is a log of events of the users. It is registered all the interactions of the users with the service. The differente options or pages that we can find on the log are these ones:

    - Cancel                         - Cancellation Confirmation
    - Submit Downgrade               - Submit Upgrade
    - Error                          - Save Settings
    - About                          - Upgrade
    - Help                           - Settings
    - Downgrade                      - Thumbs Down
    - Logout                         - Roll Advert
    - Add Friend                     - Add to Playlist
    - Home                           - Thumbs Up
    - NextSong
    
In the inputs is also saved information about the songs (the lenght, the name, the artist...), about the users (the name, the gender, level, location, register date ...)

<a id='churn'></a>
### Definition of the Churn

In our case we have based the definition of the Churn in the messages of `Cancellation Confirmation`. I a user has completed a cancellation we mark him with a churn_label class 1, otherwise we assign him the churn_label class 0.

<a id='eda'></a>
### Exploratory Data Analysis

We have made a deeper analysis to define posible features to be used in the manchine learning models. These are the features we have used:

* **Numerical Features**

    - number of downgrades by user (Integer)
    - days of activity (Integer)
    - proportion of each type of page (Float between 0 and 1)
    - mean time in minutes per session (Float)
    - mean number of songs per session (Float)
    - mean number of sessions per week (Float)
    - mean time of use in minutes per week (Float)
       
    - mean time per day in the last week (Float)
    - mean time per day in the last month (Float)
    - proportion between the time of use per day in the last week compared
      with the time of use per day in the last month (Float between 0 and 1)
         
    - number of songs in the last week (Float)
    - number of songs in the last month (Float)
    - proportion of the number of songs in the last week compared
      with the number of songs in the last month. (Float between 0 and 1)

    - mean number of different artists per week (Float)
    - total number of different artists (Integer)


* **Categorical Features**

    - gender (male,female)
    - level (free, paid)
    - platform (Windows, Mac, iPad, Linux, etc)
 
<a id='modeling'></a> 
### Modeling  

We have tried 6 different options of Machine Learning Models:

   - Logistic Regression
   - Weighted Logistic Regression
   - Random Forest Classifier
   - Decision Tree Classifier
   - GBT Classifier
   - Weighted Linear SVC
   
   
We have mainly focused our attention into improve the Logistic Regression model. We also have tried to detect as many cancellations or churned users as possible, even if this suppose to increase the number of false positives. We think that is preferable to fail some 0 values that to miss some 1 value in the churn, that was why we have decide to use the weighted version given that the dataset was highly imbalanced.
To avoid collinearities between features we have also applied PCA to Logistic Regression and Linear SVC.

<a id='conclusion'></a> 
## Conclusion  
As a conclusion this are the values for `F1` and `Accuracy` obtained for all the models we have tested. We have extracted the general performance and the values for the class 0 (not churned) and 1 (churned) because we are especially interested in knowing the scores for the churn value 1:

     
| Model                      | F1_Total | F1_0     | F1_1     | Accuracy_Total | Accuracy_0 | Accuracy_1 |
|----------------------------|----------|----------|----------|----------------|------------|------------|
| LogisticRegressionWeighted | 0.834600 | 0.907216 | 0.896552 | 0.826087       | 0.830189   | 0.8125     |
| WeightedLinearSVC          | 0.821842 | 0.895833 | 0.896552 | 0.811594       | 0.811321   | 0.8125     |
| LogisticRegression         | 0.803204 | 0.980769 | 0.545455 | 0.826087       | 0.962264   | 0.3750     |
| RandomForestClassifier     | 0.791053 | 0.970874 | 0.545455 | 0.811594       | 0.943396   | 0.3750     |
| DecisionTreeClassifier     | 0.804442 | 0.950495 | 0.666667 | 0.811594       | 0.905660   | 0.5000     |
| GBTClassifier              | 0.739130 | 0.907216 | 0.608696 | 0.739130       | 0.830189   | 0.4375     |


In this dataset the best model is `Weighted Logistic Regression` but this decision can change with a bigger dataset. `Weighted Linear SVC` has had almost the same results so it can be also a good option. The next model with better performance was `Decision Tree Classifier`.


 













