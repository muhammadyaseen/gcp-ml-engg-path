# 01. Introduction to Vertex AI Feature Store

## 01.01 - Introduction
## 01.02 - Feature Store Benefits
## 01.03 - Feature Store terminology and concepts
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
## 01.07 - PluralSight: Getting Started with GCP and Qwiklabs
## 01.08 - Lab: Using Feature Store
## 01.09 - Quiz




