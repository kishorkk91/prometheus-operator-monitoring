# prometheus-operator-monitoring
Deploy prometheus-operator on Bare Metal server using "kube-prometheus-stack-14.5.0" helm chart

## Introduction
The actual Prometheus operator is deprecated, moved, and renamed many times with new different names.
This is the new Prometheus operator from Prometheus-community with the correct helm chart: "kube-prometheus-stack":
https://github.com/prometheus-community/helm-charts/releases/tag/kube-prometheus-stack-14.5.0

## To deploy kube-prometheus-stack stack on cluster, the following images are taged and used from internal Docker repository
####custom prom-values-devops.yaml:

`
docker.artifactory.devops.crealogix.com/prometheus/alertmanager:v0.21.0

docker.artifactory.devops.crealogix.com/jettech/kube-webhook-certgen:v1.5.0

docker.artifactory.devops.crealogix.com/prometheus-operator/prometheus-operator:v0.46.0

docker.artifactory.devops.crealogix.com/prometheus/prometheus:v2.24.0
`
####grafana:

`
docker.artifactory.devops.crealogix.com/grafana/grafana:7.4.5
docker.artifactory.devops.crealogix.com/bats/bats:v1.1.0
docker.artifactory.devops.crealogix.com/curlimages/curl:7.73.0
docker.artifactory.devops.crealogix.com/busybox:1.31.1
docker.artifactory.devops.crealogix.com/kiwigrid/k8s-sidecar:1.10.7
`
####kube-state-metrics:
`
docker.artifactory.devops.crealogix.com/kube-state-metrics/kube-state-metrics:v1.9.8
`
####prometheus-node-exporter:
`
docker.artifactory.devops.crealogix.com/prometheus/node-exporter:v1.1.2
`
### Setting namespace permanently
`kubectl config set-context --current --namespace=monitoring
`
### Apply Artifactory Docker credentials to logging namespace
`kubectl apply -f artifactory-docker-regcred.yaml
`
### Deploy kube-prometheus-stack
`helm install prometheus charts/kube-prometheus-stack -f prom-values-devops.yaml
`
### How to upgrade prometheus stack instead of uninstall. Ex.
`helm upgrade prometheus charts/kube-prometheus-stack -f prom-values-devops.yaml
`
### Access Grafana, Prometheus and Alertmanager on cluster
http://grafana-devops.k8s.crealogix.net/?orgId=1

Access Graph, Targets, New UI , Classic UI:

http://prometheus-devops.k8s.crealogix.net:9090/

http://alertmanager-devops.k8s.crealogix.net:9093/#/alerts
