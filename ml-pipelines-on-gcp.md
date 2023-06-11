# ML Pipelines on Google Cloud

**Synopsis**: In this course, you will be learning from ML Engineers and Trainers who work with the state-of-the-art development of ML pipelines here at Google Cloud. The first few modules will cover about TensorFlow Extended (or TFX), which is Google’s production machine learning platform based on TensorFlow for management of ML pipelines and metadata. You will learn about pipeline components and pipeline orchestration with TFX. You will also learn how you can automate your pipeline through continuous integration and continuous deployment, and how to manage ML metadata. Then we will change focus to discuss how we can automate and reuse ML pipelines across multiple ML frameworks such as tensorflow, pytorch, scikit learn, and xgboost. You will also learn how to use another tool on Google Cloud, Cloud Composer, to orchestrate your continuous training pipelines. And finally, we will go over how to use MLflow for managing the complete machine learning life cycle.

# 01. Introduction to TFX Pipelines

## 01.01 - Tensorflow Extended (TFX)

TensorFlow Extended is Google's production machine learning platform based on TensorFlow.
It provides a flexible configuration framework and shared libraries to integrate common machine learning tests implemented as components needed to define, launch, and monitor your machine learning system. TFX makes ML ops easier through all phases of the ML project lifecycle from prototyping to production. 

It is designed to orchestrate your machine learning workflow with portability to multiple environments and orchestration frameworks in mind. This includes Apache Airflow, Apache Beam and Coop Flow.

![](images/Pasted%20image%2020230611122840.png)

A possible mapping of different components to GCP Services.

![](images/Pasted%20image%2020230611122900.png)

![](images/Pasted%20image%2020230611122920.png)


## 01.02 - TFX Concepts

A **TFX component** is an implementation of the machine learning task in your pipeline. They are designed to be modular and extensible, while incorporating Google's machine learning best practices on tasks such as data partitioning, validation and transformation.

Each step of your **TFX pipeline**, again called a **component**, produces and consumes structured data representations called **artifacts**. Subsequent components in your workflow may use these artifacts as inputs. In this way, TFX lets you transfer data between components during continuous execution of your pipeline.
![](images/Pasted%20image%2020230611131731.png)

Components are composed of **five elements**:

![](images/Pasted%20image%2020230611131814.png)

Component instances produce artifacts as outputs, and typically depend on artifacts produced by upstream component instances, as inputs, for example of transform data is an artifact. It depends on the training data artifact ingested into your pipeline, and serves as input to your model during model training.

![](images/Pasted%20image%2020230611132246.png)

 First, a driver reads the component specification for runtime parameters, and retrieve the required artifacts from the ML metadata store for the component. 
 
 Second, and executer performs the actual computation on the retrieved input artifacts in generates the output artifacts. 
 
 Finally, the publisher reads the component specification to log the pipeline component run and ML metadata and write the components output artifacts to the artifact store. 

![](images/Pasted%20image%2020230611132305.png)

 TFX pipelines are a sequence of components linked together by a directed acyclical graph of the relationships between artifact dependencies. They communicate through input and output channels.
![](images/Pasted%20image%2020230611132336.png)
Orchestrators ensure consistency with pipeline execution order, component logging, retries and failure recovery, and intelligent parallelization of component data processing

The ML metadata library stores the metadata in a relational back end. For notebook prototypes, this can be local SQL lite database and for production cloud deployments, this could be a managed MySQL or Postgres database. ML metadata does not store the actual pipeline artifacts. TFX automatically organizes and stores the artifacts on local file systems on a remote cloud storage file system for a consistent organization across your machine learning projects.

![](images/Pasted%20image%2020230611132414.png)
For most machine learning cases with TFX, you will only interact with the integrated front end layer and don't need to engage directly with the orchestrator, shared libraries, and ML metadata unless you need additional customization.

## 01.03 - TFX standard Data components

TFX standard components come pre-packaged with TensorFlow, and are designed to help improve your pipeline development velocity. You are able to mix together standard and custom component to suit your machine learning workflow needs. Each standard component is designed around common machine learning tasks and encodes Google's ML best practices around tasks such as data monitoring and validation, training and serving data transformations, model evaluation, and serving.

