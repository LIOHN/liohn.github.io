---
layout: post
title: "Titanic Survival with Logistic Regression"
author: "Daniel E"
categories: projects
tags: [Python3, jupyter-notebook, scikitlearn, pandas, numpy, gunicorn, pytest, tox, flask, github, git, gemfury, circleci, CICD, MLOps, DevOps, heroku, docker, REST, OOP, logistic-regression, reproducibility, versioning, modularity, API]
image: titanic-regression.png
---

# Project Summary
The problem to be solved here was predicting the probability of a passenger aboard the RMS Titanic surviving, given their ticket data (age, gender, fare, cabin, class, title). The goal was to see if, using a machine learning algorithm/statistical method, we could extract some results that can be verified against known truths about society at the time, like linking increased probability of drowning for lower class passengers to discrimination against people of lower societal standing.

Additionally, this algorithm was to be deployed to a cloud app where others can input their own ticket data and receive a prediction.

The way predictions were delivered was by training a model with the data and persisting it offline, then loaded in a web app that can give real time predictions which would be POSTed to a client via REST. More shortly: "Train by batch, predict on the fly, serve via REST API".
This factors making this architecture a viable choice were ease of management of this system and low to average latency of predictions.

While this project featured the full development lifecycle of a learning model, feature engineering, creating the model pipeline and automating testing and deployment were the focus of this project.

