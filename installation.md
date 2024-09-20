# Installation

The installation steps below assumes you are using a Linux distribution. If you are using Windows or macOS, you may need to use [Docker Desktop](https://docs.docker.com/desktop/) to run Docker Engine in a virtual machine.

Although the GUI provided by Docker Desktop may be more user-friendly than CLI, this guide does not cover the use of Docker Desktop, as it does not support certain advanced features. Feel free to try out Docker Desktop first, and then come back to this guide later when you are ready to use the CLI.

## Installation

To install [Docker Engine](https://docs.docker.com/engine/), follow [the official docs](https://docs.docker.com/engine/install/) for your distribution. For Ubuntu, you can use the following commands in [the official docs](https://docs.docker.com/engine/install/ubuntu/):

```bash
# Uninstall old versions
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install the Docker packages
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

The commands above also installs [the Buildx plugin](https://docs.docker.com/build/architecture/#buildx) and [the Compose plugin](https://docs.docker.com/compose/install/linux/).

!!! note "Docker Engine vs. Docker Desktop"
    Docker Desktop is a tool that includes a virtual machine (VM), a graphical user interface (GUI), and extra features, with the Docker engine running inside the VM. See [this post](https://forums.docker.com/t/difference-between-docker-desktop-and-docker-engine/124612) for more info.

!!! note "`docker-ce` vs. `docker.io`"
    For normal users, I recommend installing `docker-ce` for simplicity, as `docker.io` might not be the latest version in distributions that have reached the end of their life (EOL). However, if you are experienced and know what you are doing, using `docker.io` is also feasible. For more info, see [this post](https://stackoverflow.com/a/57678382).

!!! note "`docker-compose-plugin` vs. `docker-compose`"
    Use `docker-compose-plugin` (Compose V2, with `docker compose` command) instead of `docker-compose` (Compose V1, with `docker-compose` command) since it is [deprecated](https://docs.docker.com/compose/migrate/#can-i-still-use-compose-v1-if-i-want-to). See [this post](https://stackoverflow.com/a/66516826) for more info.

## Docker without sudo

!!! failure "The `docker` group is `root` equivalent"
    Only add trusted user to the `docker` group, as it is equivalent to root access. See the warning section in [this post](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user) for more info.

To run Docker commands without `sudo`, add your user to the `docker` group by following commands in [this post](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user):

```bash
# Create the docker group
sudo groupadd docker
# Add your user to the docker group
sudo usermod -aG docker $USER
# Activate the changes to groups
newgrp docker
```

## Verify installation

If user added to docker group, run:

```sh
docker run hello-world
```

Otherwise, if user not added to docker group:

```sh
sudo docker run hello-world
```

## NVIDIA GPU Support

Follow the [NVIDIA GPU Support](./nvidia-gpu-support.md) page.

## Uninstall

!!! failure "All Docker data will be lost"
    Only run the commands below if you are sure you want to uninstall Docker and remove all images, containers, and volumes.

```sh
# Uninstall packages
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
# Remove all images, containers, and volumes
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
