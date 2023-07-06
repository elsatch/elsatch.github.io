---
title: "Setting up an AI workstation"
description: "meta description"
image: "images/post/setting-up-ai-workstation/setting-up-ai-workstation.jpg"
date: 2022-08-05T11:00:00+02:00
categories: ["setup"]
tags: ["#100DaysToOffload"]
type: "regular" # available types: [featured/regular]
draft: false
---
#### Introduction

In this document, I will share the  steps required to get an AI workstation machine ready. I'll be updating the content as my configuration evolves.

#### Base Operating System

My workstation will be using Ubuntu 20.04 LTS as the base operating system. I have tried the newer 22.04 LTS version too, but it includes Python 3.10 as default Python installation and I have found compatibility problems with some packages I was testing at the time [like (Great_Expectations)](https://greatexpectations.io).

For the moment given, I don't need any bleeding edge version that is only available on 22.04

#### Installation media

To make my setup as portable as possible, I will do the installation in an external SSD drive. I choose the Sandisk 1 Tb SSD (NVMe) with USB 3.0 Gen 2 connection. So far, my experience with Sandisk, in terms of reliability, has been good.

During the installation I formated the drive using this partitioning:

- Boot partition: 512 Mb
- Swap partition: 16 Gb (same size as my RAM)
- Root partition: 256 Gb (mounts as /)
- Home partition: 700 Gb aprox (mounts as /home)

My goal is being able to replace the root partition in the future, for an upgraded OS, without losing any personal data stored at /home.

#### Post-Install steps

I created an Ubuntu 20.04 LTS boot disk using [BalenaEtcher](https://www.balena.io/etcher/), then booted my laptop. Configuration options were minimal and I chose not to load any external packages for NVidia or wi-fi, following the advice from [David Adrián blogpost](https://davidadrian.cc/definitive-data-scientist-setup/)

Once system was installed, I installed the following packages:
```sh
sudo apt install net-tools
sudo apt install openssh-server
```

net-tools offers `ifconfig` command and openssh-server is a must for remote configuration.

#### Installing Lambda Stack

After checking several alternatives, I decided I will give a try to [Lambda Stack](https://lambdalabs.com/lambda-stack-deep-learning-software), a customization layer on top of Ubuntu that brings the most popular tools for Deep Learning with a simple script.

To install it, launch the script from their website:

```sh
LAMBDA_REPO=$(mktemp) && \
wget -O${LAMBDA_REPO} https://lambdalabs.com/static/misc/lambda-stack-repo.deb && \
sudo dpkg -i ${LAMBDA_REPO} && rm -f ${LAMBDA_REPO} && \
sudo apt-get update && sudo apt-get install -y lambda-stack-cuda
sudo reboot
```

After installing the Lambda stack, it's time to get a GPU accelerated Docker with this command:

```sh
sudo apt-get install docker.io nvidia-container-toolkit
```

Then you can use the [lambda stack dockerfiles following this tutorial](https://lambdalabs.com/blog/set-up-a-tensorflow-gpu-docker-container-using-lambda-stack-dockerfile/).

#### Other additional packages

After Lambda Stack, I decided to install also Miniconda3 and setting up JupyterHub as a service, following [David Adrian guide](https://davidadrian.cc/definitive-data-scientist-setup/).


##### JupyterHub as a service

```sh
conda create -n jupyter_env
conda activate jupyter_env
conda install python=3.9
conda install -c conda-forge jupyterhub jupyterlab nodejs nb_conda_kernels
sudo nano /etc/systemd/system/jupyterhub.service
sudo systemctl daemon reload
sudo systemctl start jupyterhub
sudo systemctl enable jupyterhub
```

En el interior del jupyterhub.service:

```systemd
[Unit]
Description=JupyterHub
After=network.target

[Service]
User=root
Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/home/<your_user>/miniconda3/envs/jupyter_env/bin:/home/<your_user>/miniconda3/bin"
ExecStart=/home/<your_user>/miniconda3/envs/jupyter_env/bin/jupyterhub

[Install]
WantedBy=multi-user.target
```

I still have to review how this integration works. I am particularly puzzled about this paragraph:
"The most interesting feature of this Jupyter setup is that it detects kernels in all conda environments, so you can access those kernels from here with no hassle. Just install the corresponding kernel in the desired environment (conda install ipykernel, or conda install irkernel) and restart Jupyter server from JupyterHub control panel"

##### Miniconda

```sh
wget https://repo.anaconda.com/miniconda/Miniconda3-py38_4.12.0-Linux-x86_64.sh
bash Miniconda3-py38_4.12.0-Linux-x86_64.sh
```

##### PyCharm Professional

```sh
sudo snap install pycharm-professional --classic
```

#### Alternative setups

I tried following the [Ubuntu tutorial](https://ubuntu.com/blog/ubuntu-nvidia-rapids) to install the RAPIDS NVidia stack, but it did not work for me. I got an error doing `./data-science-stack setup-system` and opted for the current alternative.

_Photo by Markus Winkler at Unsplash_
