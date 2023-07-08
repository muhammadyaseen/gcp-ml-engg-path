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
![](Pasted%20image%2020230628172817.png)

## 02.02 - Overview of Feature Engineering

It involves the transformation of a given feature with the objective of reducing the modeling error for a given target. The underlying representation of the data is crucial for the learning algorithm to work effectively.

The training data used in machine learning can often be enhanced by extraction of features from the raw data collected. In most cases, appropriate transformation of data is an essential prerequisite step before model construction.

Feature engineering can be defined as a process that attempts to create additional relevant features from the existing raw features in the data and to increase the predictive power of the learning algorithm. It can also be defined as the process of combining domain knowledge, intuition and data science skill sets to create features.

Feature vectors can be numerical, categorical, bucketized, crossed and hashed.

Feature Engg asks: How can you get the most out of your data for predictive modeling? This is the problem that the process and practice of feature engineering solves.

Studying other feature-engineering problems is highly recommended. The example of seasonal sales, for example, lends itself to time-series features and longitude and latitude features.

Feature engineering in reality is an _iterative process_. In other words, you create a _baseline model with little to no feature engineering_ as determined by your data types and then add feature engineering to see how your model improves. Feature engineering types include using indicator variables to isolate key information, for example geolocation features for a New York City taxi service where you isolate a certain area for your training data set. 

It can also include _highlighting interactions between two or more features_, meaning that you can actually include the sum of two features, the difference between two features, the product of two features and the quotient of two features. And another example is to transform categorical features into dummy variables.

![](Pasted%20image%2020230628173437.png)

## 02.03 - Raw Data to Features


## 02.04 - Good features vs. Bad features
What makes a good feature?

![](Pasted%20image%2020230628173825.png)

You should have some reasonable idea of why these things could affect the outcome.
You need to have a reasonable hypothesis about why a particular feature might matter for this particular problem

## 02.05 - Features should be known at prediction time
## 02.06 - Features should be numeric
## 02.07 - Features should have enough examples

You need to have enough examples of feature value in the dataset, so it's the value of the feature. You need to have enough examples of this

Rule of thumb, and this is just a rule of thumb, is that you need to have at least five examples of any value before using it in your model. If you have only three auto transactions in your dataset and all three of them are not fraudulent, then essentially a model is going to learn that _nobody can ever commit fraud on auto transactions_. And that's going to be a problem. You want to avoid having values of which you don't have enough examples.

If you have a 10% off or a 5% off or a 15% off, you'd probably have at least five examples of these. But what if you have this one special customer to whom you give an 85% discount? Can you use that in your dataset? No. You don't have enough samples. That 85% is now way too specific.

But what if you have continuous numbers? Then you may have to group them up. This is called "discretization." And then see if, when discrete bands, you have at least five examples of each band. We're talking about the values of the features, not the values of the labels.
 
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

