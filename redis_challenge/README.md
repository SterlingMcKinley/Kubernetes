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

**** ISSUE Python-Server container status is "Not Ready" due to "ErrImagePull" error. This issue is a poteneital blocker...See Issue section for more information & resolution.


![errimagepull](https://user-images.githubusercontent.com/91057035/211167087-3712f918-4839-4d20-956f-0b67bbdd320b.png)


10- Expose the cluster IP service for Python-Server
    
    kubectl expose deploy python-server --port=8080
    
11- Establish connection from local to the cluster IP service via specified port
    
    kubectl port-forward service/python-server 8080:8080

12- Check the application from web browser via localhost IP & specified port -> 127.0.0.1:8080

See screenshot of application & Kubernetes Dashboard displaying running connections/pods


![app](https://user-images.githubusercontent.com/91057035/211167219-35728919-d17d-4d6f-b1b9-2ebf9fbdc0bf.png)


![dashboard](https://user-images.githubusercontent.com/91057035/211167212-23d3510f-58b3-4eae-b158-d2832d5a6238.png)


# Issue(s)

Python-Server container status is "Not Ready" due to "ErrImagePull" error. This issue occurs due to Kubernetes configured to ALWAYS pull image from a remote registry.

Resolution: Edit the Kubernetes deployment-python-server.yaml file, update the "imagePullPolicy" configuration from Always to IfNotPresent. This will allow Kubernetes to look for the image locally before searching the remote regristry for an image. Once the file is updated, another python-server pod will start and the faulty/not ready pod will be terminated.

Steps:

On the command-line, execute the below command:
   
    kubectl edit deployment python-server
    
The deployment-python-server.yaml file will appear in vi editor mode
* locate the configuration "imagePullPolicy" 
* update the "imagePullPolicy" configuration from Always to IfNotPresent
    

![resolution](https://user-images.githubusercontent.com/91057035/211167559-aa214de1-4d46-436d-9174-5e7f16e2e6aa.png)
