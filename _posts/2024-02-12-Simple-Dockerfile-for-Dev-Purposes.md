---
title: Simple Dockerfie for Dev Purposes
date: 2024-02-12 12:00:00 -0
categories: [Repo, Setup]
---
# Docker in Machine Learning

Dockerfiles, images, and containers are essential players in the realm of Machine Learning (ML) development, creating a foundation for consistency and reproducibility. They shape a standardized environment for ML applications, ensuring uniformity in software dependencies, libraries, and configurations. This streamlines collaboration among data scientists, engineers, and stakeholders by mitigating challenges related to environment variations and dependency conflicts. Dockerized ML environments also bolster portability, facilitating the sharing and deployment of models across diverse computing environments, from local workstations to cloud infrastructure. This promotes efficient collaboration and deployment practices in the dynamic field of machine learning.

## A Simple Dockerfile Template for Your ML Projects

Here's a straightforward Dockerfile template that suits various Machine Learning projects:

- Utilizes a base image with pre-installed Python and Conda.
- Installs basic dependencies.
- Sets up a Conda environment containing all required libraries.
- Enables seamless GitHub authentication for source control within the container.
- Uses "pip install -e ." to install the current directory as a package, allowing users to treat their utilities within the container like any other library.

The next Dockerfile was gotten from our [*llamaindex-learning repository.*](https://github.com/bubl-ai/llamaindex-learning/blob/main/docker/Dockerfile)

```
# Use an official base image with Python and Conda
FROM continuumio/anaconda3:latest

# Install additional dependencies (git, vim)
# Minimize the amount of data stored in that particular docker layer
RUN apt-get update && \
    apt-get install -y git vim && \
    rm -rf /var/lib/apt/lists/*

# Create and activate a Conda environment
RUN conda create --name myenv python=3.11 && \
    echo "conda activate myenv" >> ~/.bashrc

# Install numpy in the Conda environment
RUN conda install -n myenv numpy

# SSH for GitHub authentication
# keyscan is used to avoid manually veryfying GitHub hosts
RUN mkdir -p /root/.ssh && \
    ssh-keyscan github.com >> /root/.ssh/known_hosts

# Copy the current directory contents into the container at /llamaindex-learning
COPY . /llamaindex-learning

# Set the working directory inside the container
WORKDIR /llamaindex-learning

# Install Python dependencies using pip
RUN pip install -e . --no-binary :all:

# Specify the command to run on container startup
CMD ["/bin/bash"]
```

You can use it in VSCode to seamlessly develop your code within the container.
This is assuming that you already have Docker and VSCode installed,and that you set your GitHub ssh key in `~/.ssh/id_ed25519`. If you don't have this, feel free to check [this previous blog post!](https://bubl-ai.com/posts/Raspberry-Pi-Setup/)

The only thing missing is to build your image and run your container. This is done very easily by executing the next commands.
```bash
docker build -t [NAME_OF_IMAGE] -f Dockerfile .
docker run -t -d -v ~/.ssh/id_ed25519:/root/.ssh/id_ed25519 [Name_OF_IMAGE] /bin/bash
```

## Security Tips for Docker

Did you notice that we executed `docker run` with a volume include the ssh key? Let us explain why. When it comes to security in Docker, especially handling sensitive information like SSH private keys, caution is key. Embedding such data directly into Docker images may pose security risks, particularly if shared or made public. Best practices recommend avoiding direct embedding of sensitive information into images.

Using a volume to provide sensitive information, such as an SSH key, during runtime is a more secure approach compared to embedding it in the Docker image. Here are the advantages:

- **Avoiding Image Exposure:** By not embedding sensitive information directly, the risk of unintentional exposure is reduced, especially in shared or public images.

- **Separation of Concerns:** Storing sensitive information in a volume allows for the separation of building the image and providing runtime configurations, enhancing security and maintainability.

- **Dynamic Configuration:** With a volume, sensitive information can be changed without rebuilding the Docker image. This flexibility is valuable when updating credentials or keys without redeploying the entire application.

In the ever-evolving landscape of machine learning, ensuring a secure and adaptable Docker environment is crucial for the success of your projects.