![](images/Pasted%20image%2020230611132735.png)
The ExampleGen TFX pipeline component is the entry point to your pipeline that adjusts data. As inputs, ExampleGen supports out-of-the-box ingestion of external data sources such as CSV, TF Records, Avro, and Parquet. As outputs, ExampleGen produces TF examples or TF sequence examples, which are highly efficient in performant data set representations that can be read consistently by downstream components. This comes with directory management and logging by default to ensure ML best practices on consistent project set up. 

![](images/Pasted%20image%2020230611133823.png)

ExampleGen supports advanced data management capabilities such as data partitioning, versioning, and custom splitting on features or time.

![](images/Pasted%20image%2020230611133854.png)
StatisticsGen performs a complete pass over your data using Apache Beam and the TensorFlow Data Validation library to calculate summary statistics for each of your features over your configured train, dev, and test splits. This includes statistics like mean, standard deviation, quantile ranges, and the prevalence of null values.

As inputs, the StatisticsGen component is configured to take TF examples from the ExampleGen component. As outputs, StatisticsGen produces a data set statistics artifact that contains feature statistics used for downstream components.

![](images/Pasted%20image%2020230611133920.png)
Some TFX components use a description of your input data called a schema that can be automatically generated by the SchemaGen component. As inputs, SchemaGen reads the StatisticsGen artifact to infer characteristics of your input data from the observed feature data distributions. As outputs, SchemaGen produces a schema artifact, a description of your data's characteristics.

The schema can specify data types for feature values, whether a feature has to be present in all examples, what are the allowed value ranges for each feature, and other properties. The SchemaGen is an optional component that does not need to be run on every pipeline run.

![](images/Pasted%20image%2020230611133939.png)

The ExampleValidator pipeline component identifies anomalies in train, dev, and test data. As inputs, ExampleValidator reads feature statistics from StatisticsGen and the schema artifact produced by the SchemaGen component or imported from an external source.

As outputs, ExampleValidator outputs an anomalies report artifact. The ExampleValidator pipeline component identifies any anomalies in the example data by comparing data statistics computed by the StatisticsGen pipeline component against a schema.

![](images/Pasted%20image%2020230611134001.png)
The Transform TFX pipeline component performs feature engineering on the TF examples data artifact emitted from the ExampleGen component using the data schema artifact from SchemaGen or imported from external sources as well as TensorFlow transformations typically defined in a pre-processing function.
As outputs, the Transform component emits a SavedModel artifact that encapsulates feature engineering logic. When executed, the SavedModel will accept TF examples emitted from an ExampleGen component, and emit the Transform feature data.

![](images/Pasted%20image%2020230611134025.png)

## 01.04 - TFX standard Model components

Standard TFX components for Model Management.
![](images/Pasted%20image%2020230611143146.png)

![](images/Pasted%20image%2020230611143205.png)
![](images/Pasted%20image%2020230611143227.png)

![](images/Pasted%20image%2020230611143245.png)

![](images/Pasted%20image%2020230611143305.png)
![](images/Pasted%20image%2020230611143326.png)

![](images/Pasted%20image%2020230611143340.png)

 
## 01.05 - TFX Pipeline Nodes

Pipeline nodes are special purpose classes for performing advanced metadata operation such as importing external artifacts into ML metadata, performing queries of current ML metadata based on artifact properties and their history.

The most common pipeline node is the ImporterNode, which is a special node that registers an external resource into the ML metadata library So downstream nodes can use the registered artifact as input. The primary use case for this node is to bring in external artifacts like a schema into the TFX pipeline for use by the transform and trainer components.

![](images/Pasted%20image%2020230611140011.png)

The next type of pipeline node is a ResolverNode. ResolverNode is a special TFX node that handles special artifact resolution logistics that will be used as inputs for downstream nodes. The model resolver is only required if you're performing model validation in addition to evaluation.

![](images/Pasted%20image%2020230611140036.png)
![](images/Pasted%20image%2020230611140054.png)

## 01.06 - TFX Libraries
The diagram on the slide illustrates the relationships between TFX libraries and pipeline component

![](images/Pasted%20image%2020230611142851.png)

# 02.  Pipeline orchestration with TFX

# 03. Custom components and CI/CD for TFX pipelines

# 04. ML Metadata with TFX

