# Operationalizing Machine Learning

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we configured a cloud-based machine learning production model, deployed it, and consumed it. Also we created, published, and consumed a pipeline.

## Summary
This dataset contains data about direct marketing campaigns of a banking institution. The marketing campaigns were based on phone calls. So we seek to predict if the product (bank term deposit) would be ('yes') or not ('no') subscribed.

First I executed an AutoML experiment using the Azure ML platform for training several kinds of models such as LightGBM, XGBoost, Logistic Regression, VotingEnsemble, among others algorithms. After we got the best model, I deployed it and I got the REST endpoint. I enabled logging through Application Insights service. Also we covered the Swagger documentation in order to get information of the REST endpoint. Then we saw how to consume the endpoint using the HTTP API and the authentication. Finally I created and published a pipeline using the Python SDK and I got the REST endpoint of the pipeline and with that we consumed the endpoint in order to execute the pipeline. All these steps we can see in the following architecture:

![architecture](/image/azmle_pj2_00.jpg)

## Automated ML Experiment

We used the AutoML tool in order to train several models and get the best one with high accuracy. For this experiment I used the Bank Marketing dataset provided. I configured a compute cluster and setted up parameters like the classification task, the exit criterion to 1 hour and reduce the Concurrency to 5 wich it is a value lower than the number of nodes of the computer cluster. So after the experiment finished running we got a VotingEnsemble model as the best one. 

![dataset](/image/azmle_pj2_01.jpg)
![AutoML](/image/azmle_pj2_02.jpg)

One advantege of the AutoML is that it also gives an explanation of the model. We can see the top importance features, in this case for example, the 'duration' variable turned out as the most important in order to predict if the product would be subscribed or not.

![AutoML](/image/azmle_pj2_04.jpg)

## Model Deployment

After the experiment run completed I deployed the best model. For this I had to configure the computer type as Azure Computer Instance (ACI) and enable the authentication.

![ModelDeployment](/image/azmle_pj2_07.jpg)
![ModelDeployment](/image/azmle_pj2_08.jpg)

We can see the Application Insights is not enabled by default. So I had to run a python script logs.py to enable it. After executed it we can see now the Application Insights is enabled and we can retrieve logs

![ModelDeployment](/image/azmle_pj2_09.jpg)
![ModelDeployment](/image/azmle_pj2_10.jpg)
![ModelDeployment](/image/azmle_pj2_11.jpg)
![ModelDeployment](/image/azmle_pj2_12.jpg)

## Swagger Documentation

In the information page of the model deployed, we can see it also give us an Swagger URI. So we can consume the deployed model using Swagger. I downloaded the Swagger JSON file from the URI and then execute the swagger bash script to download the latest Swagger container and run it on port 9000. Also I executed the python script serve.py in order to interact with documentation of the HTTP API from the model. We can inspect the POST method and see the data that the endpoint expects to receive.

![Swagger](/image/azmle_pj2_13.jpg)
![Swagger](/image/azmle_pj2_14.jpg)

## Consume Model Endpoint

We can consumed the model endpoint using the HTTP API. So I used the provided python script endpoint.py to interact with the trained model. I had to edit the script with the endpoint information such as the scoring URI, the primary Key for authentication and also specify some input data. Once executed, it returns the result of the prediction as we can see in the image.

![Consume](/image/azmle_pj2_15.jpg)
![Consume](/image/azmle_pj2_16.jpg)

Also I used the Apache bench tool in order to benchmark the endpoint. For this, I executed the provided bash script and it returned some metrics like the response time and the number of failed requests. We can see the model takes approximately 194 ms per request and it didn't have failed requests.

![Consume](/image/azmle_pj2_17.jpg)

## Create, Publish and Consume a Pipeline

For this part we used the Jupyter Notebook provided. In the notebook, firts we initialized the workspace, we specified the Azure ML experiment, attached the computed cluster, we loaded the Bank marketing dataset, we configured the AutoML using the AutoMLConfig class. We also used th AutoMLStep class to specify the steps of the pipeline. Then we created the pipeline and submitted the experiment. Once the experiment is completed, we can see the diagram of the pipeline in the Azure ML studio. We can see the pipeline first has the bankmarketing dataset module followed by the AutoML module.

![Pipeline](/image/azmle_pj2_24.jpg)
![Pipeline](/image/azmle_pj2_21.jpg)
![Pipeline](/image/azmle_pj2_18.jpg)

Then we published the pipeline using the publish_pipeline method. It generated the Pipeline endpoint, in this case called "Bank Marketing  Train" and in the portal we can see the REST endpoint and its status which is Active. 

![Pipeline](/image/azmle_pj2_19.jpg)
![Pipeline](/image/azmle_pj2_20.jpg)

Once we got the Pipeline endpoint we can schedule it in order to run it every 24 hours for example. For this we used the ScheduleRecurrence class from the Azure SDK. Finally we can consume the Pipeline endpoint using the HTTP via Python SDK. So at the end of the notebook we can see how to monitor the status of the pipeline run using RunDetails Widget.

![Pipeline](/image/azmle_pj2_23.jpg)

## Screencast Video

Finally I recorded a screencast that shows the entire process of the working ML application. 

[![Screencast](https://img.youtube.com/vi/ICra76yM6sc/0.jpg)](https://www.youtube.com/watch?v=ICra76yM6sc&feature=youtu.be&ab_channel=GROVERHERNANDOSUSANIBARSIPAN)
