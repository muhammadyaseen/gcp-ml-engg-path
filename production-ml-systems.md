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

TFX components

![](images/Pasted%20image%2020230625121201.png)

## 03.01 - Intro

Welcome to Designing Adaptable ML systems. In this module, we'll explore how to: recognize the ways that a model is dependent on data, make cost-conscious engineering decisions, know when to roll back a model to an earlier version, debug the causes of observed model behavior, and implement a pipeline that is immune to one type of dependency.

## 03.02 - Adapting to data

When it comes to adapting to change, consider which of these four is most likely to change?
![](images/Pasted%20image%2020230617163355.png)

**The answer is that all of them can, and often do, change.**

Let's see how this happens, and what to do about it with a couple example scenarios

- an upstream model whose output u were consuming changes
- the data logging format chages
- some features are added w/o proper scrutiny, they are not causal but only correlated. 

## 03.03 - Changing Distribution

The statistical term for changes in the likelihood of observed values like model inputs is changes in the distribution. And it turns out that the distribution of the data can change for all sorts of reasons.

There's another name for when models are asked to make predictions on points in feature space that are far away from the training data, and that’s extrapolation. Extrapolation means to generalize outside the bounds of what we’ve previously seen. Interpolation is the opposite. It means to generalize within the bounds of what we’ve previously seen. Interpolation is always much easier.

You can protect yourself from changing distributions using a few different methods.

- The first thing you can do is be vigilant through monitoring. You can look at the descriptive summaries of your inputs and compare them to what the model has seen. If, for example, the mean or the variance has changed substantially, then you can analyze this new segment of the input space, to see if the relationships learned still hold.
-  You can also look to see whether the model’s residuals, that is the difference between its predictions and the labels, has changed as a function of your inputs. If, for example, you used to have small errors at one slice of the input and large in another, and now it’s switched, this could be evidence of a change in the relationship.
- Finally, if you have reason to believe that the relationship is changing over time, you can force the model to treat more recent observations as more important by writing a custom loss function, or by retraining the model on the most recent data.

## 03.04 Lab : Adapting to Data

One of the engineers acknowledges this but still insists that it’s worth spending time doing an extensive ablation analysis, where the value of an individual feature is computed by comparing it to
a model trained without it. What might this engineer be concerned about? The engineer might be concerned about legacy and bundled features. 
- Legacy features are older features that were added because they were valuable at the time. But since then, better features have been added, which have made them redundant without our knowledge
- Bundled features on the other hand, are features that were added as part of a bundle, which collectively are valuable but individually may not be. Both of these features represent additional unnecessary data dependencies.

![](images/Pasted%20image%2020230617164322.png)

## 03.05 - Right and Wrong Decisions

Some decisions about data are a matter of weighing cost versus benefit, like short-term performance goals against long-term maintainability. Others, though, are about right and wrong.

We refer to this idea where the label is somehow leaking into the training data as **data leakage**. Data leakage is related to a broader class of problems we’ve seen before in the last specialization, where we talked about models learning unacceptable strategies. Previously, we learned that when there’s class imbalances, a model might learn to predict the majority class. In this case, the model has learned to use a feature that wouldn’t actually be known and which cannot be plausibly causally related to the label.

**Cancer example:**

Hospital name as feature.

Hospital name is very predictive of the outcome label, but causally unrelated to the actual relationship b/w symptoms and disease. We shouldn't include this as a training feature.

**Author political affiliation example:**

Wrong way to split

![](images/Pasted%20image%2020230617165048.png)

Right way to split

![](images/Pasted%20image%2020230617165113.png)



## 03.06 - System Failure

**Scenario 1**
Here’s another slightly different scenario. You've trained a product recommendation model based on users’ click and purchase behavior on your ecommerce site.

It's impossible to have models unlearn things that have already been learned, but one thing you can do is roll back the model's state to a time prior to the data pollution. In order to do this, you will need infrastructure that automatically creates and saves models as well as their meta information.

**Scenario 2**

Another scenario. You’ve trained a static product recommendation model which alone will determine which products users see when they are on the home page and when they are viewing individual products.

What went wrong? Well, your model is not updating to new users, new products, and new patterns in user preference. Because the model only knows about your older products, it continues to recommend them long after they’ve fallen out of favor. Ultimately, users simply ignored the recommendations altogether, and made do with the site’s search functionality.

The solution here is to dynamically retrain your model on newer data, and also to understand the limits of your model.

