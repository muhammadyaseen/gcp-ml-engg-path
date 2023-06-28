# 01. Introduction to Vertex AI Feature Store

## 01.01 - Introduction

![](images/Pasted%20image%2020230628090044.png)

## 01.02 - Feature Store Benefits


![](images/Pasted%20image%2020230628090135.png)

![](images/Pasted%20image%2020230628090204.png)

![](images/Pasted%20image%2020230628090230.png)

![](images/Pasted%20image%2020230628090254.png)


![](images/Pasted%20image%2020230628090326.png)

![](images/Pasted%20image%2020230628090338.png)

![](images/Pasted%20image%2020230628090353.png)

![](images/Pasted%20image%2020230628090410.png)

![](images/Pasted%20image%2020230628090425.png)

## 01.03 - Feature Store terminology and concepts

In this lesson, we will define feature store terminology and concepts. A feature store is a top-level container for your features and their values. When you set up a feature store,

00:15permitted users can add and share their features without additional engineering support

## 01.04 - Feature Store data model

The _Feature Store data model_ includes:
- an entity type, 
- entities, 
- features, and 
- feature values.

Feature Store uses a _time series data model_ to store a series of values for features. This model enables Feature Store to _maintain feature values as they change over time_

Let's review the data model with an example where you have a sample source data from a BigQuery table. In this example, the source data is about movies and their features.

![](images/Pasted%20image%2020230628082345.png)

- the `movie_id` column header can map to an entity type called `movies`
- The `average_rating`, `title`, and `genres` are features of the movies entity type. (isn't title basically the same thing as `movie_id` ?)
- The values in each column map to specific instances (e.g. `movie_03`) of an entity type or features (associated with that instance), which are called entities and feature values.

**About timestamps:**
- The timestamp column indicates when the feature values were generated
- In the Feature Store, the timestamps are an attribute of the feature values, not a separate resource type. 
- If all feature values were generated at the same time, you are not required to have a timestamp column.
- You can specify the timestamp as part of your ingestion request.

## 01.05 - Creating a Feature Store

In this lesson, we will describe: 
- how to create a feature store, 
- create an entity type and 
- add features and 
- also describe the ingestion process.

![](images/Pasted%20image%2020230628082453.png)

![](images/Pasted%20image%2020230628082518.png)

Vertex AI Feature Store can ingest data from tables in BigQuery or files in Cloud Storage, but for files in Cloud Storage, they must be in the Avro or CSV format.

![](images/Pasted%20image%2020230628082728.png)

![](images/Pasted%20image%2020230628083653.png)

Step 1 : Keep Feature Store via Console or Command Line etc.
Step 2: Create Entity Type
Step 3: Create Features

- Entity Types group and contain related features. For example, a movie's entity type might contain features like title and genre
- A feature is a measurable attribute of an entity type
- After you add features in your entity type, you can then associate your features with values stored in BigQuery or Cloud Storage

Recall the source requirements for feature store. You must have a column for entity IDs, and the values must be of type STRING.
![](images/Pasted%20image%2020230628083913.png)

![](images/Pasted%20image%2020230628083516.png)
Before you import data, you need to define the corresponding entity type and features. Feature store offers batch ingestion so that you can do a bulk ingestion of values into a feature store. For example, your computed source data might live in locations, such as BigQuery or Cloud Storage, and you can then ingest data from those sources into a feature store so that feature values can be served at a uniform format from the central feature store.

## 01.06 - Serving Features: Batch and Online

![](images/Pasted%20image%2020230628084315.png)


## 01.07 - PluralSight: Getting Started with GCP and Qwiklabs
## 01.08 - Lab: Using Feature Store
## 01.09 - Quiz

# 02. Raw Data to Features

## 02.01 - Introduction
## 02.02 - Overview of Feature Engineering
## 02.03 - Raw Data to Features
## 02.04 - Good features vs. Bad features
## 02.05 - Features should be known at prediction time
## 02.06 - Features should be numeric
## 02.07 - Features should have enough examples
## 02.08 - Bringing human insight
## 02.09 - Representing Features
## 02.10 - Quiz

# 03. Feature Engineering

## 03.01 - Introduction
## 03.02 - ML vs. Statistics
## 03.03 - Basic Feature Engg.
## 03.04 - Lab: Performing Basic Feature Engg. in BQML
## 03.05 - Feature Crosses
## 03.06 - Bucketize & Transform Functions
## 03.07 - Advance Feature Engg. in BQML
## 03.08 - Predicting Housing Prices
## 03.09 - Estimate Taxi Fare
## 03.10 - Temporal & Geo-Location Features
## 03.10 - Lab:  Performing Basic Feature Engg. in Keras
## 03.10 - Quiz


# 04. Preprocessing & Feature Creation

## 04.01 - Introduction
## 04.02 - Apache Beam & Dataflow
## 04.03 - Dataflow terms and Concepts
## 04.04 - Quiz

# 05. Feature Crosses

## 05.01 - Introduction
## 05.02 - What is a Feature Cross
## 05.03 - Discretization
## 05.04 - Use Feature Crosses to create a good classifier
## 05.05 - Too much of a good thing
## 05.06 - Quiz

# 06. Introduction to TF Transform

## 06.01 - Introduction
## 06.02 - TF Transform
## 06.03 - Analyze Phase
## 06.04 - Transform Phase
## 06.05 - Supporting Serving
## 06.06 - Lab: Exploring `tf.transform`
## 06.07 - Quiz