# 05. Continuous Training with multiple SDKs, KubeFlow & AI Platform Pipelines

## Containerized Training Applications

We'll see:
- how to containerize training applications built with other popular machine learning frameworks, such as PyTorch, scikit-learn, and XGBoost. 
- Then, you'll learn how to deploy these containers as ops in a Kubeflow pipeline.
- You'll also see that you can train TensorFlow, PyTorch, scikit, and XGBoost models in parallel inside of a Kubeflow pipeline.

So what's the process of containerizing a TensorFlow training application?
3 step process:
- **Step 1:** First, we simply create a training script, and this script contains the code that actually ingests data, trains the model, and then saves the model.
- **Step 2:** The next step is to package the training script into a Docker image. This is where we define the dependencies of our training script
- **Step 3:** The third step is to actually build and push this image to the Container Registry. And as you know, we can do this with the `gcloud build submit --tag $IMG_URI $IMG_NAME` command.

## Containerizing PyTorch, Sci-Kit, and XGBoost Apps

What if we have a different framework? like scikit, pytorch, xgboost?
![](images/Pasted%20image%2020230407161736.png)

## Kubeflow and Vertex AI Pipelines
How to use multiple models from multiple different frameworks in a Kubeflow pipeline?
For ex, we may load train and eval data from BQ and then train 4 models, each from a different framework as shown in the following pipeline:

![](images/Pasted%20image%2020230407161906.png)

In that case: We just have to define a list for each op that contains the training arguments corresponding to each specific training op.
![](images/Pasted%20image%2020230407164547.png)
Just as we define separate lists for each training op, we also have to define separate training ops themselves that take the corresponding lists as arguments
![](images/Pasted%20image%2020230407164610.png)

## Continuous Training

Continuous training simply refers to retraining models periodically at set intervals or with specific triggers. In this discussion, we'll focus on retraining periodically or with set intervals. Model performance tends to deteriorate over time.
![](images/Pasted%20image%2020230407164932.png)
AI platform pipelines makes it incredibly easy to schedule pipeline runs. When setting up a run in the UI, we can either choose a one-off run, which just simply runs the pipeline once, or a recurring run, where we can easily schedule periodic or set interval runs of our pipeline.
![](images/Pasted%20image%2020230407165032.png)

## Lab: Continuous Training with TensorFlow, PyTorch, XGBoost, and Scikit Learn Models with Kubeflow and Vertex AI Pipelines

![](images/Pasted%20image%2020230407170343.png)

#### Task 1-4. Create Cloud Storage Bucket, Enable Services, Create AI Platform Notebook

AI Platform > Pipelines > New Instance > Configure Kubeflow > Create New Cluster > Deploy

#### Task 5-7. Clone the lab repo, and follow along in notebook

Note: I downloaded the completed notebook and saved it in `notebooks/lab-cts-train-multiple-frameworks.ipynb`

This is what a successful run looks like:

![](images/Pasted%20image%2020230408105338.png)


In this section we learned how to do manual pipeline runs, in the next section we will see an automated workflow orchestration service i.e. Cloud Composer / Airflow.


# 06. Continuous Training with Cloud Composer

## What is Cloud Composer?
![](Pasted%20image%2020230407125159.png)
![](Pasted%20image%2020230407125239.png)
![](Pasted%20image%2020230407125518.png)

**Airflow serves similar role as Prefect (there's one more software, but i don't recall the name right now) How does Composer compare to Vertex AI Pipelines, Kubeflow Pipelines, Dataflow etc? When would one use one over the other? Can they be used together?**

