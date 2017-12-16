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
