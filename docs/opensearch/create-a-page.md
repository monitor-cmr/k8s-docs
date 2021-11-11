---
sidebar_position: 1
---

# OpenSearch on Kubernetes

## Component

### OpenSearch

1. OpenSearch Coordinator
1. OpenSearch Master
1. OpenSearch Data

### Kubernetes

#### Services

1. Ingress Controller
    1. domain for routing - abc.alochym.vn
1. opensearch-cluster-master-headless - ClusterIP - None
    1. Using for discovering OpenSearch Master node
1. opensearch-coordinator-headless    - ClusterIP - None
    1. Using for discovering OpenSearch Coordinator node
    2. <https://opensearch.org/blog/community/2021/09/a-query-or-there-and-back-again/>
1. opensearch-data-headless           - ClusterIP - None
    1. Using for discovering OpenSearch Data node

#### Monitor

1. Using Prometheus Operator
    1. Using Service Monitor for monitoring OpenSearch Pods
        1. opensearch-cluster-master-headless
        1. opensearch-coordinator-headless
        1. opensearch-data-headless

## Network Topology

![Docusaurus logo](/img/opensearch/opensearch-network-topo.png)

## Installation

### Create Security

1. not-apply-network-policy.yaml

### Create Storage

1. storage-class.yaml
1. persistent-volume-master-0.yaml
1. persistent-volume-master-1.yaml
1. persistent-volume-master-2.yaml
1. persistent-volume-data-0.yaml
1. persistent-volume-data-1.yaml
1. persistent-volume-data-2.yaml
1. persistent-volume-coordinator-0.yaml
1. persistent-volume-coordinator-1.yaml
1. persistent-volume-coordinator-2.yaml

### Create Ingress Rules

1. opensearch-ingress-rules.yaml

### Create Config

1. namespace.yaml
1. configmap.yaml

### Create Master Nodes

1. poddisruptionbudget-master.yaml
1. statefulset-master-metrics.yaml
1. service-master-headless-service.yaml

### Create Data Nodes

1. poddisruptionbudget-data.yaml
1. statefulset-data-metrics.yaml
1. service-data-node-headless.yml

### Create Coordinator Node

1. poddisruptionbudget-coordinator.yaml
1. statefulset-coordinator.yaml
1. service-opensearch-coordinator.yaml

### Apply Custom Config Index

1. moment_meta_template.json
1. family-event.json
1. curl -X PUT -H "Content-Type: application/json" x.x.x.x:9200/_index_template/moment-meta -d "@moment_meta_template.json"
1. curl -X PUT -H "Content-Type: application/json" x.x.x.x:9200/_index_template/family-event -d "@family-event.json"

### Create Service Monitor

1. prometheus-role-opensearch-namespaces.yaml
1. prometheus-rolebinding-opensearch-namespaces.yaml
1. prometheus-service-account-prometheus-k8s-check-permission.md
1. servicemonitor-master-pormetheus.yaml
1. servicemonitor-data-pormetheus.yaml
1. servicemonitor-coordinator-pormetheus.yaml

## Using Helm

1. helm repo add opensearch <https://opensearch-project.github.io/helm-charts/>
1. helm repo update
1. helm template alochym-data opensearch/opensearch -f values.yaml |tee opensearch-template.yaml
1. helm custom values

   ```yml title="values.yaml"
   ...
   plugins:
        enabled: true
        installList:
        - https://github.com/aparo/opensearch-prometheus-exporter/releases/download/1.0.0/prometheus-exporter-1.0.0.zip
   ```
