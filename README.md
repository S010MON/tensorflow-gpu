# No Nonsense: Tensorflow GPU setup

## 1. Introduction
Setting up a GPU for machine learning with tensorflow can be a maze.  This is a simple setup guide that links all the
steps with a template that can be used for a quick setup.  If you want a more detailed guide on what everything is doing
check out my related blog post at leondebnath.com/no-nonsense-tensorflow-gpu.html

    1.  [Introduction](https://github.com/S010MON/tensorflow-gpu#1-introduction)
    2.  [NVIDIA Drivers (test step)](https://github.com/S010MON/tensorflow-gpu#2-nvidia-drivers-test-step)
    3.  [Installing Docker](https://github.com/S010MON/tensorflow-gpu#3-install-docker)
    4.  [NVIDIA container toolkit](https://github.com/S010MON/tensorflow-gpu#4-nvidia-container-toolkit)
    5.  [Clone template](https://github.com/S010MON/tensorflow-gpu#5-clone-this-repository)
    6.  [Run the code](https://github.com/S010MON/tensorflow-gpu#6-run-docker-compose)

Note: I've tried to link all the relevant documentation, so if Tensorflow, Docker or NVIDIA update their processes, you 
know where to find them!

## 2. NVIDIA Drivers (test step)
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

### 3.1 Post installation
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
Sometimes it's more convenient to run python scripts, this can be done by attaching to the container in the terminal 
(you will need to keep the container above running and do this in a new terminal):

Running `docker ps -a` will list all of your running containers:
```bash
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                      PORTS                                       NAMES
35f098e8e62f   tensorflow-gpu-jupyter   "bash -c 'source /et…"   14 seconds ago   Up 13 seconds               0.0.0.0:8888->8888/tcp, :::8888->8888/tcp   tensorflow-gpu
```

to execute commands in the container use the command:
```bash
docker exec -it [CONTAINER_NAME] bash
```
for example, the default name for this template is `tensorflow-gpu` so the command becomes:
```bash
docker exec -it  tensorflow-gpu bash
```
you should see this message when you attach:
```bash
________                               _______________
___  __/__________________________________  ____/__  /________      __
__  /  _  _ \_  __ \_  ___/  __ \_  ___/_  /_   __  /_  __ \_ | /| / /
_  /   /  __/  / / /(__  )/ /_/ /  /   _  __/   _  / / /_/ /_ |/ |/ /
/_/    \___//_/ /_//____/ \____//_/    /_/      /_/  \____/____/|__/


WARNING: You are running this container as root, which can cause new files in
mounted volumes to be created as the root user on your host machine.

To avoid this, run the container by specifying your user's userid:

$ docker run -u $(id -u):$(id -g) args...

root@35f098e8e62f:/tf/notebooks# 
```

Now you're ready to train on the GPU