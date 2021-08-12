# Kubernetes

[An updated version of this file](https://robust-dryosaurus-f97.notion.site/Kubernetes-223b3780ef8a4aca86b31f417a2248e5)

**What?** it is a container orchestration tool that makes sure that each container work in the right place and at the right time, helps you to manage your containerized application in different environments.

**Why?** the trend turns from Monolithic to Micro-services development so we need a proper tool to manage hundreds of micro-services (containers)

**Why K8s?** 

- Its high availability (no downtime)
- scalability (high performance)
- recovery

## Components

![Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-10_09-39-48.png](Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-10_09-39-48.png)

### Pod

- The smallest unit of cluster
- it is an abstraction of a container
- usually runs 1 main container

K8s has a virtual network for each cluster so you can access each pod with an IP, but it is not efficient cause the IP is not permanent if it restarts it takes a new IP, so we have **services**

### Services

- Pod gateway
- two types:
    - Internal: Pods call each other, the server calls the database
    - external: Outside node communication, browser call client app.

For the external communication, there is an additional layer which is **Ingre**s**s** it is routing the communication in the cluster.

### ConfigMap/Secret

External configuration for your app.

- Map: for the public configurations like the URL of the database
- Secret: for the private like username, password

### Data persistence

The k8s does not manage data persistence, so you need an external driver (cloud/local) to store your data, this (not part of cluster thing) is called **Volumes**

### Blueprints

The no-downtime and recovery features have been done with **Replica** technology. It takes a blueprint of your pods to make it work on other nodes to avoid missing the service, anytime if the original pods go down user connects to its replica.  There is two types of replica

- Deployment: For app pod (stateless apps. no data persistence)
- StateFulSet: for DB pods

![Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-09_15-23-54.png](Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-09_15-23-54.png)

## Architecture

The cluster has two types of nodes, and each node has some processes. Simple?

### Worker node

Think about the node as a machine ( your PC, Laptop, Server),

Each node has some pods and **Three processes:**

- **Container runtime:** is software that executes containers and manages container images on a node (like Docker)
- **Kubelet:** is the primary "node agent" that runs on each node, and register the newly created pods
- **Kube Proxy:** is a network proxy that runs on each node, maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

### Master node

It has four processes:

- **API server**: It is the entry point for the cluster
- **Scheduler**: calls the Kubelet in the worker to manage the pods
- **Controller Manager**: Maintain the crashed pods ( calls the scheduler )
- **etcd**: Cluster brain, it is key-value storage for the state of the cluster

![https://miro.medium.com/max/4936/1*svd2l7yAE-8LV7pR4bnVIA.png](https://miro.medium.com/max/4936/1*svd2l7yAE-8LV7pR4bnVIA.png)

![https://itglobal.com/wp-content/uploads/2021/03/01-2.png](https://itglobal.com/wp-content/uploads/2021/03/01-2.png)

in other words

**The Control Plane is responsible for managing the cluster.** The Control Plane coordinates all activities in your cluster, such as scheduling applications, maintaining applications' desired state, scaling applications, and rolling out new updates.

**A node is a VM or a physical computer that serves as a worker machine in a Kubernetes cluster.** Each node has a Kubelet, which is an agent for managing the node and communicating with the Kubernetes control plane. The node should also have tools for handling container operations, such as containerd or Docker

![Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-11_11-37-53.png](Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-11_11-37-53.png)

# Usage

For test/local setup we will use Minicube

**Minicube:** a one-node cluster do (worker/master) processes

Minikube is a lightweight Kubernetes implementation that creates a VM on your local machine and deploys a simple cluster containing only one node.

**kubectl**: (client) is a command line tool to interact with the cluster (local/remote)

## Hands-On

mini cube needs a virtual machine to works on 

mini cube has docker/kubectl as dependencies

### MiniKube commands

minikube: for starting-stoping-removing cluster

[Configuring various minikube drivers](https://minikube.sigs.k8s.io/docs/drivers/) you can start your cluster in a container or in VM.. [container](https://minikube.sigs.k8s.io/docs/drivers/docker/) preferred at linux

```bash

# start cluster on vm
minikube start --driver=docker

minikube status

# Open the Kubernetes dashboard in a browser:
minikube dashboard

# stop the Minikube virtual machine (VM):
minikube stop

# delete the Minikube VM:
minikube delete
```

### KubeCtl commands

Kubectl: for interacting with the cluster

```python
kubectl top # The kubectl top command returns current CPU and memory usage for a cluster’s pods or nodes, or for a particular pod or node if specified.
kubectl cluster-info 
```

[Kubectl commands](https://gitlab.com/nanuchi/youtube-tutorial-series/-/blob/master/basic-kubectl-commands/cli-commands.md)

```python
kubectl get all

# Check the status of any commponent
kubectl get nodes # Ready means: the vm ready to host your app
kubectl get pods
kubectl get deployment
kubectl get replicaset

# to get additional info
kubectl get pod -o wide
```

You are not creating a pod you create a deployment and it has a pod inside.

The abstraction layers look like that:

![Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-10_13-22-16.png](Kubernetes%20de5b2600884345b1ae4d2045d595d9b3/Screenshot_from_2021-08-10_13-22-16.png)

everything below the deployment should be handled automatically with k8s, you only touch the deployment

```python
# kubectl create deployment <deplyment name> --image=<local/hub image for container>
kubectl create deployment nginx-depl --image=nginx

# Or edit
kubectl edit deployment nginx-depl

# or delete
kubectl delete deployment nginx-depl
```

if you want to access the pod for debugging 

```python
kubectl logs {pod-name}
kubectl describe pod {pod-name}

# or exec the container
kubectl exec -it {pod-name} -- bin/bash

kubectl get # list resources
kubectl describe # show detailed information about a resource
kubectl logs # print the logs from a container in a pod
kubectl exec # execute a command on a container in a pod
```

```bash
kubectl proxy # create gateway

kubectl expose {pod_nam} --port= # create service

kubectl get services # list the services

kubectl discribe services
```

```bash
kubectl scale {pod-name} --replica=4
kubectl 
```

Pods that are running inside Kubernetes are running on a private, isolated network. By default they are visible from other pods and services within the same kubernetes cluster, but not outside that network. When we use `kubectl`, we're interacting through an API endpoint to communicate with our application.

We will cover other options on how to expose your application outside the kubernetes cluster in Module 4.

The `kubectl` command can create a proxy that will forward communications into the cluster-wide, private network. The proxy can be terminated by pressing control-C and won't show any output while its running.

We will open a second terminal window to run the proxy. `kubectl proxy`

### Config-file.yml

**Commands**

```python
# Create OR Update
kubectl apply -f file.yaml

# Delete
kubectl delete -f file.yaml
```

**Syntax**

The basic structure of the conf file has:

**Kind:** where you specify what kind of thing you want to build deployment or service

**Metadata:** just the metadata of the thing you create like the name or label

**Spec:** it has the specification of the component like how many replicas or pods or which port, its differ according to the kind of the component.

**Template:** under spec, you find the template and it is the blueprint of the pods in the deployment and under that, you will find a whole conf data for the pod (metadata and spec)

**Communication**

Services match a set of Pods using labels and selectors,

**Labels:** define the label (in the metadata of the component)

**Selector:** match the label (in the spec)

**Ports:** 

- port: of the service
- targetPort: in the service to match container port
- containerPort: in the pod

Finally, there is an auto-generated part of the config file is **STATUS.** it compares your desired state (defined in the conf file) with the actual state (defined in the etcd) if anything wrong it works to create it

**Example of a basic conf file.**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 8080
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

## Useful commands

```bash
minikube service <name-of-service> # to expose to the world

# make sure your Docker CLI uses the Docker deamon in your cluster via the command 
eval $(minikube docker-env). # No worries: if you restart your terminal you will automatically use your own Docker daemon again.

echo -n <passwod> | base64 # generate encrypted password

kubectl exec -it <pod-name> sh # start interactive shell
```

## Useful Resources

- [Official tutorial](https://kubernetes.io/docs/tutorials/)
- [Full course with Nana](https://www.youtube.com/watch?v=X48VuDVv0do)
- [Article from Test Driven](https://testdriven.io/blog/running-flask-on-kubernetes/)