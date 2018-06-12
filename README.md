# Kubernetes Summit 2018 - Kubernetes: Stateless → Stateful


This repository demonstrate how to setup completed ElasticSearch within Kubernetes. And the installation leverage by [ElasticSearch Operator](https://github.com/upmc-enterprises/elasticsearch-operator)

## Prerequisite

### minikube
Follow the [official document](https://kubernetes.io/docs/tasks/tools/install-minikube/) to install minikube

start minikube with custom cpu and memory

```
~$ minikube start --memory 4096 --cpus 4
```

then check the minikube is running, and the Kubernetes can be accessed
```
~$ kubectl get node


NAME       STATUS    ROLES     AGE       VERSION
minikube   Ready     master    13s       v1.10.0
```

### Helm
Follow the [official document](https://github.com/kubernetes/helm/blob/master/docs/install.md) to install helm client in the working machine

then install tiller into Kubernetes cluster

```
~$ helm init

$HELM_HOME has been configured at /Users/smalltown/.helm.
Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.
Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation
Happy Helming!
```

## Installation
Here using Helm to complete the installation

### ElasticSearch Operator

```
~$ helm install --name=elasticsearch-operator --values elasticsearch-operator.yaml charts/elasticsearch-operator

NAME:   elasticsearch-operator
LAST DEPLOYED: Tue Jun 12 23:44:56 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
elasticsearch-operator  1        1        1           0          0s

==> v1/Pod(related)
NAME                                    READY  STATUS             RESTARTS  AGE
elasticsearch-operator-f69b6d958-89zpd  0/1    ContainerCreating  0         0s

==> v1/ServiceAccount
NAME                    SECRETS  AGE
elasticsearch-operator  1        0s

==> v1beta1/ClusterRole
NAME                    AGE
elasticsearch-operator  0s

==> v1beta1/ClusterRoleBinding
NAME                    AGE
elasticsearch-operator  0s
```

### ElasticSearch

```
~$ helm install --name=elasticsearch --values elasticsearch.yaml charts/elasticsearch

AME:   elasticsearch
LAST DEPLOYED: Tue Jun 12 23:52:48 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ElasticsearchCluster
NAME                   AGE
elasticsearch-cluster  0s


NOTES:
Creating Elasticsearch cluster...

Client endpoints to access your cluster:
 - https://elasticsearch-elasticsearch-cluster:9200
 - https://elasticsearch:9200

Cluster name: elasticsearch-cluster
```

## Verification

### ElasticSearch

```
~$ kubectl port-forward svc/elasticsearch-elasticsearch-cluster 9200:9200
```

Visit [http://127.0.0.1:9200/](http://127.0.0.1:9200/) by the browser

### Kibana

```
~$ kubectl port-forward svc/kibana-elasticsearch-cluster 10080:80
```

Visit [http://127.0.0.1:10080/](http://127.0.0.1:10080/) by the browser

### Cerebro

```
~$ kubectl port-forward svc/cerebro-elasticsearch-cluster 10081:80
```

Visit [http://127.0.0.1:10081/](http://127.0.0.1:10081/) by the browser
