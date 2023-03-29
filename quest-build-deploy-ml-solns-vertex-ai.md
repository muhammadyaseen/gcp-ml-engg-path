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

## Lab # 4 - Vertex Pipelines: Quick Start

## Lab # 5 - Challenge Lab


