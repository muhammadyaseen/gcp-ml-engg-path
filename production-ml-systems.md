 Synopsis: This course covers how to implement the various flavors of production ML systems— static, dynamic, and continuous training; static and dynamic inference; and batch and online processing. You delve into TensorFlow abstraction levels, the various options for doing distributed training, and how to write distributed training models with custom estimators.

# 01. Intro to Advanced ML on Google Cloud

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

Well, when making decisions about training, you have to decide whether the phenomenon you're modeling is more like physics or like fashion. When training your model, there are two paradigms: **Static vs Dynamic training** (like Physics vs Fashion) 

- In dynamic environments like fashion, we'll have to constantly re-train our models.
- Part of the reason the dynamic is harder to build and test is that new data may have all sorts of bugs in it. (this is later discussed in Adaptable ML systems)
- Static is simpler to build and test, but will probably become stale
- Dynamic training can make use of 3 potential architectures (Cloud Functions, App Engine, Dataflow)
![](Pasted%20image%2020230613150614.png)

**3 potential architectures for dynamic training**

1. File arrives in Cloud Storage -> Launch Cloud Func -> Start Train job on AI platform -> AI platform produces new model -> Deploy
2. User web request -> start AI platform training job -> New model -> Deploy
3. Streaming topic ingested into Pub/Sub topic -> Consumed by Dataflow and aggregated -> Write agg data in BigQuery -> AIP job launched on new data in BQ -> New model  -> Deploy

**Streaming (Pub/Sub), structured batch (BigQuery), unstructured batch (Cloud Storage)**

## 02.06 - Serving/Inference design decisions (R)

Just as the use case determines appropriate training architecture, it also determines the appropriate serving architecture. One of the goals: **minimize latency**.

Remarkably, _the solution for serving models_ is very similar to what we do to optimize IO performance. We use a cache. In this case, instead of faster memory, we'll use a table.

- **Static serving** then computes the label ahead of time and serves by looking it up in the table.
- **Dynamic serving** in contrast computes the label on demand. 

There's a space-time trade-off. 

![](Pasted%20image%2020230204191816.png)
_Static serving is space intensive_, resulting in higher storage costs because we store precomputed predictions with a low, fixed latency and lower maintenance costs. _Dynamic serving, however, is compute intensive_. It has lower storage costs, higher maintenance, and variable latency. 

The choice of whether to use static or dynamic serving is determined by considering how important costs are with regard to latency, storage, and CPU.

it might be helpful to consider static and dynamic serving through another lens: **Peakedness and Cardinality**.

Taken together peakedness and cardinality create a space. 
- When the cardinality is sufficiently low, we can store the entire expected prediction workload.
- When the cardinality is high because the size of the input space is large and the workload is not very peaked, you probably want to use dynamic training.
- In practice though, a hybrid of static and dynamic is often chosen, where you statically cache some of the predictions while responding on demand for the long tail.
- The striped area above the curve and not inside the blue rectangle is suitable for a hybrid solution, with the most frequently requested predictions cached and the tail computed on demand

![](Pasted%20image%2020230613151332.png)


## 02.07 - Designing from scratch

## 02.08 - Using Vertex AI

With Vertex AI, you can access a dashboard, datasets, features, labeling tasks, notebooks, pipelines, training, experiments, models, endpoints, batch predictions, and metadata. Vertex AI is Google Cloud’s unified ML platform and provides services like:

**Notebooks** for prototyping and model development.

**Feature Store:** Uploaded datasets are stored in a Cloud Storage bucket that acts as an input for both AutoML and custom training jobs. In the feature store, the timestamps are an attribute of the feature values, not a separate resource type. If all feature values were generated at the same time, you don’t need to have a timestamp column.

**Training pipelines** are the primary model training workflow in Vertex AI, which can use training pipelines to create an AutoML-trained model or a custom-trained model. For custom-trained models, training pipelines orchestrate custom training jobs and hyperparameter tuning in conjunction with steps like adding a dataset or uploading the model to Vertex AI for prediction serving Both custom jobs and hyperparameter tuning, however, are only used by custom-trained models.

**Models** are built from datasets or unmanaged data sources. Many different types of machine learning models are available with Vertex AI. A new model can be trained, or an existing model can be imported. After the model has been imported into Vertex AI, it can be deployed to an endpoint and then used to request predictions. AutoML can be used to train a new model with minimal technical effort.

**Endpoints** are machine learning models made available for online prediction requests. An endpoint is an HTTPS endpoint that clients can call to receive the inferencing (scoring) output of a trained model. They can provide timely predictions from many users, for example, in response to an application request you must define an endpoint in Vertex AI by giving it a name and location and deciding whether the access is Standard, which makes the endpoint available for prediction serving through a REST API. Multiple models can be deployed to an endpoint, and a single model can be deployed to multiple endpoints to split traffic. You might deploy a single model to multiple endpoints to test out a new model before serving it to all traffic. Either way, it’s important to emphasize that a model must be deployed to an endpoint before that model can be used to serve online predictions.

## 02.09 - Lab: Structured Data Prediction using Vertex AI Platform

**Synopsis:** In this lab you train, evaluate, and deploy a machine learning model to predict a baby's weight.

![](images/Pasted%20image%2020230617100355.png)

#### Task 1. Create storage bucket
#### Task 2. Launch Vertex AI Notebooks
#### Task 3. Clone course repo within your Vertex AI notebooks instance

`git clone https://github.com/GoogleCloudPlatform/training-data-analyst`

#### Task 4. Structured data prediction using Vertex AI Platform

1. In the notebook interface, navigate to **training-data-analyst > courses > machine_learning > deepdive2 > production_ml > babyweight**, and open **train_deploy.ipynb**.

Follow NB instructions.

**Note**: The completed notebook is saved in `notebooks/production-ml-systems/structured-data-prediction-using-vertex-ai-platform.ipynb`


# 03. Designing Adaptable Systems

## 03.01 - Intro
## 03.02 - Adapting to data
## 03.03 - Changing Distribution
## 03.03 - Right and Wrong Decisions
## 03.02 - System Failure
## 03.02 - Concept Drift
## 03.02 - Actions to mitigate Concept Drift
## 03.02 - Tensorflow Data Validation
## 03.02 - Components of Tensorflow Data Validation
## 03.02 - Lab - Intro to Tensorflow Data Validation
## 03.02 - Lab - Advanced Visualizations with Tensorflow Data Validation
## 03.02 - Mitigating train-serve skew thru design
## 03.02 - Lab - Serving ML Predictions in Batch and Real Time
## 03.02 - Diagnosing a production model

# 04. Designing High-Performance ML Systems

## 04.01 - Intro
## 04.02 - Training
## 04.03 - Predictions
## 04.04 - Why distributed training is needed
## 04.05 - Distributed training architectures
## 04.06 - Tensorflow Distributed training strategies
## 04.07 - Mirrored strategy
## 04.08 - Multi-worker Mirrored strategy
## 04.09 - TPU strategy
## 04.10 - Parameter server strategy
## 04.11 - Lab: Distributed Training with Keras
## 04.12 - Lab: Distributed Training with GPUs on Cloud AI Platform
## 04.13 - Training on large datasets with `tf.data` API
## 04.14 - TPU Speed Data Pipelines
## 04.15 - Inference
## 04.16 - Quiz


# 05. Building Hybrid ML Systems

## 05.01 - ML on Hybrid Cloud
## 05.02 - Kubeflow
## 05.03 - Lab - Kubeflow Pipelines on Vertex AI 2.5
## 05.04 - Tensorflow Lite
## 05.05 - Optimizing Tensorflow for Mobile



