## [install kubenetes completion](https://medium.com/merapar/fixing-bash-autocompletion-on-macos-for-kubectl-and-kops-e87f019652e8)

See also the build in command `kubectl completion  -h` for detail.

```bash
brew install bash-completion@2 # for bash 4.1

## then update your ~/.bash_profile or ~/.bashrc follow the caveats section

source <(kubectl completion bash)

if [ -f $(brew --prefix)/etc/bash_completion ]; then 
. $(brew --prefix)/etc/bash_completion
fi
```

## minikube

```bash
minikube start
minikube status
minikube dashboard
minikube stop
```

## kubectl

```bash
kubectl cluster-info

kubectl proxy # make dashboard available http://loclahost:8001

kubectl config view # display merged settings

# git clone https://github.com/reactiveops/k8s-workshop
kubectl apply -f redis/redis-primary.deployment.yml
kubectl create namespace test
kubectl get pods
kubectl logs redis-primary-fc7f47bd4-cljh5

kubectl get all -o wide
kubectl get service -o yaml
kubectl get pods -w
kubectl exec -it webapp-68d66c6cdf-p9lv7 bash
```

## Access Kubernetes API

```bash
TOKEN=$(kubectl describe secret $(kubectl get secrets |cut -d ' ' -f1|grep -E 'token')|grep -E '^toke'|cut -f2 -d':'|tr -d ' ')
APISERVER=$(kubectl config view |grep https|cut -f 2- -d':'|tr -d ' ')
curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure
```

## Kubenetes Object Model

Example of objects are Pods, Deployments, ReplicaSets, etc. The object model defines:

* what containerized app we are running and on which node
* app resource consumption
* policies attached to apps, like restart/upgrade, fault tolerance, etc

Following example defines a deployment, and a spec with 3 pods running, which is created using pod template defined in `spec.template.spec`. Each pod is a `nginx:1.7.9` on port 80

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

### Pods

A Pod s teh smallest and simplest Kubernetes object. It's the unit of deployment in Kubernetes, which represents a single instance of the application. A pod is logical collection of one of more containers which:

* are scheduled together on the same host
* share teh samel network namespace
* mount the same external storage(Volumes)
* pods are ephemeral, and they do not have sel-healing capability by themselves. They normally live together with controllers, which can handl a pod's replcation, fault tolerance, self-heal, etc. Example of controllers are Deployments, ReplicaSets, ReplicationControllers, etc. We attach pod's spec to other object using pod templates (see above example)

### Labels

Labels are key-value pair that can be attached to any Kubernetes objects(e.g., pods). Labels are used to organize and select a subset of objects, based on the requirements in place(similar to saltstack).

### Label selectors

With Label Selectors, we can select a subset of objects. Two types of selectors:

* Equality-based selectors: `env==dev`
* Set-based selectors: `env in (dev, qa)`

### Replication Controllers

A ReplciationController(rc) is a controller that is part of the Master Node's Controller Manager. It make sure the specified numver of replicas for a pod is running at any given point in time. If there are more, rc will kill the extra pods, otherwise, it will create more pods. Generally, we don't deploy a pod independently, as it has not self-heal capability. we always use controllers like ReplicationController to create ane manange pods.

### ReplicaSet

A ReplicaSet(rs) is the next-generation ReplicationController. ReplicaSet support both equality- and set-based selectors, whereas ReplicationControllers only suppport equality-based selectors. 

ReplicaSet can be used independently, but they usually used by deploymentsl to orchestrate the pod creattion, deletion, and updates.

### Deployments

Deployment objects provide declarative updates to Pods and ReplicaSets. The DeploymentController is part of the Master Node's Controller Manager, and it make sure that current state always match the desired state.

On top of RepliaSets, deployments provide features like Deployment recording, with which, if something goes wrong, we can rollback to a previously known state.

### Namespaces

We can partition the kubernetes cluster into sub-clusters using Namespace. The names of the resource/objects created inside a Namespace are unique, but not across Namespaces

```bash
kubectl get namespaces
```

Two default namespaces:

* kube-system
* default

### Resource Quotas

With resource quotas, we can divide the cluster resouces withing Namespaces,
