# Quest: Perform Foundational Data, ML, and AI Tasks in Google Cloud

![VertexAI High Level Architecture](Pasted%20image%2020230326105304.png)

## Lab # 1 - Vertex AI: Qwik Start

**Synopsis**: 
- Use BigQuery for data processing and EDA
- Use VertexAI to train and deploy custom TensorFlow Regression model to predict customer lifetime value (CLV)
- We will start with a local BQ and TF workflow, and progress toward training and deploying the model in the cloud with Vertex AI

**What you'll learn**:
![](Pasted%20image%2020230326105717.png)

#### Task 1. Enable Google Cloud services

First, we Activate Cloud Shell so that we can send commands to Google Cloud. Then, using `gcloud`, we enable the required services / APIS e.g. IAM, monitoring, AI Platform / Vertex AI, Cloud Build etc.

#### Task 2. Create Custom Service Accounts

We need to create [Service Accounts](https://cloud.google.com/iam/docs/service-account-overview) so that the resources we need can authenticate and communicate with each other and have the right permissions. We need to give this SA access to:
- Cloud Storage for writing and retrieving Tensorboard logs
- BigQuery data source to read data into TensorFlow model
- Vertex AI for running model training, deployment, and explanation jobs

This can be accomplished via Cloud Shell i.e. `gcloud` or UI, We will use `gcloud` cmd line.

#### Task 3. Launch Vertex AI Workbench notebook

We create a Vertex AI Workbench notebook instance with TensorFlow Enterprise environment.
It can be understood as a VM with TF and Jupyter Notebook support.

#### Task 4. Clone the lab repository

We open our Notebook instance and then we clone a repo which has the necessary starting code and resources to complete this lab. [GCP - training data analyst repo](https://github.com/GoogleCloudPlatform/training-data-analyst)
We get the following after cloning.
![](Pasted%20image%2020230326113539.png)


#### Task 5. Install lab dependencies

We install the dependencies specified in the `requirements.txt` file.
Then, open the notebook and follow the instructions given in it.

Essentially, we:
- Download a `.xlsx` dataset from internet and convert it to `.csv`
- Creata Bucket for our BQ dataset
- Create a BQ dataset and load csv data into it
- Use BQ queries to load data into pandas dataframes
- Create a simple baseline model in BQ, using SQL stat functions (could have also used BQ ML)
- Pass those dfs to TensorFlow to create a DNN Regressor model
Up to this point, we have a 'local' workflow, now we will work towards making it 'cloud native'
- adssad
- a
- a
- a
https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/self-paced-labs/vertex-ai/vertex-ai-qwikstart/lab_exercise_long.ipynb

## Lab # 2 - Dataprep: Qwik Start

#### Task 1. Create a Cloud Storage bucket in your project

Created bucket `myaseen-qwik-lab2-dataprep`

#### Task 2. Initialize Cloud Dataprep
Accept service agreements and so on.
#### Task 3. Create a flow
#### Task 4. Import datasets
#### Task 5. Prep the candidate file
#### Task 6. Wrangle the Contributions file and join it to the Candidates file
#### Task 7. Summary of data
#### Task 8. Rename Columns

Tasks 3-8 we done in Dataprep UI.

We can use the service to read data and apply transformations and joins while visually inspecting the data. After we are done, we can run the job and store it back into the cloud storage.
It has a DSL called Wrangler to apply transformations to columns and data.

## Lab # 3 - Dataflow: Qwik Start - Templates or Python

**Synopsis**: In this lab, you will learn how to create a streaming pipeline using one of [Google's Cloud Dataflow templates](https://cloud.google.com/dataflow/docs/templates/provided-templates). More specifically, you will use the Cloud Pub/Sub to BigQuery template, which reads messages written in JSON from a Pub/Sub topic and pushes them to a BigQuery table.

In this lab, we have a choice of either using Console (UI) or Cloud Shell (Cmdline) to complete the tasks. I chose cmdline, bcz imo it makes things more clear.

#### Task 1. Create a Cloud BigQuery dataset and table Using Cloud Shell
Can use `bg mk` command
#### Task 2. Run the pipeline
We can use the Template and only need to specify PubSub topic and Output table.
The template takes care of the rest.

![](Pasted%20image%2020230326150402.png)
This is what the template is doing:
![](Pasted%20image%2020230326164506.png)

#### Task 3. Submit a query
Once data starts being populated in the BQ Table, we can query against it.

#### Task 4. Test your understanding
- Dataflow supports both stream and batch workflows, here we worked with Pub/Sub topic to BigQuery -- which is a stream dataflow.
- Dataflow is a distribution of Apache Beam

The Python version of this lab allows us to submit jobs from the local machine e.g. a container image. But that didn't work because their authentication mode was broken.

**Synopsis**: In this lab you will set up your Python development environment, get the Cloud Dataflow SDK for Python, and run an example pipeline using the Cloud Console

## Lab # 4 - Dataproc: Qwik Start - Console or Cmdline

Cloud Dataproc is a fast, easy-to-use, fully-managed cloud service for running [Apache Spark](http://spark.apache.org/) and [Apache Hadoop](http://hadoop.apache.org/) clusters.

We need Dataproc API enabled for this.

**Synopsis**: This lab shows you how to use the Google Cloud Console to create a Google Cloud Dataproc cluster, run a simple [Apache Spark](http://spark.apache.org/) job in the cluster, then modify the number of workers in the cluster.

#### Task 1. Create a cluster

We can easily create a cluster over either Compute Enginer or GKE. Here we're using GCE.
We can specify machine types for Master and Worker nodes. If we want we can later also scale the server up and down e.g. change the number of worker nodes.
![](Pasted%20image%2020230326164557.png)
#### Task 2. Submit a job
We can submit a job against the spawned cluster. Here we are running Spark example to compute Pi.
![](Pasted%20image%2020230326165606.png)

#### Task 3. View the job output
Once the job has been submitted we can see output and details in the Job Details tab.
![](Pasted%20image%2020230326165629.png)

## Lab # 5 - Cloud Natural Language API: Qwik Start
Cloud Natural Language API lets you extract information about people, places, events, (and more) mentioned in text documents, news articles, or blog posts. Can be used for sentiment and intent analysis.

Cloud Natural Language API features: syntax analysis, Entity Recognition, Sentiment Analysis, Content Classification (pre-define categories), Multi-Language, Integrated REST API.

**Synopsis**: In this lab you'll use the `analyze-entities` method to ask the Cloud Natural Language API to extract "entities" (e.g. people, places, and events) from a snippet of text.

#### Task 1. Create an API key

Console: Go to API & Services and Create and API key. This need so that we can access resources within our project.

Cmdline: 
1. Get the project ID: `export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)`
2. Create Service Account to access NL API: `gcloud iam service-accounts create my-natlang-sa --display-name "my natural language service account"`
3.  create credentials to log in as your new service account and save them in json file. 
```
gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
```
4. Set env var that points to the key. 
`export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"`

#### Task 2. Make an Entity Analysis request

To make a request, a Compute Engine instance has been provision already. (why can't we make request directly from Cloud Shell?). We SSH into the instance and run:
```
gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'." > result.json
```


## Lab # 6 - Cloud Speech API: Qwik Start

The Google Cloud Speech API enables easy integration of Google speech recognition technologies into developer applications. The Speech API allows you to send audio and receive a text transcription from the service.

**Synopsis**:
Learn to create an API Key, Create and Call Speech API request

#### Task 1. Create an API key
Go to API & Services and Create and API key. This need so that we can access resources within our project.

#### Task 2. Create your Speech API request
Create a json file with required format.
#### Task 3. Call the Speech API
Query the SPeech API endpoint and provide the json file as data. It returns a json response

## Lab # 7 - Video Intelligence: Qwik Start
## Lab # 8 - Perform Foundational Data, ML, and AI Tasks in Google Cloud: Challenge Lab



