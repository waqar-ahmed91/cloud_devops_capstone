# Cloud DevOps Capstone Project

This is a udacity cloud devops engineer capstone project to deploy an app using jenkins pipeline for continous integration and delivery of the project. All the tasks like build, linting, docker image building, docker pushing, deployment and cleaning performed using jenkins pipeline.

I used the machine learning microservices app that can predict the house prices of Boston using sklearn.

### Deployment Type

**Rolling deployment** type is used for this project as it is faster than blue/green deployment while it slowly replaces the previous version of the application with the new one across all the servers.

### Cluster Creation

AWS **EKSCTL** is used to create the cluster which uses cloudformation process to create the cluster stack. **1.18 version** has been used with **t2.medium** node type having **maximum three nodes** and **minimum one**. I also used t2.small node type with 1.17 version but that was not work out well for the app deployment because of not supported version and node type.

# To Run the App Locally Before Deploying it on the Cloud:

### Setup the Environment

- Create a virtualenv (e.g. python3 -m venv ~/.devops) and activate it (e.g. source ~/.devops/bin/activate)
- Run `make install` to install the necessary dependencies
- Run `make lint` to lint the files

### Running `app.py`

1. Standalone: `python app.py`
2. Run in Docker: `./run_docker.sh`
3. Run in Kubernetes: `./run_kubernetes.sh`

### Kubernetes Steps

- Setup and Configure Docker locally
- Setup and Configure Kubernetes locally
- Create Flask app in Container
- Run via kubectl