The dataset was quite small and relatively balanced, sizing up to 2200 rows of categorical and discrete data, with some empty fields. It is freely available [online](https://www.openml.org/data/get_csv/16826755/phpMYEkMl). 

*This project was completed 26/12/2020.*

The code can be found on [my GitHub page](https://github.com/LIOHN/titanic-survival-regression).

## Keywords
Python3, jupyter-notebook, scikitlearn, pandas, numpy, gunicorn, pytest, tox, flask, github, git, gemfury, circleci, CICD, MLOps, DevOps, heroku, docker, REST, OOP, logistic-regression, reproducibility, versioning, modularity, API

# Delivery

## Research Phase
This project began as a Jupyter notebook where, upon exploring the data and splitting it into train and test sets, I devised data preprocessing methods that are suitable for the problem and algorithm to be used.

The feature engineering steps included dealing with missing data, removing rare labels, performing one hot encoding and normalising the data. These steps were key in ensuring the model can learn the data effectively. As part of these preprocessors, additional features/columns are created for each feature that was altered, in order to keep track of changes, for reproducibility purposes.

Using the pandas method `pandas.DataFrame.fillna`, missing values in numerical columns were filled with the median of the values in the column. When it came to categorical variables, missing data was filled with the string "Missing".
With a simple function making use of `pandas.DataFrame.groupby` I removed rare labels (occuring in less than 5% of passengers) so as to increase the robustness of the future learning model.
Categorical variables had to be one-hot encoded to a numerical form. Using `pandas.get_dummies`, encoding was done into k-1 binary variables so as to reduce redundancies, and then the columns containing categorical variables were dropped.
Finally, with `sklearn.preprocessing.StandardScaler` all features were standardised to resemble a normal distribution.

Once the features were engineered, the data could be passed through to the learning algorithm. This was chosen to be a logistic regression classifier (`sklearn.linear_model.LogisticRegression`) due to the nature of the prediction variable (categorical; probability of survival between 0-1)

## Development phase
This is the part where the project starts to take shape with two main parts:
* creating the pipeline application
* serving the model via a REST API
Both components have their own versioning, tests, requirements and config files. Further in line with best practices, even when I wasn't sure how to carry out a certain operation, I created placeholder files (e.g. validate_inputs), and integrated those steps into the pipeline so that later development can integrate seamlessly. This also allows for extensibility.

The ML pipeline application was coded OOP style with the previous Jupyter scripts translated into classes. They were coded to allow for integration with a  `sklearn.pipeline.Pipeline` object that links the transformers with algorithms. Within the pipeline object, the transformers progressively process the data until it reaches the logistic regression classifier, where the model is fit to the data.
```
from regression_model.processing import preprocessors as pp
titanic_pipe = Pipeline([
	(pp.ExtractFirstLetter(variables=config.CABIN)),
	(pp.CategoricalImputer(variables=config.CATEGORICAL_VARS)),
	(pp.MissingIndicator(variables=config.NUMERICAL_VARS)),
	(pp.NumericalImputer(variables=config.NUMERICAL_VARS)),
	(pp.RareLabelCategoricalEncoder(tol=0.05, variables=config.CATEGORICAL_VARS)), 
	(pp.CategoricalEncoder(variables=config.CATEGORICAL_VARS)),
	(StandardScaler()),
	(LogisticRegression(C=0.0005, random_state=0))
])
```

The prediction step was defined as: load existing pipeline (location defined in config file), convert input dataset to `pandas.DataFrame`, validate_inputs (with predefined checks to ensure data is processable) and run `sklearn.pipeline.Pipeline.predict` on the dataset.
This step would get called by the ML API, and is tested both with the model and with the ML API.

Tests were coded with pytest for both parts. They were generally basic sanity checks like: Does the model return a sensble accuracy score, and does the model return the expected prediction size? And for the ML API again, sanity checks like: Does the flask app return status code 200 for a health request, does the app return the correct version, and does the app return the expected prediction size.
Using tox, I put all the testing into one place with one tox.ini script that I could easily run at any point in development to make sure there were no syntax errors and everything works as intended on the surface.

Thanks to Python's logging library I was also able to define a flexible event logging system that would output updates at various points in the process like:
```
_logger.info(f"saved pipeline: {save_file_name}")

_logger.info(
	f"Making predictions with model version: {_version} "
	f"Inputs: {validated_data} "
	f"Predictions: {results}"
)
```

Finally the model was packaged to make it portable for the steps to follow. For this I used tox again, this time with a `setuptools.setup` script that would train the model and then package it into distributables. This way the regression model can be installed on a different machine using pip.
	
## Production phase
This phase dealt mainly with developing the CICD pipeline.
CircleCI allowed me to define an automated workflow in a YAML configuration file  and check out deployment status right in my GitHub repository.
This config file defined the order in which the deployment steps are to run and the dependencies between them.
The CircleCI pipeline runs my defined jobs, described below, inside a predefined Docker Image (`circleci/python:3.7.2`), aka the primary container. My project requires a few preliminary steps on this Docker Image, namely creating a virtual environment and fetching the titanic dataset from the web. With these prerequisites in mind, the jobs are:
* test_regression_model: This installs the specified libraries from the requirements file in the packaged model, trains the model and checks if the model passes predefined tests (described earlier)
* test_ML_API: This installs the specific libraries in from the requirements file, and runs tests (described earlier)
* train_and_upload_regression_model: Runs the test_regression_model job with the additional step of publishing the model to **Gemfury** private cloud repository  
and either
* deploy_to_heroku: Deploys my app with the command `git push`, the Heroku app specified by unique API_KEY and APP_NAME one can obtain from Heroku
or 
* build_and_push_to_heroku_docker: Containerises the packaged model from Gemfury (with instructions in a separate Dockerfile) and builds and pushes the Docker Image to Heroku which then releases (deploys) the app. 

Should any tests fail and deployment to production is halted.

# Conclusion
Titanic regression taught me how machine learning models might be deployed in a commercial setting. For the first time, I worked on a learning model at all stages in its lifecycle: research, development, production. 
In Titanic Regression I was exposed to many technologies and methodologies that I had not encountered before. Automated DevOps, or CICD was completely new to me. The feature engineering process varies from project to project, so the data science practice was invaluable for me. I applied some existing knowledge on Docker and REST API and explored with cloud services like Heroku and Gemfury.

This worked out great for me, as my goal for a while has been becoming a well rounded machine learning engineer.

While this project was mainly independent work, a community of like-minded students on this Udemy course were available to help when I got stuck, smoothing the "learning by doing" process.

*This project was completed 26/12/2020.*

The code can be found on [my GitHub page](https://github.com/LIOHN/titanic-survival-regression).


