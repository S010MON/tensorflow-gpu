# Tensorflow GPU setup

## 1. Introduction


## 2. Host Machine 
This guide assumes you have a linux machine with a GPU with drivers installed.  Different distros have varying support 
and drivers available, I have personally found Pop!_OS to work very well as they provide an ISO with NVIDIA support 
inbuilt, for Ubuntu, the apt repository holds drivers for most modern GPUs. You can test your setup using the command:
```
sudo docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
```
>source: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/sample-workload.html

If this runs on your host machine, then it is time to install docker and set up the docker container toolkit from NVIDIA.

## 3. Install Docker
If you already have docker installed on your machine, skip to section 4. Otherwise, follow the official installation 
instructions for your distro here: https://docs.docker.com/engine/install/ubuntu/ 

Note: although some distros allow you to install using 
a repository (such as APT for debian based distros) **it is strongly recommended that you use the official method**

### 3.1 Post installaion
If you hate having to type `sudo` before every docker command, follow the 
[post install steps](https://docs.docker.com/engine/install/linux-postinstall/) to add docker to your user group.


## 4. NVIDIA Container Toolkit
The container toolkit allows the container to talk to the GPU, follow the instructions: 
https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html provided by NVIDIA.

## 5. Clone this repository
```bash
git clone https://github.com/S010MON/tensorflow-gpu
```

## 6. Run Docker Compose
Navigate to the top level directory of the repo you just cloned, and run:
```bash
docker compose up
```
This should start the process of downloading the image and building the container.  If you have an older version of 
docker, you may need to use a hypen in the command `docker-compose up`.  It may take some time for the first set-up, but 
will be much faster next time as all the steps are cached by docker and only the final changes are re-run.  


### 6.1. Jupyter Notebooks
You should end up with this, use the links to access the notebooks
```bash
tensorflow-gpu  | [I 12:14:38.101 NotebookApp] Serving notebooks from local directory: /tf
tensorflow-gpu  | [I 12:14:38.101 NotebookApp] Jupyter Notebook 6.5.3 is running at:
tensorflow-gpu  | [I 12:14:38.101 NotebookApp] http://e81742778f1c:8888/?token=4e10a373c039e1a178f9c688ad4c504fad3d9bcfc48cd831
tensorflow-gpu  | [I 12:14:38.101 NotebookApp]  or http://127.0.0.1:8888/?token=4e10a373c039e1a178f9c688ad4c504fad3d9bcfc48cd831
tensorflow-gpu  | [I 12:14:38.101 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
tensorflow-gpu  | [C 12:14:38.103 NotebookApp] 
tensorflow-gpu  |     
tensorflow-gpu  |     To access the notebook, open this file in a browser:
tensorflow-gpu  |         file:///root/.local/share/jupyter/runtime/nbserver-1-open.html
tensorflow-gpu  |     Or copy and paste one of these URLs:
tensorflow-gpu  |         http://e81742778f1c:8888/?token=4e10a373c039e1a178f9c688ad4c504fad3d9bcfc48cd831
tensorflow-gpu  |      or http://127.0.0.1:8888/?token=4e10a373c039e1a178f9c688ad4c504fad3d9bcfc48cd831
```

### 6.2 Python Scripts
Sometimes it's more convenient to run python scripts