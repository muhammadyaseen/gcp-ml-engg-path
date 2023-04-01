# Quest: Build & Deploy ML Solutions with Vertex AI


## Lab # 1 - Vertex AI: Quick Start 

Same as done in [quest-foundational-ml-ai-tasks]

## Lab # 2 - Identify Damaged Car Parts with Vertex AutoML Vision

![](Pasted%20image%2020230329181728.png)

**Synopsis**: AutoML Vision helps anyone with limited Machine Learning (ML) expertise train high quality image classification models. In this hands-on lab, you will learn how to produce a custom ML model that automatically recognizes damaged car parts.

#### Task 1. Upload training images to Cloud Storage
Model will learn to classify five different damaged car parts: **bumper**, **engine compartment**, **hood**, **lateral**, and **windshield**.
1. we create a cloud storage bucket
2. copy images from a public bucket into our created bucket

#### Task 2. Create a dataset

We will create a new dataset and connect it to our training images to allow Vertex AI to access them. Normally, we need to create a CSV file where each row contains a URL to a training image and the associated label for that image. In this case, the CSV file has been created already; we just need to update it with our bucket name and upload the CSV file to our Cloud Storage bucket.

**Creating a managed dataset**: Vertex AI > Datasets > Create (requires name and dataset type)
Once created, we can add data to it, or point it to an existing data store e.g. Cloud Storage
**Connect dataset to training images**: Import files from Cloud Storage > Select .csv file (this will cause the images to be downloaded)

#### Task 3. Inspect images

Examine the images to ensure there are no errors in your dataset.

#### Task 4. Train your model

Train New Model > AutoML > then define train params and budget

We can also enable Explainability here.

#### Task 5. Request a prediction from a hosted model

To request predictions from the model, you will send predictions to an endpoint inside of your project that will forward the request to the hosted model and return back the output. Sending a prediction to the AutoML Proxy is very similar to the way that you would interact with your model you just created, so you can use this as practice.

**Get the name of AutoML proxy endpoint**:
1. Cloud Run > automl-proxy 
2.  Copy the **URL** to the endpoint. It should look something like: `https://automl-proxy-xfpm6c62ta-uc.a.run.app`.
**Create a prediction request**
3. Cloud Shell > Open Editor > New File > Then paste the code which has base64 encoded image. > Save file as `payload.json`
4. Next, set the following environment variables
```
AUTOML_PROXY=<automl-proxy url>
INPUT_DATA_FILE=payload.json
```
5. Perform a API request to the AutoML Proxy endpoint to request the prediction from the hosted model.
```
curl -X POST -H "Content-Type: application/json" $AUTOML_PROXY/v1 -d "@${INPUT_DATA_FILE}"
```

We used a bit of Cloud Run in this lab but **What is Cloud Run?**
Cloud Run is a managed compute platform that lets you run containers directly on top of Google's scalable infrastructure. You can deploy code written in any programming language on Cloud Run if you can build a container image from it. In fact, building container images is optional. If you're using Go, Node.js, Python, Java, .NET Core, or Ruby, you can use the [source-based deployment](https://cloud.google.com/run/docs/deploying-source-code) option that builds the container for you, using the best practices for the language you're using. Cloud Run allows developers to spend their time writing their code, and very little time operating, configuring, and scaling their Cloud Run service. You don't have to create a cluster or manage infrastructure in order to be productive with Cloud Run.

## Lab # 3 - Deploy a BigQuery ML Customer Churn Classifier to Vertex AI for Online Predictions

![](Pasted%20image%2020230330134248.png)

**Synopsis:** In this lab, you will go one step further and focus on how Vertex AI extends BigQuery ML's capabilities through online prediction so you can incorporate both customer churn predictions into decision making UIs such as [Looker dashboards](https://looker.com/google-cloud) but also online predictions directly into customer applications to power targeted interventions such as targeted incentives.

In this lab, you will train, tune, evaluate, explain, and generate batch and online predictions with a BigQuery ML XGBoost model. You will use a Google Analytics 4 dataset from a real mobile application, Flood it!, to determine the likelihood of users returning to the application. You will generate batch predictions with your BigQuery ML model as well as export and deploy it to **Vertex AI** for online predictions using the Vertex Python SDK.

![](Pasted%20image%2020230330134216.png)

#### Task 1. Enable Google Cloud services
Via gcloud command on Cloud Shell
#### Task 2. Deploy Vertex Notebook instance
Vertex AI workbench NB
#### Task 3. Clone the lab repository
#### Task 4. Create a BigQuery dataset
Command line
#### Task 5. Create a BigQuery ML XGBoost churn propensity model
#### Task 6. Evaluate your BigQuery ML model
#### Task 7. Batch predict user churn with your BigQuery ML model

After having trained and tested the model, we 'extract' it and store the assets in GCS Bucket. We then create an Endpoint to deploy the Model. This endpoint uses the GCS bucket and makes it available as an online prediction service.

This is what the bucket folder looks like:
![](Pasted%20image%2020230331172755.png)

## Lab # 4 - Vertex Pipelines: Quick Start

Pipelines help you automate and reproduce your ML workflow. Vertex AI also includes a variety of MLOps products, like Vertex Pipelines. In this lab, you will learn how to create and run ML pipelines with Vertex Pipelines.

**Why are ML pipelines useful?**

Why you would want to use a pipeline? Imagine you're building out a ML workflow that includes processing data, training a model, hyperparameter tuning, evaluation, and model deployment. Each of these steps may have different dependencies, which may become unwieldy if you treat the entire workflow as a monolith. 
As you begin to scale your ML process, you might want to share your ML workflow with others on your team so they can run it and contribute code. Without a reliable, reproducible process, this can become difficult. **With pipelines, each step in your ML process is its own container**. This lets you develop steps independently and track the input and output from each step in a reproducible way. You can also schedule or trigger runs of your pipeline based on other events in your Cloud environment, like when new training data is available.

![](Pasted%20image%2020230401114059.png)

#### Task 1. Create a Vertex Notebooks instance
#### Task 2. Vertex Pipelines setup
##### Step 1: Create Python notebook and install libraries
##### Step 2: Set your project ID and bucket
##### Step 3: Import libraries
##### Step 4: Define constants
#### Task 3. Creating your first pipeline
##### Step 1 - Create a Python function based component
##### Step 2: Create two additional components
##### Step 3: Putting the components together into a pipeline
##### Step 4: Compile and run the pipeline
#### Task 4. Creating an end-to-end ML pipeline
##### Step 1: A custom component for model evaluation
##### Step 2: Adding Google Cloud pre-built components
##### Step 3: Compile and run the end-to-end ML pipeline
##### Step 4: Comparing metrics across pipeline runs

## Lab # 5 - Challenge Lab