**Scenario 3**
Here’s one other scenario. You’ve deployed a statically-trained fraud detection model and its performance starts off good but quickly degrades. 

What’s gone wrong here? In adversarial environments, where one party is trying to beat another, it’s particularly important to dynamically retrain the model, to keep up with the most recent strategies.

## 03.07 - Concept Drift

Why do machine learning models lose their predictive power over time?

traditional machine learning algorithms were developed  with certain assumptions

![](images/Pasted%20image%2020230617174626.png)

Drift is the change in an entity with respect to a baseline.

01:02In the case of production ML models, this is the change between the real-time production data and a baseline data set, likely the training set, that is representative of the task the

01:13model is intended to perform

But production data can diverge or drift from the

01:38baseline data over time due  to changes in the real world. There are several types of drift in ML models. 

![](images/Pasted%20image%2020230617174847.png)


![](images/Pasted%20image%2020230617174901.png)

In this supervised learning  classification example, when the distribution of the label changes,

04:59it could mean that the relationship between features and labels is changing as well. At the very least, it’s likely that our model’s predictions, which will typically match  the distribution of the labels

05:10on the data on which it was trained, will be significantly less accurate.

![](images/Pasted%20image%2020230617175126.png)

![](images/Pasted%20image%2020230617175251.png)

![](images/Pasted%20image%2020230617175302.png)
![](images/Pasted%20image%2020230617175350.png)

Concept drift can occur due to shifts

08:13in the feature space and /  or the decision boundary, so we need to be aware of these during production. If the data is changing, or if the relationship between the features

08:23and the label is changing, this is going to cause issues with our model
There are 4 different types of concept drift.

![](images/Pasted%20image%2020230617175453.png)

## 03.08 - Actions to mitigate Concept Drift

both data drift and concept drift lead to model drift

![](images/Pasted%20image%2020230617175607.png)

![](images/Pasted%20image%2020230617175637.png)

Also, for concept drift, you can design your systems to detect changes. Periodically updating your static model with more recent historical data, for example, is a common way to mitigate concept drift.

Remember, concept drift is the change in relationships between the model inputs and the model output. After your diagnosis and mitigation efforts, retraining or refreshing the model over time will help to maintain model quality.

![](images/Pasted%20image%2020230617175741.png)

## 03.09 - Tensorflow Data Validation
There are three phases in a pipeline. 
- Data is ingested and validated, 
- a model is trained and analyzed, 
- and the model is then deployed in production

In this video, we’ll provide an overview of TensorFlow Data Validation, which is part of _the ingest and validate data phase_. TensorFlow Data Validation is a library for analyzing and validating machine learning data.

Two common use-cases of TensorFlow Data Validation within a TensorFlow Extended pipelines are:
- validation of continuously arriving data, and 
- training-serving skew detection

This brings us to the **three** main components TFDV. 
- The Statistics Generation component, which generates statistics for feature analysis. 
- The Schema Generation component, which gives you a description of your data. 
- And the Example Validator component, which allows you to check for anomalies

In general, any difference between how you generate your training data and your serving data, the data you use to generate predictions, should be reviewed to prevent training-serving skew. Training-serving skew can also occur based on your data distribution in your training, validation, and testing data splits. To summarize, distribution skew occurs when the distribution of feature values for training data is significantly different from serving data and one of the key causes for distribution skew is how data is handled or changed in training vs production.

## 03.10 - Components of Tensorflow Data Validation

TFDV has 3 Components

![](images/Pasted%20image%2020230625121036.png)

For the `trip_start_hour` feature:, 
- it appears there is not that much data in the early morning hours. It appears that the `trip_start_hour` column, where the time window is between `2:00am to 6:00am`, has data missing. 
- This helps determine the area we need to focus on to fix any data-related problems. We’ll need to get more data, otherwise, the prediction for 4:00am data will be overgeneralized.

- The `StatisticsGen` component generates feature statistics and random samples over training data, which can be used for visualization and validation. 
- These statistics are being generated from a Pandas DataFrame. You can also generate statistics from a CSV file or a TF.Record formatted file.
- StatisticsGen generates both numeric and categorical features
- Notice that for our categorical features, in addition to seeing the  number of missing values, we also see the number of unique values. 
- TensorFlow Data Validation can also help you identify unbalanced data distributions.
- There are a number of  `StatisticsGen` **data validation checks** that you should be aware of. These include: Feature min, max, mean, mode, and median, feature correlations, class imbalance, check to see missing values, and histograms of features, for both numerical and categorical. 
- The SchemaGen TFX pipeline component can specify data types for feature values, whether a feature has to  be present in all examples, allowed value ranges, and other properties.
- A SchemaGen pipeline component will automatically generate a schema by inferring types, categories, and ranges  from the training data
 
