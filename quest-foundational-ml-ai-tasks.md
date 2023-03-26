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

We need to create Service Accounts so that the resources we need can authenticate and communicate with each other and have the right permissions. We need to give this SA access to:
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

## Lab # 2 - Dataprep: Qwik Start

#### Task 1. Create a Cloud Storage bucket in your project
#### Task 2. Initialize Cloud Dataprep
#### Task 3. Create a flow
#### Task 4. Import datasets
#### Task 5. Prep the candidate file
#### Task 6. Wrangle the Contributions file and join it to the Candidates file
#### Task 7. Summary of data
#### Task 8. Rename Columns

## Lab # 3 - Dataflow: Qwik Start - Templates
## Lab # 4 - Dataflow: Qwik Start - Python
## Lab # 5 - Dataflow: Qwik Start - Console or Cmdline
## Lab # 6 - Cloud Natural Language API: Qwik Start
## Lab # 7 - Cloud Speech API: Qwik Start
## Lab # 8 - Video Intelligence: Qwik Start
## Lab # 9 - Perform Foundational Data, ML, and AI Tasks in Google Cloud: Challenge Lab



