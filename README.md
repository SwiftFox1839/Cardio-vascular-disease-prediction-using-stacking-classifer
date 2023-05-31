# **HealthCoder 2023 - Cardiovascular Disease Prediction**

> A web app that predicts the probability of heart disease based on user input, offering a convenient and accessible tool for assessing individual risk.

![cvd home](https://github.com/somyasubham9/CVD_Prediction/assets/77459972/dcaea43a-ca64-4a5c-9062-8f2a23d49aa4)

## Abstract
Heart disease is a leading cause of mortality worldwide, and early detection is crucial for effective treatment and prevention. In recent years, machine learning (ML) techniques have shown promise in predicting heart disease risk. However, the performance of individual ML models can vary significantly, and it is challenging to identify the best model for a given dataset. In this study, we propose a heart disease prediction model based on a stacking ensemble approach that combines the predictions of ten different ML models.
The stacking ensemble is designed in a two-layer architecture. In the first layer, the ten base models generate predictions based on the input features. These predictions are then used as inputs to a meta-model, which is trained to learn the optimal combination of base model predictions. The meta-model's objective is to effectively weigh and combine the predictions from the base models, maximizing the overall predictive metric used in study.

## Data
This dataset was created by combining different datasets already available independently but not combined before. In this dataset, 5 heart datasets are combined over 11 common features which makes it the largest heart disease dataset available so far for research purposes. The five datasets used for its curation are:

- Cleveland: 303 observations
- Hungarian: 294 observations
- Switzerland: 123 observations
- Long Beach VA: 200 observations
- Stalog (Heart) Data Set: 270 observations


Total: 1190 observations <br>
Duplicated: 272 observations<br>
`Final dataset: 918 observations`

## Data Visualization
In this study we have used Pandas Profiling on the dataset to provide an in-depth understanding of the data. The library generates descriptive statistics, including measures of central tendency, dispersion, and quantiles, for each variable. It also identifies missing values, data types, and unique values in the dataset.
Moreover, Pandas Profiling generates interactive visualizations such as histograms, bar plots, scatter plots, and correlation matrices. These visualizations allow data scientists to explore the relationships between different variables and identify potential patterns or trends related to heart disease risk factors.
The visualizations produced by Pandas Profiling help in understanding the distribution of variables, identifying outliers, and identifying potential correlations between variables. These insights can inform feature selection, data preprocessing, and model development stages of the heart disease prediction project. <br>
Here's a snapshot of one of the visulization generated by pandas profiling <br>

<img width="1171" alt="Screenshot 2023-05-30 at 10 00 21 PM" src="https://github.com/SouravBiswal/Cardio-vascular-disease-prediction-using-stacking-classifer/assets/69891272/455bcb56-f736-41ba-b865-905a3bd049e3">

<br>

`Note : You can download the pandas profile report.html on your local machine to see the visulization yourself!`



## Data Pre-processing

- Encoding the categorical features
- Scaling was performed (`Oldpeak` was _normalized_ while `Age, Cholestrol and MaxHR` were _standardized_)
- Feature selection was performed (Chi <sup> 2 </sup> test for _categorical features_ and ANOVA test for _numerical features_)
- Splitting the dataset into train and test sets

## Model Training
We have trained 10 different classifiers (base-models) and employed Grid-SearchCV with a Cross-validation of `5` folds and scored based on `AUC ROC` for finding the most optimal hyperparameters for each classifier. <br>
Models trained in order along with optimal hyperparameters :-

```python
1. SVC(C = 10, gamma = 'auto', kernel = 'linear')
2. KNeighborsClassifier(metric = 'manhattan', n_neighbors = 15, weights = 'distance')
3. MLPClassifier(activation = 'relu', alpha = 0.0001, early_stopping = False,
                                     hidden_layer_sizes = (50, 30, 20, 30, 10),
                                     learning_rate_init = 0.0001,
                                     max_iter = 500,solver = 'adam')
                                     
4. RandomForestClassifier(n_estimators = 150, max_features = 'auto', max_depth = 6, criterion = 'entropy')
5. XGBClassifier(n_estimators = 250, subsample = 0.8, min_child_weight = 1, max_depth = 3, gamma = 1.5, learning_rate = 0.02)
6. CatBoostClassifier(n_estimators = 300, depth = 6, l2_leaf_reg = 1, learning_rate = 0.03, verbose = False)
7. AdaBoostClassifier(n_estimators = 300, learning_rate = 0.03, algorithm = 'SAMME.R')
8. DecisionTreeClassifier(criterion='entropy', max_depth=5, min_samples_leaf=10,random_state=42)
9. GaussianNB(var_smoothing = 0.0015199110829529332)
10. LogisticRegression(C=0.08858667904100823, max_iter=7,solver='saga',verbose = 0)
```
Thereafter, the probablities values for each classes were fed as input to the meta-model
 ```python
 final_estimator = GaussianNB(var_smoothing = 0.0015199110829529332)
 model = StackingClassifier(estimators=level0, final_estimator, stack_method = 'predict_proba', passthrough = True)
 ```
 > Surprisingly a simple model like Gaussian Naive Bayes worked very well as the final estimator, we believe it can be tuned further!
 
## Model Performance
Model performance was evaluated using `AUC ROC` as the scoring metric and RepeatedStratifiedKFold Cross-validation scheme
`n_splits = 5` and `n_repeats = 3` was followed to ensure each fold of dataset has the same proportion of observations.

|      Model            |  AUC ROC (%)|
|-----------------------|-------------|
|        SVC            |  91.1 ± 2.5 |
|    KNeighbors         |  92.3 ± 2.3 |
|    MLPClassifier      |  92.3 ± 2.3 |
|    RandomForest       |  93.1 ± 2.1 |
|    XGBClassifier      |  93.6 ± 2.0 |
|    CatBoostClassifier |  93.2 ± 1.9 |
|    AdaBoostClassifier |  92.6 ± 2.3 |
|    DecisionTree       |  90.0 ± 2.4 |
|    GaussianNB         |  91.3 ± 2.5 |
|    LogisticRegression |  91.1 ± 2.5 |
|    Stacked Ensemble   |  93.3 ± 2.0 |
 

## Technology Stack:

ML: sklearn, pandas, numpy, matplotlib, mlxtend

Client: React,CSS,React-Router

Server: Python

## Further Study
The stacked ensemble's final-estimator can be fined tuned to give even better results. We've the included the code for the same in the notebook.
However, `Note : it's computationally super expensive and time taken to complete the execution can go from several hours to several days depending on the model used as final estimator and the parameter grid`

## Demo
Video Link: https://drive.google.com/file/d/1Oh3mtgwJ8AfMGnt0_T-KSfUpU3TQeUgY/view?usp=sharing

## Setup
To run this project, install it locally using npm:
Clone the Project
```
git clone  https://github.com/SouravBiswal/Cardio-vascular-disease-prediction-using-stacking-classifer.git
npm i
```
Client Side
```
$ cd client
$ npm install
$ npm start
```
Server Side
```
$ npm start
```
## Authors
- [Suman Sourav Biswal](https://github.com/SouravBiswal)
- [Swagat Satprem Jena](https://github.com/Swagat-Satprem-Jena)
- [Somya Subham Dash](https://github.com/somyasubham9)
