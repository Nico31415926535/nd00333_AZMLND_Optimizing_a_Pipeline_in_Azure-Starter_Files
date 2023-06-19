# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Useful Resources
- [ScriptRunConfig Class](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py)
- [Configure and submit training runs](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-set-up-training-targets)
- [HyperDriveConfig Class](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriveconfig?view=azure-ml-py)
- [How to tune hyperparamters](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-tune-hyperparameters)


## Summary
**In 1-2 sentences, explain the problem statement: e.g "This dataset contains data about... we seek to predict..."**

This dataset contains data about different customers to classify a binary decision yes or no. This decision and the data could presumably be used for a campaign to send marketing materials on different investment products, customer churn prevention or something simlar.

**In 1-2 sentences, explain the solution: e.g. "The best performing model was a ..."**

The best performing Logistic regression optimized with hyperdrive had a regulization strength (C) of 367.4868 and had an max iteration of 75. The achieved accuracy is 91.15%.

## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**

First of all the data is loaded from the provided URL and then preprocessed.
During the preprocessing most categorical features are being encoded into numerical values (such as month and weekday)
or into a binary flag (marital, default, housing, loan, poutcome). The features jobs, contact, and education are one-hot-encoded. The target is encoded as binary flag.
The preprocessed data is further splitted into a training and a test set. The training set is used to train the Logistic model with the provided hyperparameter and is evaluated in regard of accuracy against the test set.
The classification algorithm itself is the LogisticRegression, which is a rather simple -- but still effective -- classification algorithm. It uses the features to predict the target as one would observe it with a classical regression. However to classify the data the regression function is inserted into an sigmoid function to get a more clear dinstinction.
To find the best hyperparameter random samples from the hyperparameter space are taken with help of the random search algorithm.

**What are the benefits of the parameter sampler you chose?**
I have choosen the RandomParameterSampler as it gives a faster sample of accuracy from the hyperparameter space and -- most of the times -- achieves a similar accuracy as a more elaborate and tedious grid search.
However, for a more complicated scenario (more hyperparameter dimensions) I would have choosen the Bayesian sampling instead because it is even more efficient than the RadonParameterSampler.

**What are the benefits of the early stopping policy you chose?**

There will be somewhat a limit in accuracy defined by the task. If this accuracy is met it does not make much sense to try to further improve the accuracy. Most of the times trying to force an improvement in accuracy results in overfitting the algorithm.

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**
The best performing model was the "VotingEnsemble" from the optimization with AutoML. It has an accuaracy of 91.76%.

## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**
The performance are almost similar. However, the AutoML algorithm was able to achieve a slightly better result. Taking a look at the VotingEnsemble one can see that besides different boosting algorithms such as XGBoost and LightGBM the Logistic regression is also part of the voting ensemble. 
It is to be expected that an ensemble of different weighted algorithms perform better than one alone. Why? Because, every algorithm has its particular weaknesses and with an voting ensemble it is possible to accomocated for that by trying to 
fill the deficiencies of one algorithm with the competence of another and vice versa for other cases.

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**
One could try to get more data for training and try to improve the data preprocessing. I am not quite sure, whether it e.g. wouldn't be a better idea to also one-hot encode the date feature or even sine/cosine transform it.