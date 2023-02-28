| Abkürzung | Explanation |
| ----|----|
| QFD | quoted from documentation / Google's own tutorial |


## Compute Engine VM
- You can provide a Docker image which will then be started on the VM after boot. For ex, you can run an `nginx:latest1` container on the VM and access it via public IP. The limitation is that you can only start ONE container on the VM after boot. (Can we start more manually, after SSHing? -- should check)
- When SSHing into the VM, you first connect to host OS, then use `docker attach` to hook into the running container.

## Vertex AI Workbench

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
After clicking 'New Notebook', you have to select an environment.

![](Pasted%20image%2020230228102606.png)

![](Pasted%20image%2020230228110231.png)

Can specify your own Docker container

![](Pasted%20image%2020230228111210.png)