In this example visualization, "type" indicates the feature datatype, "presence" indicates whether the feature must be present in 100% of examples or not, so whether it’s required or optional. "Valency" indicates the number of values required per training example. "Domain" and "Values" indicates the feature domain and its values.

The `ExampleValidator` pipeline component **identifies anomalies** in training and serving data by comparing data statistics computed by the StatisticsGen pipeline component against a schema. It takes the inputs and looks for problems in the data, like missing values, and reports any anomalies.

## 03.11 - Lab - Intro to Tensorflow Data Validation

**Synopsis:** This lab is an introduction to TensorFlow Data Validation (TFDV), a key component of TensorFlow Extended. This lab serves as a foundation for understanding the features of TFDV and how it can help you understand, validate, and monitor your data.

TFDV can be used for generating schemas and statistics about the distribution of every feature in the dataset. Such information is useful for comparing multiple datasets (e.g., training vs inference datasets) and reporting.

Statistical differences in the features distribution TFDV also offers visualization capabilities for comparing datasets based on the Google PAIR Facets project.
![](images/Pasted%20image%2020230625122041.png)

#### Task 1. Set up environment
#### Task 2. Launch Vertex AI Notebooks
#### Task 3. Clone course repo
#### Task 4. Follow Intro to TF DV notebook
1. In the notebook interface, navigate to **training-data-analyst > courses > machine_learning > deepdive2 > production_ml > labs**, and open **tfdv_basic_spending.ipynb**.
2. 

## 03.12 - Lab - Advanced Visualizations with Tensorflow Data Validation
## 03.13 - Mitigating train-serve skew thru design
## 03.14 - Lab - Serving ML Predictions in Batch and Real Time
## 03.15 - Diagnosing a production model

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

## 05.01 - Intro

You will learn how to build hybrid machine learning models, and how to optimize TensorFlow graphs for mobile. Let's start by discussing a technology called Kubeflow, which helps us build hybrid cloud machine learning models. But why are we discussing hybrid in the first place?

**Why would you need anything other than Google Cloud?** There are times, when you cannot be fully cloud native. What kinds of situations? You may not be able to do machine learning solely on the cloud. Perhaps you are tied to on-premises infrastructure, and your ultimate goal is to move to the public cloud, but it's gonna take a few years. 

Perhaps there are constraints about being able to move your training data off your on-premise cluster or data center. So you have to make do with the system that you have. 

Or maybe the data that is being produced is produced by a system that is running on a different cloud or the model predictions need to be consumed by an application on some other cloud. So you need a multi-cloud solution, not a solution that is solely GCP. 

Or maybe you are running machine learning on the edge and connectivity constraints force you to have to do your predictions on the edge-- on the device itself. And so you have to do inference on the edge. This is a very common situation if you're doing Internet of Things.

So here, for example, is Cisco's hybrid cloud architecture. Cisco partnered with Google Cloud to bridge their private cloud infrastructure and their existing applications with Google Cloud Platform

Kubeflow helps you migrate between cloud and on-prem environments.

## 05.02 - ML on Hybrid Cloud

In order to build hybrid machine learning systems that work well both on-premises and in the cloud, your machine learning framework has to support three things:

![](images/Pasted%20image%2020230617122134.png)

**Composability**
Each machine learning stage-- data analysis, training, model validation, monitoring-- these are all independent systems. Everyone has a different way to handle these. When we say "composability," it's about the ability to compose a bunch of microservices together and the option to use what makes sense for your problem.

**Portability**
But now that you've built your specific framework, you want to move it around. And that's where we get into portability. The stack that you use is likely made up of all these components-- and probably lots more. you want to move it around. And that's where we get into portability.

**Scalability:**
scalability in machine learning means so many more things. Accelerators-- GPUs, TPUs, et cetera-- disks, skillsets-- software engineers, researchers, data engineers, data analysts, data scientists, different skillsets-- teams across the org-- because there's teams that are gonna be building the experiments, teams that are gonna be using the experiments, teams that are gonna be monitoring the machine learning models.

## 05.03 - Kubeflow

In this module, you will learn:
 - how to build hybrid cloud machine learning models with Kubeflow and
 - how to optimize TensorFlow graphs for mobile

