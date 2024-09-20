# NVIDIA GPU Support

This page assumes you have installed Docker as described in the [Installation](./installation.md) page.

## Installation

To use NVIDIA GPUs with Docker, follow [the official docs](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) to install [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/index.html) and configure with Docker.

As a prerequisite, you need to install the driver for NVIDIA GPUs. You can use the following commands to [install the driver for Ubuntu](https://ubuntu.com/server/docs/nvidia-drivers-installation):

```sh
sudo ubuntu-drivers list
sudo ubuntu-drivers install
# Alternatively, install a specific driver version, such as:
#     sudo ubuntu-drivers install nvidia:535
nvidia-smi
```

To [install and configure NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) for Ubuntu, you can use the following commands:

```sh
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

!!! note "`/etc/docker/daemon.json`"
    The `nvidia-ctk` command simply modifies the Docker configuration file at `/etc/docker/daemon.json`. Alternatively, you can edit that file manually.

!!! note "`nvidia-container-toolkit` vs. `nvidia-container-runtime` vs. `nvidia-docker2` vs. `nvidia-docker` vs. `nvidia-docker-plugin`"
    NVIDIA officially recommends using `nvidia-container-toolkit` to configure Docker for NVIDIA GPU support. For normal users, this is the only package you need to install. For more details on the available packages, please refer to [this post](https://github.com/NVIDIA/nvidia-docker/issues/1268). For the deprecated packages, please refer to [this post](https://web.archive.org/web/20230420012010/https://github.com/NVIDIA/nvidia-docker/wiki/nvidia-docker) and [this post](https://web.archive.org/web/20230329061235/https://github.com/NVIDIA/nvidia-docker/wiki/nvidia-docker-plugin).

!!! note "What's installed Inside vs. Outside the container"
    Only the NVIDIA Driver is installed on the host machine, while the CUDA Toolkit, cuDNN, and other libraries are installed inside the container. For instance, in the [PyTorch NGC image](https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/rel-24-08.html):

    * NVIDIA Driver (on host machine)
    * NVIDIA CUDA (inside container)
    * NVIDIA cuDNN (inside container)
    * Python (inside container)
    * PyTorch (inside container)

## Verify installation

If user added to docker group, [run a sample workload](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/sample-workload.html):

```sh
docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
```

Otherwise, if user not added to docker group:

```sh
sudo docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
```
