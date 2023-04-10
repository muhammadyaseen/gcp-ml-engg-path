# ML Pipelines on Google Cloud

**Synopsis**: In this course, you will be learning from ML Engineers and Trainers who work with the state-of-the-art development of ML pipelines here at Google Cloud. The first few modules will cover about TensorFlow Extended (or TFX), which is Google’s production machine learning platform based on TensorFlow for management of ML pipelines and metadata. You will learn about pipeline components and pipeline orchestration with TFX. You will also learn how you can automate your pipeline through continuous integration and continuous deployment, and how to manage ML metadata. Then we will change focus to discuss how we can automate and reuse ML pipelines across multiple ML frameworks such as tensorflow, pytorch, scikit learn, and xgboost. You will also learn how to use another tool on Google Cloud, Cloud Composer, to orchestrate your continuous training pipelines. And finally, we will go over how to use MLflow for managing the complete machine learning life cycle.

# Introduction to TFX Pipelines

# Pipeline orchestration with TFX

# Custom components and CI/CD for TFX pipelines

# ML Metadata with TFX

# Continuous Training with multiple SDKs, KubeFlow & AI Platform Pipelines

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

# Continuous Training with Cloud Composer

## What is Cloud Composer
![](Pasted%20image%2020230407125159.png)
![](Pasted%20image%2020230407125239.png)
![](Pasted%20image%2020230407125518.png)

**Airflow serves similar role as Prefect (there's one more software, but i don't recall the name right now) How does Composer compare to Vertex AI Pipelines, Kubeflow Pipelines, Dataflow etc? When would one use one over the other?**

blogpost: [Kubeflow Pipelines vs. Cloud Composer for Orchestration](https://datatonic.com/insights/kubeflow-pipelines-cloud-composer-data-orchestration/)
## Core Concepts of Apache Airflow

**DAG**: A DAG is a collection of the tasks you want to run, represented by the nodes of the graph, organized in such a way that reflects the relationships and dependencies, represented by the edges of the graph. In Airflow, we use a Python SDK to define the DAGs, the task and dependencies, as code. Every DAG has:
- a definition, 
- operators, 
- and definitions of the operator relationships

**DAG Run**: A DAG run is a physical instance of that DAG, containing task instances that run for a specific execution date. The execution date is the logical date and time that the DAG run and its task instances are running for. The Airflow scheduler, which is managed by Composer within a Kubernetes pod, will often create the DAG runs. However, they can also be created by external triggers. DAG can have multiple runs, even concurrently, each with different execution dates.

**Tasks**: Tasks define a unit of work in your workflow. What are these tasks? They are parameterized implementations of operators that we will discuss in the next section

**Operators**: are usually, but not always, atomic. This means that they can work alone and they don't need to share resources with any other operators. The DAG will make sure that operators run in the correct order. Other than those dependencies, operators do generally run independently. In Airflow you can leverage XComms if you do need to pass information between different operators. Airflow supports three main types of operators:
- operators that perform an action or tell another system to perform an action
- transfer operators that move data from one system to another, for example, BigQuery to Cloud storage
- sensors that will keep running until a specific criterion is met, such as a file sensor that waits until a file is present at a certain location before triggering any downstream task.

Task and operator example:
![](images/Pasted%20image%2020230408111320.png)

Airflow DAG is defined in a Python script. The scripts have five main sections
![](images/Pasted%20image%2020230408111040.png)

And any code outside of the DAG definition will then be run every second, so keep that in mind.

Setting Op dependency / order:
![](images/Pasted%20image%2020230408111446.png)

Now that we have a DAG ready to go, how do we create and access our environment to run it? We can access the composer environment via the Google Cloud Console, Google Cloud SDK CLI, or via rest APIs.


## Continuous Training Pipelines using Cloud Composer (data)

Now that we know how to create our DAGs using the Airflow Python SDK Creator Composer environment and access the Airflow web server. Let's focus on what we need to work on for building continuous training pipelines in Airflow and running them on Cloud Composer.

There are 6 stages:
![](images/Pasted%20image%2020230408111747.png)

![](images/Pasted%20image%2020230408111832.png)
![](images/Pasted%20image%2020230408111852.png)
![](images/Pasted%20image%2020230408111923.png)
![](images/Pasted%20image%2020230408111957.png)
![](images/Pasted%20image%2020230408112043.png)

## Continuous Training Pipelines using Cloud Composer (model)
After the data is prepared, we're ready to start training our model. If we're using a TensorFlow, XGboost or scikit-learn model, we can train directly on AI platform

![](images/Pasted%20image%2020230408112204.png)
![](images/Pasted%20image%2020230408112232.png)
![](images/Pasted%20image%2020230408112245.png)
![](images/Pasted%20image%2020230408112351.png)
![](images/Pasted%20image%2020230408112444.png)

## Apache Airflow, Containers, and TFX
How we can use Airflow and Cloud Composer to orchestrate container based workloads and TFX pipelines? If we have tasks with non-PyPI dependencies, or if the tasks are already containerized, we can run the containers as Airflow task
![](images/Pasted%20image%2020230408112740.png)
If you have already written your ML pipeline using TFX, then you don't need to do much. TFX includes an Airflow DAG runner for running your TFX pipelines via Airflow.
![](images/Pasted%20image%2020230408112814.png)

## Lab - Continuous Training Pipelines with Cloud Composer

![](images/Pasted%20image%2020230408112940.png)

#### Task 1. Provision Google Cloud resources

Enable ML service, create buckets, PubSub topics, and Cloud Composer environment

#### Task 2. Write DAG in Apache Airflow


#### Task 3. Run DAG in Apache Airflow

Navigation > Big Data > Composer

#### Task 4. Exploring your DAG run in the Airflow UI

# ML Pipelines with MLflow

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


