[My Google Cloud Skill Boost Public Profile](https://www.cloudskillsboost.google/public_profiles/d85f8295-b522-4522-964c-f0fcf9375090)

# ML and Big Data Fundamentals on GCP
## GCP Overview

GCP uses 'Project' to organize resources. AFAIR there wasn't something equivalent in AWS.
Scalabale, Available, Reliable, Secure, 4Vs
Infrastructure parts: Compute, Storage, Big Data, ML
Compute and Storage can scale independently based on need. They are decoupled.

**Compute offerings**:
- Compute: IaaS offering, Gives compute, some storage
- GKE: containerized applications
- App Enginer: fully managed PaaS
- Cloud Funcitons: FaaS
- Cloud Run: fully managed platform, auto scale (like Fargate?)

**Storage and DB**:
Right storage depends on data and business need
- Cloud storage (Unstructured, has storage classes like S3)
- BigTable (Analytical, NoSQL, real-time high-throughput apps like Hbase?)
- Cloud SQL (Analytical + Transactional, relational, local/regional scale, Aurora?)
- Spanner (Analytical + Transactional, relational, global scale)
- Firestore (Transactional, NoSQL document oriented db, Like DynamoDB)
- BigQuery (Analytical, SQL, Datawarehouse soln. like Hive/Athena?)

**Big Data and ML product Categories**:
- Ingestion and Process (stream and batch): Pub/Sub, Dataflow (streaming), Dataproc, Cloud DataFusion
- Storage: Cloud Storage, BigTable, BigQuery, FireStore, Spanner
- Analytics: BigQuery, Data Studio, Looker
- ML: Vertex AI, Auto ML

## Data Engg for Streaming Data

**Pub/Sub**: 
- Distributed message oriented architecture
- Ensures at least once delivery.
- Supports many different inputs and outputs. 
- Can deliver/broadcase msg to subscribers e.g Dataflow.

**Dataflow**:
- Dataflow can handle both stream and batch data. It is mainly a service for ETL. (like Glue Jobs?)
- Apache Beam can be used for pipeline design, provides pipeline templates.
- Choosing execution engine to run the pipeline. Dataflow is a fully managed service to run Beam pipelines. It is serverless and NoOps (has auto-scalling , graph optimization etc.)
- Dataflow has templates for Streaming, Batch, Utility which gives a stattng point for common use cases. For ex, we can ingest data from pub sub into dataflow then load it in bigquery.

**Visualization:**
- Once data is in a database like bigquery, we can use Looker to visualize and create dashbroads.
- Another tool is Data Studio. it is more easy to use for non-experts. It doesn't require creating connectors that Looker does. Steps: choose template, link dashboard to datasource (e.g. BQ), explore dashboard

## Big Data with Big Query

Big Query is like Amazon Athena

**Big Query**: 
- Fully managed serverles soln. provides Storage (dataset/tables) and Analytics (via SQL), and has built-in ML
- BQ connects with AutoML and Vertex AI Workbench i.e. we can load / store datasets in BQ
- BQ can also query external data sources e.g. Cloud Storage, Cloud Spanner, Could SQL. Also from AWS, Azure.

**3 patterns to load**: batch load, streaming, generated results from queries

**Big Query ML**: can train models via queries, can do transforms like 1-hot enc, can predict with queries, hyp param tuning etc. also supports loading tf modesl, and exporting BQ models. kinda like AWS Athena.

**BQ ML project phases**:
- ETL into BigQuery (has connectors to other google services)
- pre-process features (using sql)
- create model inside bigquery (CREATE MODEL command)
- eval performance of model (ML.EVALUATE command)
- use model to predict new data (ML.PREDICT(model, data) command)

**BQ ML Key Commands**:
```
CREATE OR REPLACE MODEL `model_name` 
WITH OPTIONS(model_type, input_label_cols) AS
<training dataset>
```
![[images/Pasted image 20230202181526.png]]

## ML Options on GCP

GCP offers **4 options for building ML models**
- BQ ML: as we saw above
- Pre-Built APIs: for models built and trained by Google
- AutoML: point and click NoCode interface
- Custom Training: full flexibility and control over ML pipeline
![[images/Pasted image 20230202183051.png]]
Comparison - in depth
![[images/Pasted image 20230202183234.png]]

**Pre-Built APIs**: offered as a service, for Speech-to-Text, NLP, translation, text-to-speech, vision api, video intelligence.

**AutoML**: Goal was to automate ML pipelines. It has 2 vital parts: transfer learning and neural architecture search. We can upload data into AutoML (from bigquery, cloud storage etc.) 

**Custom model**: Vertex AI Workbench can work as an AI development env. Can use pre-built vs customer 'containers'.

**Vertex AI**: unified platform that brings all components of ML workflow together. Provides Feature Store, Hyperparam tuning (Vizier), XAI, pipelines

## The ML Workflow with Vertex AI
AutoML based example
Data Preparation: Upload data, provide a name, select data type and objective
Feature Engg: using Feature Store, it makes features shareable, reusabale, scalable
Model training and Model eval: VAI provides extensive set of metrics after training via AutoML
Model serving:
	- Serving = deployment + monitoring
	- Deploy options: Endpoint for RT preds, Batch Pred, Offline / Edge pred
	- Monitoring: VAI Pipelines, 