Kubeflow is the machine learning toolkit for Kubernetes, and it brings several benefits. It makes deploying machine learning workflows on Kubernetes simple, portable, and scalable

Kubeflow can actually run on anything, whether it's a phone, a laptop, or an on-premises cluster. Regardless of where it's run, the code remains the same. Some of the configuration settings just change. Portability is at the container level, and you can move to any environment that offers Kubernetes.

## 05.04 - Lab - Kubeflow Pipelines on Vertex AI 2.5

In this lab, you learn how to utilize Vertex AI Pipelines to execute a simple Kubeflow Pipeline SDK derived ML Pipeline.

![](images/Pasted%20image%2020230617160126.png)

The main point of the exercise is that we can run a compiled pipeline (.json) by importing it from GCS via Vertex AI Pipelines.

#### Task 1. Set up the project environment

First we add the Storage Admin Role to the default Compute Engine Service Account

Then run

```bash
gcloud storage buckets create gs://qwiklabs-gcp-03-ce6ef1ccac82
touch emptyfile1
touch emptyfile2
gcloud storage cp emptyfile1 gs://qwiklabs-gcp-03-ce6ef1ccac82/pipeline-output/emptyfile1
gcloud storage cp emptyfile2 gs://qwiklabs-gcp-03-ce6ef1ccac82/pipeline-input/emptyfile2
```

The Pipeline has already been created for you and simply requires a few minor adjustments to allow it to run in your Qwiklabs project. Download the AI Pipeline from the lab assets folder:

`wget https://storage.googleapis.com/cloud-training/dataengineering/lab_assets/ai_pipelines/basic_pipeline.json`

#### Task 2. Configure and inspect the Pipeline code

The Pipeline code is a compilation of two AI operations written in Python. The example is very simple but demonstrates how easy it is orchestrate ML procedures written in a variety of languages (TensorFlow, Python, Java, etc.) into an easy to deploy AI Pipeline. The lab example performs two operations, concatenation and reverse, on two string values.

1. First you must make an adjustment to update the output folder for the AI Pipeline execution
`sed -i 's/PROJECT_ID/qwiklabs-gcp-03-ce6ef1ccac82/g' basic_pipeline.json`

2. Inspect **basic_pipeline.json** to confirm that the output folder is set to your project:
`tail -20 basic_pipeline.json`

3. move the updated **basic_pipeline.json** file to the Cloud Storage bucket created earlier so that it can be accessed to run an AI Pipeline job
`gcloud storage cp basic_pipeline.json gs://qwiklabs-gcp-03-ce6ef1ccac82/pipeline-input/basic_pipeline.json`

#### Task 3. Execute the AI Pipeline

**Vertex AI** > Pipelines > Create Run 

1. From **Run detail**, select **Import from Cloud Storage** and for **Cloud Storage URL** browse to the **pipeline-input** folder you created inside your project's cloud storage bucket. Select the **basic_pipeline.json** file.
2. Click **Select**.
3. Leave the remaining default values, click **Continue**. Click Submt

What the run looks like:

![](images/Pasted%20image%2020230617161307.png)

**Concat job info**

![](images/Pasted%20image%2020230617161116.png)

**Reverse job info:**
![](images/Pasted%20image%2020230617161327.png)

The pipeline file is saved as `production-ml-systems/kfp-basic-pipeline.json`

## 05.05 - Tensorflow Lite

 Mobile TensorFlow makes sense when there's a poor or missing network connection,

00:16or where sending continuous data to a server would be too expensive. The purpose is to help developers make lean mobile apps using TensorFlow, both by continuing to reduce the code footprint,

00:30and by supporting quantization and lower-precision arithmetic that make the models smaller.

a new frontier is confederated learning. The idea is you continuously train the model on the device, and then you combine the model updates from a federation of user devices

01:58to update the overall model.

## 05.06 - Optimizing Tensorflow for Mobile

Let's look at a second scenario where hybrid models are necessary

Take Google Translate, for example, which is composed of several models.

It uses one model to find a sign, another model to read the sign, using optical character recognition, a third model to translate the sign, a fourth model to superimpose the translated text, and a fifth model to select the best font to use.

Dependending on latency and accuracy requirements we might need a hybrid of on device and on cloud inference. This means sometimes embedding the model within the device itself.

TensorFlow Lite makes specific compromises to enable machine learning inference on low-power or under-resourced devices

## 05.07 - Quiz





