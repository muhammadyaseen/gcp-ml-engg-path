| Abkürzung | Explanation |
| ----|----|
| QFD | quoted from documentation / Google's own tutorial |


## Compute Engine VM
- You can provide a Docker image which will then be started on the VM after boot. For ex, you can run an `nginx:latest1` container on the VM and access it via public IP.
- When SSHing into the VM, you first connect to host OS, then use `docker attach` to hook into the running container.

## Vertex AI Workbench

#### Managed Notebooks

[Google Cloud documentation on Managed Notebooks](https://cloud.google.com/vertex-ai/docs/workbench/managed/introduction)

- They cost about 10x more than user-manged notebooks
- You can provide a Docker image which containers jupyter kernels and those will be imported and made available. QFD "Create and access additional custom Jupyter kernels by providing your own custom docker images. All available Jupyter kernels on the container will be imported."
- QFD "Managed notebooks instances are prepackaged with JupyterLab and have a preinstalled suite of deep learning packages, including support for the TensorFlow and PyTorch frameworks"
- QFD "Managed notebooks instances support GPU accelerators and the ability to sync with a GitHub repository"
- When you create a managed notebooks instance, it is deployed as a Google-managed virtual machine (VM) instance.
- QFD "Managed notebooks instance includes common data science framework environments. You can also add your own custom container images to your managed notebooks instance. These environments are available as kernels that you can run your notebook file in. When you run a notebook in one of the kernels, Vertex AI Workbench starts the corresponding container, creates a Jupyter session on it, and uses that Jupyter session to run your notebook on the container."

#### User-managed Notebooks
