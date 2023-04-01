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
```bash
USER_FLAG=--user

pip3 install {USER_FLAG} google-cloud-aiplatform==1.0.0 --upgrade
pip3 install {USER_FLAG} kfp google-cloud-pipeline-components==0.1.1 --upgrade
```


##### Step 2: Set your project ID and bucket
Create vars to save project id and bucker name
```python
PROJECT_ID=...
BUCKET_NAME="gs://" + PROJECT_ID + "-bucket"
```

##### Step 3: Import libraries
Required libraries
```python

from typing import NamedTuple
import kfp
from kfp import dsl
from kfp.v2 import compiler
from kfp.v2.dsl import (Artifact, Dataset, Input, InputPath, Model, Output,
                        OutputPath, ClassificationMetrics, Metrics, component)
from kfp.v2.google.client import AIPlatformClient
from google.cloud import aiplatform
from google_cloud_pipeline_components import aiplatform as gcc_aip
```

##### Step 4: Define constants

```python
PATH=%env PATH
%env PATH={PATH}:/home/jupyter/.local/bin
REGION="us-central1"

# Cloud Storage path where the artifacts created by the pipeline will be written
PIPELINE_ROOT = f"{BUCKET_NAME}/pipeline_root/"
PIPELINE_ROOT
```


#### Task 3. Creating your first pipeline

Relevant: [ML Ops Course - AI Pipelines on GCP](ml-ops-fundamentals.md#^b72b49)

We will create a short pipeline using the KFP SDK. This pipeline doesn't do anything ML related. This pipelines prints out a sentence using two outputs: a product name and an emoji description. This pipeline will consist of three components:
1. `product_name`: This component will take a product name as input, and return that string as output. 
2. `emoji`: This component will take the text description of an emoji and convert it to an emoji. This component uses an emoji library to show you how to manage external dependencies in your pipeline
3. `build_sentence`: This final component will consume the output of the previous two to build a sentence that uses the emoji.

We will do this task in 4 steps.

##### Step 1: Create a Python function based component

Using the KFP SDK, you can create components based on Python functions. First build the `product_name` component, which simply takes a string as input and returns that string.

```python
@component(
	base_image="python:3.9", 
	output_component_file="first-component.yaml")
def product_name(text: str) -> str:
    return text
```

The `output_component_file` parameter is optional, and specifies the yaml file to write the compiled component to. After running the cell you should see that file written to your notebook instance. If you wanted to share this component with someone, you could send them the generated yaml file and have them load it with the following.

```python
product_name_component = kfp.components.load_component_from_file(
		'./first-component.yaml'
)
```


##### Step 2: Create two additional components

Create two more components. The first one takes a string as input, and converts this string to its corresponding emoji if there is one. It returns a tuple with the input text passed, and the resulting emoji:

```python
@component(packages_to_install=["emoji"])
def emoji(
    text: str,
) -> NamedTuple(
    "Outputs",
    [
        ("emoji_text", str),  # Return parameters
        ("emoji", str),
    ],
):
    import emoji
    
    emoji_text = text
    emoji_str = emoji.emojize(':' + emoji_text + ':', language='alias')
    print("output one: {}; output_two: {}".format(emoji_text, emoji_str))
    
    return (emoji_text, emoji_str)
```

-  The `packages_to_install` parameter tells the component any external library dependencies for this container.
-  This component returns a `NamedTuple` called `Outputs`. Notice that each of the strings in this tuple have keys: `emoji_text` and `emoji`. You'll use these in your next component to access the output.

Create the final final component. The final component in this pipeline will consume the output of the first two and combine them to return a string:

```python
@component
def build_sentence(
    product: str,
    emoji: str,
    emojitext: str
) -> str:
    
    print("We completed the pipeline, hooray!")
    
    end_str = product + " is "   
    end_str += emoji if len(emoji) > 0 else emojitext

    return end_str
```

**How does this component know to use the output from the previous steps you defined?** Good question! You will tie it all together in the next step.

##### Step 3: Putting the components together into a pipeline

The component definitions defined above created factory functions that can be used in a pipeline definition to create steps.
1. To set up a pipeline, use the `@dsl.pipeline` decorator, give the pipeline a name and description, and provide the root path where your pipeline's artifacts should be written. By artifacts, it means any output files generated by your pipeline. This intro pipeline doesn't generate any, but your next pipeline will
2. In the next block of code you define an `intro_pipeline` function. This is where you specify the inputs to your initial pipeline steps, and how steps connect to each other


##### Step 4: Compile and run the pipeline

This pipeline will take **5-6 minutes** to run

#### Task 4. Creating an end-to-end ML pipeline

It's time to build your first ML pipeline. In this pipeline, you'll use the UCI Machine Learning Dry Beans dataset, from: KOKLU, M. and OZKAN, I.A., (2020), "Multiclass Classification of Dry Beans Using Computer Vision and Machine Learning Techniques." This is a tabular dataset, and in your pipeline you'll use the dataset to train, evaluate, and deploy an AutoML model that classifies beans into one of 7 types based on their characteristics.

This pipeline will:
-   Create a Dataset in Vertex AI
-   Train a tabular classification model with AutoML
-   Get evaluation metrics on this model
-   Based on the evaluation metrics, decide whether to deploy the model using conditional logic in Vertex Pipelines
-   Deploy the model to an endpoint using Vertex Prediction

**Each of the steps outlined will be a component. Most of the pipeline steps will use pre-built components for Vertex AI services** via the `google_cloud_pipeline_components` library yoy imported earlier.

##### Step 1: A custom component for model evaluation

This component  will be used towards the end of the pipeline once model training has completed. This component will do a few things:
-   Get the evaluation metrics from the trained AutoML classification model
-   Parse the metrics and render them in the Vertex Pipelines UI
-   Compare the metrics to a threshold to determine whether the model should be deployed

##### Step 2: Adding Google Cloud pre-built components
##### Step 3: Compile and run the end-to-end ML pipeline
1.  With the full pipeline defined, it's time to compile it:
```python
compiler.Compiler().compile(
    pipeline_func=pipeline, package_path="tab_classif_pipeline.json"
)
```

2. Next, kick off a pipeline run:

```python
response = api_client.create_run_from_job_spec(
    "tab_classif_pipeline.json", 
    pipeline_root=PIPELINE_ROOT,
    parameter_values={"project": PROJECT_ID,
                      "display_name": DISPLAY_NAME}
)
```

Click on the link shown after running the cell above to see your pipeline in the console. If you toggle the "Expand artifacts" button at the top, you'll be able to see details for the different artifacts created from your pipeline. For example, if you click on the `dataset` artifact, you'll see details on the Vertex AI dataset that was created. You can click the link here to go to the page for that dataset. Similarly, to see the resulting metric visualizations from your custom evaluation component, click on the artifact called **metricsc**

To see the model and endpoint created from this pipeline run, go to the models section and click on the model named `automl-beans`. There you should see this model deployed to an endpoint



##### Step 4: Comparing metrics across pipeline runs

If you run this pipeline multiple times, you may want to compare metrics across runs. You can use the `aiplatform.get_pipeline_df()` method to access run metadata. Here, we'll get metadata for all runs of this pipeline and load it into a Pandas DataFrame

```python
pipeline_df = aiplatform.get_pipeline_df(
				pipeline="automl-tab-beans-training-v2"
			)
small_pipeline_df = pipeline_df.head(2)
small_pipeline_df
```

You've now learned how to build, run, and get metadata for an end-to-end ML pipeline on Vertex Pipelines.
## Lab # 5 - Challenge Lab


