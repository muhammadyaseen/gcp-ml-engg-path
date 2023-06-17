[My Google Cloud Skill Boost Public Profile](https://www.cloudskillsboost.google/public_profiles/d85f8295-b522-4522-964c-f0fcf9375090)

# MLOps Fundamentals

# 01. When and Why do we need MLOps ?

**ML froms Ops POV**: 
- Everything from defining the problem to scaling the solution
- MLOps is a lifecycle management discipline for ML
- Continuous Integration + Delivery + Training

It attempts to answer Data Scientist's challenges:
- keeping track of models, code, metrics, experiment runs
- reproducibility, tracebility, re-training

![](images/Pasted%20image%2020230208163146.png)
![](images/Pasted%20image%2020230208163752.png)
![](images/Pasted%20image%2020230208163853.png)

# 02. Understanding the Main K8s components

## Intro to Containers
**Containers**: isolated user space (above kernel) for running application
Microservices = loosely coupled fine grained applications
Docker = create and build containers
K8s = Tools for orchesterating containers on a large scale
Google has a container registery: `gcr.io`. We can pull public containers from here and save our pvt containers here that will be organized under our project

**Google Cloud Build**: Can be used to build container images and deliver them to Cloud Functions, GKE, App Engine for execution. We can use the command `gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/<container-name> <Dockerfile dir>` or also provide a YAML file and use `gcloud builds submit --config cloudbuild.yaml .`. 
Once the%20image%20has been build it will be avaibale in the Container Registery. The true power of custom build configuration files (like YAML) is their ability to perform other actions, in parallel or in sequence, in addition to simply building containers: running tests on your newly built containers, pushing them to various destinations, and even deploying them to Kubernetes Engine. So it can act as a CD system.

## Intro to K8s

How can you manage your container infrastructure better? e.g. if the containers need to find each other and connect to each other etc.

Kubernetes is an open source platform that helps you orchestrate and manage your container infrastructure on-premises or in the Cloud. It automates the deployment, scaling, load balancing, logging, monitoring, and other management features of containerized applications.

Kubernetes supports declarative configurations, which means that you describe the desired state you want to achieve instead of issuing a series of commands to achieve that desired state. Kubernetes' job is to make the deployed system conform to your desired state and then keep it there in spite of failures. Kubernetes also allows imperative configuration in which you issue commands to change the system state, but for large container environments it becomes unscalable.

K8s supports both stateful and stateless applications. It also supports batched jobs and daemon tasks. Kubernetes can automatically scale in and out containerized applications based on resource utilization

![](images/Pasted%20image%2020230209135844.png)

## Intro to Google K8s Engine (GKE)

- Google cloud's managed service offering for Kubernetes is called GKE.
- It helps you deploy, manage, and scale K8s environments for your containerized applications on GCP. GKE is a component of the GCP compute offerings, it makes it easy to bring your Kubernetes workloads into the cloud.
-  The VMs that host your containers inside of a GKE cluster are called nodes
- GKE has auto-repair with request draining. Just K8s supports scaling workloads, GKE supports scaling the cluster itself.
- GKE seamlessly integrates with Google Cloud's Build and Container Registry. This allows you to automate deployment using private container images that you've securely stored in Container Registry.

## Compute Options 

![](images/Pasted%20image%2020230209140353.png)

## K8s Concepts

2 related concepts:
- K8s Object Model
- Principle of declarative management

**K8s object model**: Each thing Kubernetes manages is represented by an object, and you can view and change these objects, attributes, and state.
**Declarative mgmt**: K8s expects you to tell it what you want the state of the objects under its management to be, and it will work to bring that state into being and keep it there.

How is it accomplished: Watch-loop

##### K8s Object
Formally, a Kubernetes object is defined as a persistent entity that represent the state of something running in a cluster: it's desired state and its current state.

Kubernetes objects have two important element:
- Object spec: desired state
- Object status: current state described by k8s
Each object is of a certain 'Kind'.

##### Pod
A 'Pod' is the smallest deployable unit. A Pod embodies the environment where containers live and that environment can accommodate one or more containers. If there is more than one container in a Pod, they are tightly coupled and they share resources including networking and storage. Kubernetes assigns each Pod a unique IP address. Every container within a Pod shares the network namespace including IP address and network ports. Containers within the same Pod can communicate through localhost 127.0.0.1. A Pod can also specify a set of storage volumes to be shared amongst it's containers. Pods are not self-healing.

![](images/Pasted%20image%2020230209142408.png)


## K8s Control Plane
In a K8s cluster..
- One computer is called the control plane and the others are simply called nodes.
- Job of nodes is to run Pods
- The job of the control plane is to coordinate the entire cluster.

**Control Plane Components**:
- `kube-api-server` (view / change state of server). kube-APIserver also authenticates incoming requests, determines whether they are authorized and valid, and manages admission control.
- `etcd` is the cluster database. Its job is to reliably store the state of the cluster
- `kube-scheduler`: responsible for scheduling pods onto nodes. But it doesn't do the work of actually launching the pods on the notes. 
- `kube-controller-manager` :  It continuously monitors the state of the cluster through kubeAPIserver. Whenever the current state of the cluster doesn't match the desired state, kube-controller-manager will attempt to make changes to achieve the desired state
- `Kube-cloud manager` manages controllers that interact with the underlying cloud providers.
- Each node runs a small family of control plane components too. For example, each node runs a `kubelet`. You can think of a kubelet as Kubernetes agent on each node
- `Kube-proxy`'s job is to maintain the network connectivity among the pods in the cluster
![](images/Pasted%20image%2020230209143532.png)

## GKE Concepts
Node pools
Zonal vs Regional clusters
Control plane is managed by Google.
Control plane can manage and deploy nodes in the form of Compute Engine VMs

## Deployments
Deployments describe a desired state of Pods. For example, a desired state could be that you want to make sure that you have five nginx Pods running at all times.

Its declarative stance means that Kubernetes will continuously make sure this configuration is running across your cluster.

## Ways to Create Deployments
You can create a Deployment in three different ways

Services and Scaling
Updating Deployments
Rolling Updates
Blue-Green Deployments
Canary Deployments
![](images/Pasted%20image%2020230209160859.png)

Managing Deployments
Jobs and CronJobs
Parallel Jobs
CronJobs

# 03. Intro to AI Platform Pipelines

## 03.01 - Overview
Covers the basics of AI platform pipelines, a Google Cloud product to build ML Pipelines, and how it is different from the regular ML Ops pipelines. AI Platform Pipelines makes ML Ops easy, seamless and scalable with Google Cloud Services.

We will also discuss what is the technical stack behind this product and what is the ecosystem around it that makes it very collaborative and scalable.

![](images/Pasted%20image%2020230223133008.png)

![](images/Pasted%20image%2020230223133053.png)
![](images/Pasted%20image%2020230223133112.png)
![](images/Pasted%20image%2020230223133132.png)

## 03.02 - Intro to AI Platform Pipelines
![](images/Pasted%20image%2020230223133350.png)
Pipeline consists of various **components**. Pipeline components are self-contained sets of code that perform one step in a pipeline's workflow, such as data pre-processing, data transformation, model training, and so on. Components are composed of a set of input parameters, a set of outputs, and the location of a container image. Each task in a pipeline performs a step in the pipeline's workflow.

![](images/Pasted%20image%2020230223133735.png)

![](images/Pasted%20image%2020230223134017.png)

Traditional process for building MLOps pipelines consists of setting up a cluster. In GCP, it will be Google Kubernetes Engine. Then we need a Cloud Storage bucket to store the data, followed by installing Kubeflow pipelines. Then we need to configure it and set up the port forwarding. Finally, we need to create a process to share the pipeline with your team or organization. This is a very long step and it takes lot of time. Sometimes missing any one step can cause errors and confusion. This raises a question: **Can we automate this whole process to make MLOps a seamless and easy experience**? Can we make it one-click deploy?

This is the question services by AI Platform Pipelines. It is a product to deploy robust, repeatable machine learning pipelines, along with monitoring, auditing, version tracking, and reproducibility. Al Platform Pipelines provides seamless integration with Google Cloud managed services such as BigQuery, Dataflow, AI Platform Training and Serving, and Cloud Functions.

**Al Platform Pipelines has two major parts**:
First, the enterprise-ready infrastructure for deploying and running structured ML workflows that are integrated with Google Cloud services
The second is pipeline tools for building, debugging, and sharing pipelines and its components.

## 03.03 -  AIP Pipelines: Concepts

Tech stack behind pipelines. TFX is focused on TensorFlow while Kubeflow is generic.

![](images/Pasted%20image%2020230223134819.png)

## 03.04 -  When to use
![](images/Pasted%20image%2020230223140305.png)
![](images/Pasted%20image%2020230223140408.png)
## 03.05 -  Ecosystem

![](images/Pasted%20image%2020230223140550.png)
## 03.06 - Lab: Running AI Platform Pipelines
Prebuilt pipeline is available in the lab / account. We just have to run it.

AI Platform > Pipelines > New Instance > Configure > Create New Cluster > Deploy

![](Pasted%20image%2020230615092600.png)
# 04. Training, Tuning, and Serving on AI Platform

## 04.01 - System and Concept Overview
![](images/Pasted%20image%2020230224162605.png)
Specify the training configurations, such as the hyperparameter ranges to be tuned, into a `config.yaml` file.

When developing an ML model, developers and data scientists usually develop most of their code on Jupyter Notebooks. An AI Platform Notebook, is a configurable Jupyter Notebook server on AI Platform.

**This is a typical flow**: Load the training data from BigQuery. You can then store the training files, which have been already transformed, in Cloud Storage. Package the training code into a train.py file. This code will be pushed, as a Docker image, into Container Registry. 
Trigger the training on AI Platform Training. AI Platform also stores the training artifacts (such as the trained model) in Cloud Storage.
Deploy the trained model using AI Platform Prediction, so the model can be served. AI Platform Prediction does that by retrieving the saved model from Cloud Storage and deploying it as an API.

We will cover how to automate and CI/CD this process in later modules

## 04.02 - Create a reproducible dataset

Getting a random sample from BigQuery / SQL data 
![](images/Pasted%20image%2020230224165115.png)
... but this won't be reproducible as we will get different rows every time. We can use some kind of hashing and binning to solve this issue. Hashes are deterministic, so it will be repeatable but still random. We should use a field that is not correlated with target var otherwise we will be throwing away important information.

![](images/Pasted%20image%2020230224165230.png)

from command line.. create a split
![](images/Pasted%20image%2020230224165307.png)
... then store result in Cloud Storage
![](images/Pasted%20image%2020230224165434.png)

## 04.02 -  Implement a tunable model
AI platform training performs both training and tuning for us. The built-in  hyperparameter tuning feature of AI platform will help us with this. We only need a few changes to our existing workflow to benefit from these advantages.

1. Make the hyperparam a cmd line argument
2. Set-up `cloudml-hypertune` to record training metrics
3. Export the final trained model
4. Suppy hyperparams to the training job

![](images/Pasted%20image%2020230224182523.png)
![](images/Pasted%20image%2020230224182538.png)
![](images/Pasted%20image%2020230224182556.png)
![](images/Pasted%20image%2020230224182610.png)

## 04.04 -  Build and push a training container
At this point, we know how to write the `train.py` and the `config.yaml` files, which will contain the training code and the training configuration respectively. The last stage before we can train the model is to wrap our training container into a Docker image. That's what we'll look at now.

The docker file is shown in the%20image%20following. Any additional arguments passed to the command that executes the container run command will be forwarded to the `ENTRYPOINT` command as command line arguments. That's how you pass the hyperparameter values, to our training container.

Then we'll need to build and push the container%20image%20(using `gcloud builds submit` cmd), to Container Registry, from which AI platform training we'll retrieve it during training. Every Google Cloud project has its own attached Container Registry.

![](images/Pasted%20image%2020230224182901.png)

## 04.05 -  Train and Tune a model
So we have pushed our training container to Container Registry. Now we need to actually trigger the training so that we can train and tune the model. Let's see how we can do that.

We can execute our trainer package locally. After we ensure everything is working, we use the gcloud command to submit the training job to AI Platform. For this, First, you need to provide the URI of the container image. Then you need to provide a path to the config.yaml file, which contains the information about which hyperparameters to tune and which range of values to try for each of them.
![](images/Pasted%20image%2020230224195414.png)
After you launch the command from the JupyterLab, training will start. After the training job is complete, the console will display the various runs with both their hyperparameters and performance metrics. You'll see the different trials (or runs) of the training, with the different parameters that have been trained, along with the target metric.
![](images/Pasted%20image%2020230224195440.png)
Now that we know what the best values for the hyperparameters that we tuned are, our goal is to train a model with these hyperparameter values, save the model, and deploy it to start getting predictions. There are two ways to do that:
1. We can just look at the values on the console and enter them manually when starting a new training job
2. we can do that programmatically using SDK

SDK method:
![](images/Pasted%20image%2020230224195610.png)

this time, make sure you omit the --config argument. Instead, you'll pass the actual best parameter values through the command line after the backslash argument
![](images/Pasted%20image%2020230224195706.png)
The trained model will then be saved to Cloud Storage at the end of the run
## 04.06 -  Serve and Query a model

Now that we know how to train into and models using AI platform. Let's see how to deploy them as rest APIs so you can send predictions from the model. AI platform prediction retrieves the trained model and saves it as a pkl in cloud storage. It can then be deployed as a REST API that can be query from anywhere, including the Jupiter lab notebook.

Deploying a model requires 2 steps. 
First we need to create a model object. For that, we use the command `gcloud  ai-platform models create` and assign a name to the model. 
![](images/Pasted%20image%2020230224200233.png)
Next, we run a similar command, which is `gcloud ai-platform versions create` to create a version of the model. At this point, you tie the actual model saved as a pkl on some cloud storage bucket to the version through the origin argument, which is the path to where the model pkl is located.
![](images/Pasted%20image%2020230224200305.png)

Finally, the last step of our lesson is to set a prediction request that can be done easily through the command `gcloud ai-platform predict`. For the prediction, it is necessary to specify both the model in its version which were created previously.
![](images/Pasted%20image%2020230224200400.png)
## 04.07 - Lab: Using Custom Containers with AI Platform Training

**Synopsis:** In this lab, you develop a multi-class classification model, package the model as a Docker image, and run the model on AI Platform Training as a training application. The training application trains a multi-class classification model that predicts the type of forest cover from cartographic data. The dataset used in the lab is based on the Covertype Data Set from the UCI Machine Learning Repository.
The code is instrumented using the hypertune package. Therefore, it can be used with an AI Platform hyperparameter tuning job to search for the best combination of hyperparameter values by optimizing the metrics you specify.

![](Pasted%20image%2020230615094314.png)

We can create a k8s cluster and then deploy AI Platform pipelines instance into it which installs the relevant pipeline related software on it e.g. KFP sdk etc.

#### Task 1: Enable Cloud services

```bash
export PROJECT_ID=$(gcloud config get-value core/project)
gcloud config set project $PROJECT_ID

gcloud services enable \
cloudbuild.googleapis.com \
container.googleapis.com \
cloudresourcemanager.googleapis.com \
iam.googleapis.com \
containerregistry.googleapis.com \
containeranalysis.googleapis.com \
ml.googleapis.com \
dataflow.googleapis.com

# Editor permission for your Cloud Build service accoun

PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format="value(projectNumber)")
CLOUD_BUILD_SERVICE_ACCOUNT="${PROJECT_NUMBER}@cloudbuild.gserviceaccount.com"
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member serviceAccount:$CLOUD_BUILD_SERVICE_ACCOUNT \
  --role roles/editor
```

#### Task 2: Create an instance of AI Platform Pipelines 

AI Platform > Pipelines > New Instance > Configure > Create New Cluster > Deploy

Or do this
```bash
gcloud container clusters create cluster-1 --zone us-central1-a --release-channel stable --machine-type n1-standard-2 --scopes=https://www.googleapis.com/auth/cloud-platform
```

Then go to AI Platform > Pipelines > New Instance > Configure > (select this cluster) > Deploy

#### Task 3: Create an instance of Vertex AI Platform Notebooks 

The instance is configured using a custom container image that includes all Python packages required for this lab.

```bash
# In Cloud shell

cd
mkdir tmp-workspace
cd tmp-workspace

gsutil cp gs://cloud-training/OCBL203/requirements.txt .

# Create a Dockerfile that defines your custom container image:
gsutil cp gs://cloud-training/OCBL203/Dockerfile .  

# Build the image and push it to your project's Container Registry:
IMAGE_NAME=kfp-dev
TAG=latest
IMAGE_URI="gcr.io/${PROJECT_ID}/${IMAGE_NAME}:${TAG}"
gcloud builds submit --timeout 15m --tag ${IMAGE_URI} .
```

**Creating a Notebook Instance from cmd line:**

```bash
ZONE=us-central1-a
INSTANCE_NAME=ai-notebook

IMAGE_FAMILY="common-container"
IMAGE_PROJECT="deeplearning-platform-release"
INSTANCE_TYPE="n1-standard-4"
METADATA="proxy-mode=service_account,container=$IMAGE_URI"
gcloud compute instances create $INSTANCE_NAME \
  --zone=$ZONE \
  --image-family=$IMAGE_FAMILY \
  --machine-type=$INSTANCE_TYPE \
  --image-project=$IMAGE_PROJECT \
  --maintenance-policy=TERMINATE \
  --boot-disk-device-name=${INSTANCE_NAME}-disk \
  --boot-disk-size=100GB \
  --boot-disk-type=pd-ssd \
  --scopes=cloud-platform,userinfo-email \
  --metadata=$METADATA

```

Then Open the Notebook

#### Task 4: Clone the mlops-on-gcp repo within your Vertex AI Platform Notebooks instance

`git clone https://github.com/GoogleCloudPlatform/mlops-on-gcp`


#### Task 5: Navigate to the mlops-on-gcp notebook

Go to **mlops-on-gcp > on_demand > kfp-caip-sklearn > lab-01-caip-containers > exercises >lab-01.ipynb**.

Follow the instructions in the notebook.

The notebook is saved in `notebooks/ml-ops-course/using-custom-containers-with-ai-platform-training.ipynb`


# 05. Kubeflow Pipelines on AI Platform
## 05.01 - Intro Kubeflow
Kubeflow was developed to use Kubernetes to standardize and streamline the DevOps work around machine learning. Kubeflow pipeline can be used to automate the training and tuning process for ML models. Instead of having to trigger every single step of the process manually from the Jupiter Lab Notebook, we can trigger the entire process with a single click after we have expressed the various steps as a Kubeflow Pipeline. Kubeflow provides out of the box support to a lot of common ML frameworks like TensorFlow, PyTorch, Caffe, and XGboost.

ML processes can be split into sequence of more or less standard steps. These steps can be arranged in a pipeline of tasks that are organized into a DAG. Kubeflow is composable, portable, and standard, which means that you can create a component, reuse it in a different pipeline, and share it with your colleagues. To sum up, Kubeflow allows you to orchestrate the entire machine learning workflow. You can share and re-use the components in a very standardized way, that allows for rapid and reliable experimentation.

The **Kubeflow UI** allows you to visualize the pipeline and view the different tasks. You can click each of these tasks for more information, such as where the corresponding container is located. You can also see all the configs, and all the inputs, and outputs of each of the components.

After the tasks have been executed, some artifacts are saved. These artifacts of different characteristics. For example, if you look at the artifacts for the training component, you might see the various performance matrix, the various training curves, the RC curves, etc. We can do things like train multiple models over the same data and then deploy the best model to production.

![](images/Pasted%20image%2020230221185055.png)
![](images/Pasted%20image%2020230221185145.png)

**what does it look like to implement a Kubeflow pipeline:** We can simply do it in the notebook via Kubeflow Python SDK if all the corresponding tasks are already available as Docker images pushed to a container registry, and then compile it to a YAML pipeline description file can be uploaded to the Kubeflow cluster. We can also manually upload the YAML file via UI.  After uploading the pipeline, just enter the run parameters where the input is located, where the output is to be stored, which project it is, the Google Cloud region, where the pipeline will be executed, the location of the training data, the hyperparam values, etc. and then create the run, and the pipeline will run with these parameters.

## 05.02 - Describing a Kubeflow Pipeline with KF DSL
How to use the Python SDK to describe a Kubeflow Pipeline?
Kubeflow offers a DSL that allows you to describe in Python code, how Kubeflow tasks organize themselves into a dependency graph.
![](images/Pasted%20image%2020230221190821.png)

Name and description is available in the KF UI. 
The Arguements become the pipeline run arguements.

![](images/Pasted%20image%2020230221190924.png)
![](images/Pasted%20image%2020230221190954.png)

Some ops can also be triggered conditionally. For ex, deploying only if some threshold is met.

![](images/Pasted%20image%2020230221191032.png)

Every Kubeflow component or task coincides with a docker container being run. In practice, there are 3 different ways to create and use these components.

![](images/Pasted%20image%2020230221191200.png)

## 05.03 - Pre-built Components

Seems similar in concept to built-in GitHub Actions.
GitHub has a repo full of pre-built components. To use them, you need a URI to the `component.yaml` file.  
The components are pre-built, we only have to load and compose them. The process essentially describes where the component Container is through a URI that points to the Container Registry. The second part specifies the component run parameters.

Example of Pipeline code to load pre-built components:
![](images/Pasted%20image%2020230221203144.png)
We can init the components as follows. 
![](images/Pasted%20image%2020230221203355.png)
We can compose components as functions by using the op outputs to chain the output of one component to the input of the second.
![](images/Pasted%20image%2020230221203526.png)

##  05.04 - Lightweight Python Components
Suppose that you have two Python functions that you want to wrap into two Kubeflow components. You don't really want to write the full Dockerfiles, and build and push the Docker images into some Container Registry for just two small Python functions. The Kubeflow SDK allows you to do that easily without writing any of the containerization code. For that, we'll use the `func_ to_container_op`  helper function that we import from `kfp.components`. The helper function takes as input the function that we want to wrap into a Kubeflow component along with a base container image. Behind the scenes, it loads our Python code into this container.

Assuming we want to create components out of 2 functions `evaluate_model` and `retrieve_best_run`, defined in the `helper_components` module.

![](images/Pasted%20image%2020230221205443.png)
Now they can be used like normal components
![](images/Pasted%20image%2020230221205628.png)
The params given here are passed to the helper function

## 05.05 - Custom Components
For custom components, you write the code that prescribes the behavior of the component, along with the code that creates the container, which encapsulate all the dependencies of your code. You have to follow several steps to achieve that.
![](images/Pasted%20image%2020230221205840.png)
Use the `load_component_from_file(URI)` func to load component via the yaml file description.

## 05.06 - Compile, Upload, and Run
How to compile the pipeline, upload the result of the compilation  to the Kubeflow cluster and create a run.

1. The first step is to build and push all the containers that might be needed by the various components of the Kubeflow pipeline.
2. In step two, we build and push the base containers that are needed by all the Python lightweights ops.
3. Step three, we compile the Kubeflow pipeline, which means that we use the DSL-compile command to which we pass the Python file that contains the pipeline description. The DSL-compile command will transform the Python code describing a pipeline into a YAML file, which can then be uploaded to AI platform.
4. Step four, is to upload the pipeline to the KF cluster. We can do that from the Kubeflow UI or we can do it programmatically by using the KFP pipeline upload command line.
5. We can use the KFP run submit command to run the pipeline.

## 05.07 - Lab: Continuous Training Pipeline with Kubeflow Pipeline and Cloud AI Platform

**Synopsis:** In this lab, you build, deploy, and run a Kubeflow Pipeline (KFP) that orchestrates BigQuery and AI Platform services to train, tune, and deploy a Scikit-learn model.

![](Pasted%20image%2020230615100225.png)


#### Task 1: Create an Instance of AI Platform Pipelines
#### Task 2: Create an Instance of Vertex AI Notebook
#### Task 3: Clone the repo within Vertex AI Notebook Instance
#### Task 4: Navigate to the mlops-on-gcp notebook

Go to **mlops-on-gcp > on_demand > kfp-caip-sklearn > lab-02-kfp-pipeline > lab-02.ipynb**.

Follow the instructions in the notebook.

The notebook is saved in `notebooks/ml-ops-course/cts-training-pipeline-with-kubeflow-and-ai-platform.ipynb`

# 06. CI/CD for Kubeflow Pipelines on AI Platform

## 06.01 - Concept Overview
In last section, we saw how to build an automated Kubeflow Pipeline.

In this section, we will see how can we integrate this pipeline in a continuous integration stack? The goal is to rebuild pipeline assets immediately when new training code is pushed to the corresponding repository.

GitHub uses several triggers to start a new Container build and push. But first, we need to connect our GitHub repo to Cloud Build. Cloud Build allows us to monitor pushes to a specific branch, tags, or pull requests to trigger a rebuild. We'll discuss how to build configuration files that will tell Cloud Build what Container to rebuild based on a trigger.

Usually, after experimentation, the code is pushed to a repo, that triggers a rebuild of all the assets, which are then pushed to an artifact repository like Google Cloud Registry. The models are then retrained using the new training images that have been pushed into the registry. If the model meets the criteria, they're deployed to AI platform prediction  where the API is monitored.

##  06.02 - Cloud Build Builders

With Google Cloud, the CI/CD stack is powered by Cloud Build.
One of the main components of Cloud Build is cloud builders. These are cloud configuration or provisioning actions that are packaged as Docker containers. Now there's many cloud builder actions.

![](images/Pasted%20image%2020230222183814.png)

Now there's two types of cloud builders, standard builders and custom builders.
Standard builders are already packaged configuration actions that are really common, such as building a Docker container and pushing that Docker container to a registry. We look at how the standard cloud builders are actually implemented, we see they boil down to two parts. One, a script that executes the configuration actions. And two, a Docker container that wraps the script with all the dependencies that it needs to be executed.
With custom builders, you must write your own Docker file and configuration script. With custom builders, you need to push the container yourself into your own project container registry.

## 06.03 -  Cloud Build Configuration
The goal is to run a cloud builder whenever a trigger is detected. Well, this raises the question, how does Cloud Build know which cloud builder to run?
The answer lies in a Cloud Build configuration file. We tell Cloud Build which builders to run, in a `cloudbuild.yaml` file, which describes the cloud builder to be run, and what arguments should be passed to the entry point command, defined in the corresponding docker file.
![](images/Pasted%20image%2020230222185001.png)

How do we trigger the build itself, and execute all the build steps described in a `cloudbuild.yaml` file? Well, we simply use the `gcloud builds submit` command and pass it the `cloudbuild.yaml` file, as well as some substitution variables. 
![](images/Pasted%20image%2020230222185018.png)
![](images/Pasted%20image%2020230222185103.png)
![](images/Pasted%20image%2020230222185123.png)

## 06.04 - Cloud Build Triggers

Now that we know how to specify the configuration actions for Cloud Build and how to invoke the build manually using the gcloud command, let's see how to trigger that build from GitHub triggers.

Cloud Build can manage multiple actions on GitHub (push, tag, pull request etc). We want to configure Cloud Build to execute the build using these triggers. The first step towards this automation is to link the GitHub repo with the Google Cloud Project. The next step is to specify the Cloud Build, what action to take when one of these triggers is detected and we set this up within Cloud Build. Once done, After you've tagged your code and pushed your tag to GitHub, the build will complete. Everything is triggered automatically. 

## 06.05 - Lab: CI/CD for Kubeflow pipelines on AI Platform 

**Synopsis:** n this lab, you develop a Cloud Build CI/CD workflow that automatically builds and deploys a Kubeflow Pipeline (KFP). You also integrate your workflow with GitHub by setting up a trigger that starts the workflow when a new tag is applied to the GitHub repo that hosts the pipeline's code.

![](images/Pasted%20image%2020230617090932.png)

#### Task 1: Create an instance of AI Platform Pipelines

#### Task 2: Create an instance of Vertex AI Platform Notebooks

#### Task 3: Clone the mlops-on-gcp repo within your Vertex AI Platform Notebooks instance

`git clone https://github.com/GoogleCloudPlatform/mlops-on-gcp`

#### Task 4: Navigate to the mlops-on-gcp notebook

In the notebook interface, navigate to **mlops-on-gcp > on_demand > kfp-caip-sklearn > lab-03-kfp-cicd > exercises**, and open **lab-03.ipynb**.

The completed notebook has been saved in:  `notebooks/ml-ops-course/ci-cd-for-kubeflow-pipeline.ipynb`



