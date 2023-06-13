 Synopsis: This course covers how to implement the various flavors of production ML systems— static, dynamic, and continuous training; static and dynamic inference; and batch and online processing. You delve into TensorFlow abstraction levels, the various options for doing distributed training, and how to write distributed training models with custom estimators.

# 01. Intro to Advanced ML on Google Cloud

Static vs Dynamic training (like physica vs fashion) In dynamic environments like fashion, we'll have to constantly re-train our models

Dynamic training can make use of 3 potential architectures (Cloud Funciton, App Enginer, DataFlow)

3 use-cases for dynamic training

1. file arrives in Cloud Storage -> Launch Cloud Func -> Start Train job on AI platform -> AI platform produces new model
2. user web request -> start AI plt tr job -> new model
3. Pub/Sub topic -> consumed by Dataflow and aggregated -> write agg data in BigQuery -> AIP job launched on new data in BQ -> new model

Serving design decision:

one of the goals: minimize latency

Static serving: pre-computed labels stored in look-up tables Dynamic serving: computes label on demand

there's a space time trade-off 
![](Pasted%20image%2020230204191816.png)
We will look at:

![](Pasted%20image%2020230613112010.png)

# 02. Architecting Production ML Systems

## 02.01 - Architecting ML Systems 
Modeling, ML, Data Science code accounts for only 5% of the code for most applications.
There is a lot of supporting infrastructure and code.

![](Pasted%20image%2020230613111900.png)

## 02.02 - Data Extraction, Analysis, and Preparation

After you define the business use case and establish the success criteria, the process of delivering an ML model to production typically involves several steps, which can be completed manually or by an automated pipeline.
![](Pasted%20image%2020230613113038.png)

The first three steps deal with data.
- Data Extraction / Ingestion (from BQ, GCS, PubSub etc.)
- Data Analysis (EDA, basic stats, feature dists and hists)
- Data Prep (transformations, featurization, encoding etc.)

In data analysis, you analyze the data you've extracted. For example, you can use exploratory data analysis, or EDA. This involves using graphics and basic sample statistics to explore your data, such as looking for outliers or anomalies, trends, and data distributions. This step helps you identify those features that can aid in increasing the predictive power of your machine learning model.

For example, here are three types of preprocessing for dates using SQL in BigQuery ML: Where we are extracting the parts of the date into different columns, year, month, day, etc., extracting the time period between the current date and columns in terms of years, months, days, etc., and extracting some specific features from the date, name of the weekday, weekend or not, holiday or not, etc.
![](Pasted%20image%2020230613113128.png)
Please note that for all non-numeric columns other than timestamp, BigQuery ML performs a one-hot encoding transformation. This transformation generates a separate feature for each unique value in the column.

## 02.03 - Model Training, Eval, and Validation

- **Training:** The next step in the workflow is choosing a model that you'll train. In model training, you implement different machine learning algorithms with the prepared data to train various ML models.
- **Eval:** Model evaluation aims to estimate the generalization accuracy of a model on future, unseen, or out-of-sample data. Model evaluation consists of a person or a group of people evaluating or assessing the model with respect to some business-relevant metric like AUC, area under the curve, or cost weighted error. If the model meets their criteria, it is moved from the assessment phase to development.
- **Validation:** After development, the model is then ready for a live experiment or real-world test of the model. In contrast to the model evaluation component, which is performed by humans, the model validation component evaluates the model against fixed thresholds and alerts engineers when things go wrong. One common test is to look at performance by slice. Let's say, for example, business stakeholders care strongly about a particular geographic market region.

## 02.04 - Trained model, prediction service, and performance monitoring

**Model Registry:** The output of model validation is a trained model that can be pushed to the model registry. The machine learning model registry is a centralized tracking system that stores linage, versioning, and related metadata for published machine learning models. 
A registry may capture governance data required for auditing purposes, such as who trained and published a model, which datasets were used for training, the values of metrics measuring predictive performance, and when the model was deployed to production. It's a place to find, publish, and use ML models or model pipeline components.
![](Pasted%20image%2020230613145730.png)

**Prediction Service**: The trained model is served as a prediction service to production. It's important to note that the process is concerned only with deploying the trained model as a prediction service, for example, a microservice with a REST API, instead of deploying the entire ML system. For example, Google's AI Platform Prediction service has an API for serving predictions from machine learning models. The AI Platform Prediction service can host models trained in popular machine learning frameworks, including TensorFlow, XGBoost, and Scikit-Learn.
![](Pasted%20image%2020230613145802.png)

**Monitoring:** Monitoring lets you detect model performance degradation or model staleness. The output of monitoring for these changes then feeds into the data analysis component, which can serve as a trigger to execute the pipeline or to execute a new experimental cycle. For example, monitoring should be designed to detect data skews, which occur when your model training data is not representative of the live data.

**Using Google Cloud Monitoring:** To understand other performance metrics, you can configure Google's Cloud monitoring to monitor your model's traffic patterns, error rates, latency, and resource utilization.

## 02.05 - Training design decisions
## 02.06 - Serving design decisions
## 02.07 - Designing from scratch
## 02.08 - Using Vertex AI
## 02.09 - Lab: Structured Data Prediction using Vertex AI Platform

# 03. Designing Adaptable Systems
# 04. Designing High-Performance ML Systems
# 05. Building Hybrid ML Systems

