| Abkürzung | Explanation |
| ----|----|
| QFD | quoted from documentation / Google's own tutorial |
| NB | notebook |


## Compute Engine VM
- You can provide a Docker image which will then be started on the VM after boot. For ex, you can run an `nginx:latest1` container on the VM and access it via public IP. The limitation is that you can only start ONE container on the VM after boot. (Can we start more manually, after SSHing? -- should check)
- When SSHing into the VM, you first connect to host OS, then use `docker attach` to hook into the running container.

## Vertex AI Workbench

There are 2 types of notebooks. You have to select a `region` to create NBs.
#### Managed Notebooks

[Google Cloud documentation on Managed Notebooks](https://cloud.google.com/vertex-ai/docs/workbench/managed/introduction)

- They cost about 10x more than user-manged notebooks
- **Custom container**: You can provide a Docker image which containers jupyter kernels and those will be imported and made available. QFD "Create and access additional custom Jupyter kernels by providing your own custom docker images. All available Jupyter kernels on the container will be imported.". Ther are excellent [details on how to do this and requirements](https://cloud.google.com/vertex-ai/docs/workbench/managed/custom-container) for custom containers in the doc
- QFD "Managed notebooks instances are prepackaged with JupyterLab and have a preinstalled suite of deep learning packages, including support for the TensorFlow and PyTorch frameworks"
- **GPU support**: QFD "Managed notebooks instances support GPU accelerators and the ability to sync with a GitHub repository". Cost rises significantly of course.
- When you create a managed notebooks instance, it is deployed as a Google-managed virtual machine (VM) instance.
- QFD "Managed notebooks instance includes common data science framework environments. You can also add your own custom container images to your managed notebooks instance." You're greeted with the following when you open jupyter lab.
![](Pasted%20image%2020230228102731.png)
- You can specify what compute resources your code will run on (can configure vCPUs/GPUs/RAM) form the notebook UI, no need to recreate the instance. This can come in handy to quickly switch from prototyping to running on full data.
- We can access services like Google Cloud Build, Big Query, AI Platform etc. from the notebook cells as we have seen previously in the 2 courses.
- You can run multiple notebooks in the instance, all using various kernels. So it is better to think of the "instance" as a machine hosting your 'project', where you have various notebooks for data exploration and cleaning, model building, pipelines etc.
![](Pasted%20image%2020230228103236.png)
- **Strorage**: When you create an instance, you also attach a Storage with it (min. 100GB Standard Persistent Storage). This is persistent storage. Which means that your notebooks (and other data) will persist, even after you have shutdown the notebook and 'Stopped' the instance. When you 'start' the instance again, you will find all your previous work there. You'll be charged for the storage of course. So make sure to delete the instance once done. In the following I created `my-test-nb` on first run, then stopped the instance, started it again, and created `my-pytorch-nb`.
![](Pasted%20image%2020230228103325.png)
- For your stopped instance, you will see an 'Open Jupyterlab' link
![](Pasted%20image%2020230228103901.png)

#### User-managed Notebooks
- After clicking 'New Notebook', you have to select an environment, so it is not completely bare bones. You get basic packages installed depending on the env you choose.
![](Pasted%20image%2020230228102606.png)

![](Pasted%20image%2020230228111210.png)
- As can be seen, the selection of Kernels is very limited compared to the managed notebook. Some of this stuff may be mitigated by installing relevant packages in the boot script etc.
![](Pasted%20image%2020230228110231.png)
- **Storage and GPU**: You can still attach a GPU and get a persistant storage which keeps your notebooks saved.
- You can specify your own Docker container. [Details here](https://cloud.google.com/vertex-ai/docs/workbench/user-managed/custom-container)
- **Imp difference**: Most imp difference when compared to managed notebooks is tht you CANNOT change hardware config (cpu,ram,gpu) on-the-fly. You'll have to create a new instance.

## Vertex AI Pipelines
https://codelabs.developers.google.com/vertex-pipelines-intro#0


## Deploying a FastAPI + Streamlit app on a Google Compute Engine Instance using docker-compose

1. Create a Compute Engine VM, make sure to enable HTTP + HTTPs access while creating it.
2. Log into the VM via SSH and install `docker` on it. You can use the steps from [official Docker guide for installation on Debian OS](https://docs.docker.com/engine/install/debian/) for this.
3. Next, create an Artifact repository, this will be used to store container images. It can be done in the cloud console or via `gclound`. In case of gcloud, the command is as follows 
```bash
gcloud artifacts repositories create $ARTIFACT_REGISTRY_NAME \
--repository-format=docker \
--location=$REGION \
--description="Artifact registry description"
```
5. Enable Google Cloud Build API for your project. This will also setup a required Service Account that will be required by the Cloud Build service to access Cloud Storage. Detail info on the SA can be found [here](https://cloud.google.com/build/docs/cloud-build-service-account)
![](images/Pasted%20image%2020230506160400.png)
7. Now we have to build our containers. For this, on your local machine where you have the source code, change into the directory which contains the Dockerfile and run 
```bash
gcloud builds submit . \
--tag <region>-docker.pkg.dev/<project_name>/<artifcat reg name>/image_name:tag
```
This will start the building process and push the container to the repo you created earlier.
9. Give the CE Service Account principal the additional of Artifact Registry Writer so that we can push and pull containers from the repo after authentication.
![](images/Pasted%20image%2020230506160334.png)
11. Now, we need to log in to the VM and pull our containers. For that, we first have to authenticate with the AR on our CE instance using an account that has read/write permissions on our AR. You can create a key for the CE service account (that we granted AR Writer role earlier) and download it to ur local PC (IAM and Admin > Service Accounts > Add Key). Then do `touch key.json` and then `vim key.json` then paste the content of the file into the repo. Detailed instructions for repo auth are [here](https://cloud.google.com/artifact-registry/docs/docker/authentication#json-key) e.g. `cat key.json | docker login -u _json_key --pasword-stdin https://europe-west3-docker.pkg.dev`
12. Once authenticated, we can pull the images. But we don't need to do so directly. We can simply write a docker compose file with `image` property and then use `docker compose up` which will automatically pull the images and start the containers. 
13. You will have to use `sudo docker` to run commands otherwise you'll get `permission denied` errors.
14. Congratulations now you have your app running on a VM instance.



