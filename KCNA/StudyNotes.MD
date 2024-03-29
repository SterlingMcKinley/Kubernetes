EXAM components

Multiple choice - 60 questions

90 minutes


Kubernetes Fundamentals – 46%

What problem Kubernetes solves?

Scalability, Self-Healing, deploy/back out easier & faster, helps to limit buying hardware


What is container, pod, replicaset, service, deployment, configmap, secrets, daemonset and ingress

Pod - the smallest Kubernetes objects that represents an application. Pods wrapper for one or more containers, which contains not only working application but also some metadata.

Replicaset - A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.

Service - A service is responsible for enabling network access to a set of pods.
 
An abstract way to expose an application running on a set of Pods as a network service.

Deployment - A responsible for keeping a set of pods running. A Deployment provides declarative updates for Pods and ReplicaSets.

Configmap - an API object used to store non-confidential data in key-value pairs. A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable.

Secret - an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Podspecification or in a container image. Using a Secret means that you don't need to include confidential data in your application code. Secrets are similar to ConfigMaps but are specifically intended to hold confidential data.

DaemonSet - ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

•	running a cluster storage daemon on every node

•	running a logs collection daemon on every node

•	running a node monitoring daemon on every node

Ingress - Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.


What is control plane? What is data plane? What are different types of controllers in control plane?

control plane - manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

Control Plane Components
Controller manager
Cloud Controller manager
Api Server
ETCD 
Scheduler
Data Plane – also known as the application plane, is the layer that provides capacity such as CPU, memory, network, and storage so that the containers can run and connect to a network. It consists in a set of worker nodes.


What does etcd do? 

Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.


What is a cluster? 

a set of nodes that run containerized applications.

What is a node?

where the application services run

What is a name space?

namespaces provides a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. 


What is RBAC?

a method of regulating access to computer or network resources based on the roles of individual users within your organization.

What is kubectl?

controls the Kubernetes cluster manager


***What are common Kubernetes commands?***

List one or more pods
Kubectl get pod

Delete a pod 
Kubectl delete pod <pod_name>
Display K8s version
Kubectl version

Get configuration of cluster
Kubectl config view
List the api resources
Kubectl api-resources 

List everything
Kubectl get all -all-namespaces

Create a new deployment
Kubectl delete deployment <deployment_name>

Print logs for a pod
Kubectl logs <pod_name>

Print logs for a container in a pod 
Kubectl logs -c <container_name> <pod_name>



***What is YAML and JSON?***

YAML - YAML is a human-readable text-based format that lets you easily specify configuration-type information by using a combination of maps of name-value pairs and lists of items. YAML stands for yet another markup language 

JSON - a "query" language that allows us to filter and format information in environments where the amount of data is too big to be managed using the "kubectl" regular commands.




***Kubernetes Architecture***

![K8sArchitecture](https://user-images.githubusercontent.com/91057035/166108875-f5cda66c-df29-4ea0-8230-8ce05e41e6e4.png)


The components of a Kubernetes Architecture Diagram template are: 

Cloud controller manager: links your cluster to the cloud provider

Controller manager: makes global decisions about the cluster 

Etcd (persistent store): backs store for all cluster data 

Kubelet: runs each node in the cluster 

Kube proxy: network proxy that runs in each node of the cluster 

Scheduler: watches for newly created Pods with no assigned node, and selects a node for them to run on 

Control place: runs controller processes 

Node: provides Kubernetes runtime environment 

***Kubernetes API***
The core of Kubernetes' control plane is the API server. The API server exposes an HTTP API that lets end users, different parts of your cluster, and external components communicate with one another.
The Kubernetes API lets you query and manipulate the state of API objects in Kubernetes (for example: Pods, Namespaces, ConfigMaps, and Events).



***Scheduling***
In Kubernetes, scheduling refers to making sure that Pods are matched to Nodes so that Kubelet can run them.
A scheduler watches for newly created Pods that have no Node assigned. For every Pod that the scheduler discovers, the scheduler becomes responsible for finding the best Node for that Pod to run on. The scheduler reaches this placement decision taking into account the scheduling principles described below.

kube-scheduler is the default scheduler for Kubernetes and runs as part of the control plane. kube-scheduler is designed so that, if you want and need to, you can write your own scheduling component and use that instead.

kube-scheduler selects a node for the pod in a 2-step operation:
1.	Filtering
2.	Scoring
The filtering step finds the set of Nodes where it's feasible to schedule the Pod. For example, the PodFitsResources filter checks whether a candidate Node has enough available resource to meet a Pod's specific resource requests. After this step, the node list contains any suitable Nodes; often, there will be more than one. If the list is empty, that Pod isn't (yet) schedulable.
In the scoring step, the scheduler ranks the remaining nodes to choose the most suitable Pod placement. The scheduler assigns a score to each Node that survived filtering, basing this score on the active scoring rules.
Finally, kube-scheduler assigns the Pod to the Node with the highest ranking. If there is more than one node with equal scores, kube-scheduler selects one of these at random.
There are two supported ways to configure the filtering and scoring behavior of the scheduler:
1.	Scheduling Policies allow you to configure Predicates for filtering and Priorities for scoring.
2.	Scheduling Profiles allow you to configure Plugins that implement different scheduling stages, including: QueueSort, Filter, Score, Bind, Reserve, Permit, and others. You can also configure the kube-scheduler to run different profiles.

