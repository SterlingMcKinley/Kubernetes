# Redis-Challenge

Project and source code by [Vladimir Mukhin](https://github.com/vladimirmukhin/redis-challenge.git) (Click the link to see repo/files)

In this project, the tasks will be to:

* Utilize a web application with a page saying, "This is the X visitor", where X is a counter fetched from Redis that gets incremented on every page visit. 

* Containerize the application and run it on Kubernetes.

Prerequistes:
* Linux

* Docker

* Minikube ( Lightweight version of a Kubernetes cluster virtually running locally)

# Steps:

1- Start Minikube

    minikube start

2- Create Redis deployment

    kubectl create deploy redis --image=redis
    
3- Verify the Redis container is running

    kubectl get pods

4- Expose the cluster IP service
    
    kubectl expose deploy redis --port=6379

5- Establish connection from local to the cluster IP service

    kubectl port-forward service/redis 6379:6379

6- Review and Reference source code. Understand what the code is doing.

7- Build and containerize the application
  
  * Execute evaluate command to build the image directly in Minikube
    
    eval $(minikube -p minikube docker-env)
    
  * Builds Python-Server image
  
    docker build . -t python-server:latest
    
8- Create a deployment for Python-Server
    
    kubectl create deployment python-server --image=python-server

9- Verify the python-Server container is running

    kubectl get pods

**** ISSUE Python-Server container is status is "Not Ready" due to "ErrImagePull" error. See Issue section for more information & resolution.



10- Expose the cluster IP service for Python-Server
    
    kubectl expose deploy python-server --port=8080
    
11- Establish connection from local to the cluster IP service via specified port
    
    kubectl port-forward service/python-server 8080:8080