blogpost: [Kubeflow Pipelines vs. Cloud Composer for Orchestration](https://datatonic.com/insights/kubeflow-pipelines-cloud-composer-data-orchestration/)

## Core Concepts of Apache Airflow

**DAG**: A DAG is a collection of the tasks you want to run, represented by the nodes of the graph, organized in such a way that reflects the relationships and dependencies, represented by the edges of the graph. In Airflow, we use a Python SDK to define the DAGs, the task and dependencies, as code. Every DAG has:
- a definition, 
- operators, 
- and definitions of the operator relationships

**DAG Run**: A DAG run is a physical instance of that DAG, containing task instances that run for a specific execution date. The execution date is the logical date and time that the DAG run and its task instances are running for. The Airflow scheduler, which is managed by Composer within a Kubernetes pod, will often create the DAG runs. However, they can also be created by external triggers. DAG can have multiple runs, even concurrently, each with different execution dates.

**Tasks**: Tasks define a unit of work in your workflow. What are these tasks? They are parameterized implementations of operators that we will discuss in the next section

**Operators**: are usually, but not always, atomic. This means that they can work alone and they don't need to share resources with any other operators. The DAG will make sure that operators run in the correct order. 
Other than those dependencies, operators do generally run independently. In Airflow you can leverage XComms if you do need to pass information between different operators. Airflow supports **three main types** of operators:
- operators that perform an action or tell another system to perform an action
- transfer operators that move data from one system to another, for example, BigQuery to Cloud storage
- sensors that will keep running until a specific criterion is met, such as a file sensor that waits until a file is present at a certain location before triggering any downstream task.

Some concrete examples are DatflowOp, DataprocOp, BigQueryOp etc.

**Task and operator example:**
![](images/Pasted%20image%2020230408111320.png)

Airflow DAG is defined in a Python script. The scripts have **five main sections**
![](images/Pasted%20image%2020230408111040.png)

And _any code outside of the DAG definition will then be run every second_, so keep that in mind.

Setting Op dependency / order:
![](images/Pasted%20image%2020230408111446.png)

**Creating Cloud Composer Environmnet:**
Now that we have a DAG ready to go, how do we create and access our environment to run it? We can access the composer environment via the Google Cloud Console, Google Cloud SDK CLI, or via rest APIs.

## Continuous Training Pipelines using Cloud Composer (Stages 1-4: Data)

Now that we know how to create our DAGs using the Airflow Python SDK Creator Composer environment and access the Airflow web server. Let's focus on what we need to work on for building continuous training pipelines in Airflow and running them on Cloud Composer.

**There are 6 stages:**
![](images/Pasted%20image%2020230408111747.png)

![](images/Pasted%20image%2020230408111832.png)
![](images/Pasted%20image%2020230408111852.png)
![](images/Pasted%20image%2020230408111923.png)
![](images/Pasted%20image%2020230408111957.png)
![](images/Pasted%20image%2020230408112043.png)

## Continuous Training Pipelines using Cloud Composer (Stages 5-6: Model)
After the data is prepared, we're ready to start training our model. 
If we're using a TensorFlow, XGboost, or scikit-learn model, we can train directly on AI platform

![](images/Pasted%20image%2020230408112204.png)
![](images/Pasted%20image%2020230408112232.png)
![](images/Pasted%20image%2020230408112245.png)
![](images/Pasted%20image%2020230408112351.png)
![](images/Pasted%20image%2020230408112444.png)

## Apache Airflow, Containers, and TFX
How we can **use Airflow and Cloud Composer to orchestrate container based workloads** and TFX pipelines? 

**01. For container workloads:**
If we have tasks with non-PyPI dependencies, or if the tasks are already containerized, we can run the containers as Airflow task.
![](images/Pasted%20image%2020230408112740.png)
**02. For TFX Pipelines:**
If you have already written your ML pipeline using TFX, then you don't need to do much. TFX includes an Airflow DAG runner for running your TFX pipelines via Airflow.
![](images/Pasted%20image%2020230408112814.png)

## Lab - Continuous Training Pipelines with Cloud Composer
In this lab you will learn how to write an Airflow DAG for continuous training and deploy the DAG within a Cloud Composer environment. You will also learn how to explore and monitor your DAG runs using the Apache Airflow webserver.

![](images/Pasted%20image%2020230408112940.png)

#### Task 1. Provision Google Cloud resources

Enable ML services, 
`gcloud services enable ml.googleapis.com`

create buckets, 
```bash
export BUCKET_NAME=${DEVSHELL_PROJECT_ID}
export REGION=us-central1
export ZONE=us-central1-a
gsutil mb -l ${REGION} gs://${BUCKET_NAME}
```
PubSub topics, (why?)
`gcloud pubsub topics create chicago-taxi-pipeline`

Create a BigQuery dataset for storing preprocessed data for training and a table for storing training metrics
```bash
bq mk -d chicago_taxi_ct
bq mk --table chicago_taxi_ct.model_metrics version_name:STRING,rmse:FLOAT

```

and Cloud Composer environment
```bash

gcloud composer environments create demo-environment \
    --location $REGION \
    --zone $ZONE \
    --python-version 3 \
    --image-version composer-1.20.8-airflow-1.10.15

```

#### Task 2. Write DAG in Apache Airflow

We will be working through the Python script defining our continuous training DAG in Airflow.
The [Apache Airflow Concepts](https://airflow.apache.org/docs/stable/concepts.html) and [Google Cloud Operators](https://airflow.apache.org/docs/stable/howto/operator/gcp/index.html) documentation may be helpful in the following exercises.

**TODO 1**: Define basic DAG params e.g. name and args

**TODO 2**: Define SQL queries for extracting the training and validation datasets

This uses BigQuery. We read data and store it in relevant BQ tables.
We are creating two Ops, one for train and the other for test.

**TODO 3**: use the python callable `set_new_version_name` to set an Airflow Variable

**TODO 4**: Using the query (`model_check_sql`) , finish defining the task `bq_check_rmse_query_op` to ensure that the model meets our threshold of an RMSE of 10.0 by defining the `pass_value` argument appropriately. Set the `task_id` to `bq_value_check_rmse_task`

**TODO 5**: Correct ordering. The `bq_check_data_op` should have four downstream tasks: `publish_if_failed_check_op`, `python_new_version_name_op`, `bq_train_data_op`, `bq_valid_data_op`. Define these dependencies to finish the Python script.

#### Task 3. Run DAG in Apache Airflow
In this task you will copy your newly completed DAG into a Cloud Storage bucket, which will be automatically synced with your Cloud Composer environment. Afterwards you will check that your DAG was loaded correctly and start a DAG run.

Check if env is ready: Navigation > Big Data > Composer

Define Airflow vars:

#### Task 4. Exploring your DAG run in the Airflow UI

![](images/Pasted%20image%2020230611121946.png)

![](images/Pasted%20image%2020230611122018.png)

![](images/Pasted%20image%2020230611122044.png)

# 07. ML Pipelines with MLflow

MLflow is composed of four components: tracking, projects, models, and registry.

![](Pasted%20image%2020230407120111.png)

## MLFlow Tracking

For reproducibility, MLflow enables users to log the particular source code that was used to produce a model, along with this version by integrating tightly with Git, to map every model to a particular commit hash.

MLflow provides support for flexible text and notes associated with the training session. For example, notes might be a good place to drop some information about the business use case that our model is being developed for.

The MLflow tracking service back end is divided into **two components**.
1. Entity or Metadata store
2. Artifact store

![](Pasted%20image%2020230407120915.png)

## MLFlow Projects

MLflow's solution to the reproducibility challenge is a self-contained Project Specification, that bundles all of the machine learning training code with this version library dependencies, its configurations, and its training and test data.

![](Pasted%20image%2020230407121525.png)
![](Pasted%20image%2020230407122440.png)

## MLFlow Models

MLflow models is the general purpose model format that supports different production environments. It can be used to load and deploy models produced in a variety of different ways and written in varioud ML frameworks.

Similar to a project, an MLflow model is also a directory structure. It contains a configuration file and instead of containing training goals, this time it contains a serialized model artifact. It also contains (like a project does) a set of dependencies for reproducibility. This time, we are talking about evaluation dependencies in the form of a Conda environment. Additionally, MLflow provides model creation utilities for serializing models from a variety of popular frameworks in MLflow format. Finally, MLflow introduces deployments, APIs for produce theorizing and deploying any MLflow model to a variety of services.

![](Pasted%20image%2020230407122646.png)
![](Pasted%20image%2020230407122713.png)
We can load a model via different 'flavors':
![](Pasted%20image%2020230407122739.png)

## MLFlow Model Registry

MLflow Model Registry is a centralized model store and a UI and set of APIs that enables you to manage the full lifecycle of MLflow models.

![](Pasted%20image%2020230407123413.png)

As a developer, you log your model into the registry (it works with any type of model supported by mlflow). And other people now can go to this model and either manually review this model or plug in automated tools and use the API. Then, downstream users can, in a safe way, pull the latest model after it has been reviewed and confirmed to work.

![](Pasted%20image%2020230407123718.png)


