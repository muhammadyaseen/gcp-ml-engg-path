# ML Pipelines on Google Cloud

**Synopsis**: In this course, you will be learning from ML Engineers and Trainers who work with the state-of-the-art development of ML pipelines here at Google Cloud. The first few modules will cover about TensorFlow Extended (or TFX), which is Google’s production machine learning platform based on TensorFlow for management of ML pipelines and metadata. You will learn about pipeline components and pipeline orchestration with TFX. You will also learn how you can automate your pipeline through continuous integration and continuous deployment, and how to manage ML metadata. Then we will change focus to discuss how we can automate and reuse ML pipelines across multiple ML frameworks such as tensorflow, pytorch, scikit learn, and xgboost. You will also learn how to use another tool on Google Cloud, Cloud Composer, to orchestrate your continuous training pipelines. And finally, we will go over how to use MLflow for managing the complete machine learning life cycle.

# Introduction to TFX Pipelines

# Pipeline orchestration with TFX

# Custom components and CI/CD for TFX pipelines

# ML Metadata with TFX

# Continuous Training with multiple SDKs, KubeFlow & AI Platform Pipelines

# Continuous Training with Cloud Composer

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


