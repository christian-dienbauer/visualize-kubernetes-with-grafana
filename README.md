# visualize-kubernetes-with-grafana

A deployment of Grafana within Kubernetes for its data visualization

## Prerequisits

1. Kubernetes cluster - If you don't have one available. You can use Minikube.
2. Kubectl
3. Helm - The package manager to install software on Kubernetes

### Run Minikube (Optional)

If you don't have a running Kubernetes cluster available, you can use minikube to spin up a local one. Follow the steps as described here: [Get started with Minikube](https://minikube.sigs.k8s.io/docs/start)

## Get Started

### Install Prometheus

For the installation we can make use of the following Helm charts.

First, let's create a namespace where we are going to deploy the services

```bash
kubectl create namespace monitoring
```

Add the prometheus-community repository to helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

```bash
helm repo update
```

Install prometheus

```bash
helm install prometheus prometheus-community/prometheus --namespace monitoring
```
<!-- Minikube specific -->
Expose prometheus externally to access the user interface

```bash
kubectl patch svc prometheus-server -n monitoring -p '{"spec": {"type": "NodePort", "ports": [{"port": 80}]}}'
```

Verify the deployment using

```bash
kubectl get pods -n monitoring
```
<!-- Minikube specific -->
Get a service URL with

```bash
minikube service prometheus-server -n monitoring
```

### Install Grafana


Add the grafana repository to helm.

```bash
helm repo add grafana https://grafana.github.io/helm-charts
```

Install Grafana

```bash
helm install grafana grafana/grafana --namespace monitoring
```
<!-- Minikube specific -->
Expose grafana externally to access the user interface

```bash
kubectl patch svc grafana -n monitoring -p '{"spec": {"type": "NodePort", "ports": [{"port": 80}]}}'
```

Verify the deployment using

```bash
kubectl get pods -n monitoring
```

Get the admin password

```bash
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
<!-- Minikube specific -->
Get a service URL with

```bash
minikube service grafana -n monitoring
```
