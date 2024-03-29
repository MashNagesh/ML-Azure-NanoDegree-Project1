# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
This dataset contains data from a marketing campaign of a bank.The classification goal is to predict if the client will subscribe a term deposit (y).

The best performing model was a **VotingEnsemble** model with a Weighted AUC score of **0.9157**

## Scikit-learn Pipeline
The pipeline contains the following components
  1. Compute Cluster
  2. Parameter Sampler
  3. Stopping Policy
  4. Estimator - points to the training script which Loads data,Cleans data ,Splits data into train and test, Uses Logistic Regression on train and provides Accuracy                 scores
  5. HyperDriveConfiguration  - which takes Compute,Estimator,Policy and Parameters as input along with the Primary Metric criterion

**What are the benefits of the parameter sampler you chose?**
The parameter sampler chosen for this project is <b> Random Parameter Sampler </b>.
Random Parameters are chosen to arrive at a quicker solution which might also be closest to the optimal solution as opposed to Grid search where it might not be possible to arrive at an optimal solution if those parameters are not part of the sampling grid which renders grid search as an inefficient method as compared to Random Parameter Sampling.

**What are the benefits of the early stopping policy you chose?**
The Stopping policy chosen for this project is **Median Stopping Policy**.The policy terminates runs whose average performances are below the median performance.
This is conservative policy but provides a lot of savings in terms of time and cost without compromising heavily on the best performance.

## AutoML
The AutoML model will search on a series of models and check for the best primary metric in our case  -Accuracy.

**Architecture:**
The Parameters chosen are  - 
* Early stopping - True. Early stopping is enabled after 20 iterations and if there are no improvements to the score.Most useful when timeout is not mentioned.
Timeout of 30 minutes - default is 6 days.Hence advisable to mention to avoid compute costs.
* Cross Validation - n_cross_validations = 5.Five folds for cross-validation are defined. Hence, five different trainings, each training using 4/5 of the data, and each validation using 1/5 of the data with a different holdout fold each time.As a result, metrics are calculated with the average of the five validation metrics.
* Compute Instance  - Using the Local instance to pass the Dataframe as is
* Primary Metric - Accuracy. The metric to be used during model training for optimization

## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**

HyperDrive Run Accuracy : **0.9101** 

AutoML run Accuracy :     **9.9157** with a Weighted AUC of **0.947** 

Architecture  for the Hyperdrive Run
Parameter Sampler --> Estimator with Logistic Regression -- >Median Stopping Policy

Architecture for the AutoML Run
The best performing AutoML run was an Ensemble model using the weighted scores across XGBoost,Random Forest,Logistic Regression and Light GBM.

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**

**Areas of Improvement:**
1. DataSet provided is imbalanced which will lead to a falsely perceived positive effect of a model's accuracy because the input data has bias towards one class


Suggested improvement:

Using **Weight** to denote a class more or less "important".

Evaluation metric **Weighted AUC** will be a better metric to evaluate the model as opposed to accuracy since the contribution is calculated based on the relative size of samples in that class.

  2. Using the Compute Cluster

For the current project the Local instance was used for running the AutoML model.However using a powerful compute cluster for the model would yield faster results in quick time.

## Proof of cluster clean up
Cluster deletion performed in the notebook
