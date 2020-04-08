# Introduction
This guide aims to provide some "hits and tips", useful examples and other information which would help the beginners to learn and start using Kubernetes, famous open-source container-orchestration system.

# Monitoring
Metric Server replaced Heapster as primary cluster-wide metrics aggregator. It collects resource metrics from kubelets and exposes them in Kubernetes API Server through [Metrics API](https://github.com/kubernetes/metrics). Main consumers of those metrics are `kubectl top`, [HPA](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) and [VPA](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler). Metric server stores only the latest values of metrics needed for core metrics pipeline (CPU, Memory) and is not responsible for forwarding metrics to third-party destinations. Also, please review [Kubernetes monitoring architecture diagram](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/monitoring_architecture.md#appendix-architecture-diagram). It looks slightly outdated, but still can be useful.

# Auditing
* K8S auditing provides a security-relevant chronological set of records documenting the sequence of activities that affected the system
* Aims to to answer: who did what, to what entity, how it was initiated
* K8S auditing is performed by kube-apiserver according to the Audit Policy
* Auditing records are sent to the Audit Backend

* Auditing process cannot be accessed by the kubectl runtime or by Pods running inside the cluster

## K8S auditing can be broken down into 2 areas:
* The Audit Policy
  * Defines what should be logged
  * Is defined in a .yaml file and on the kube-apiserver, when it starts
  * kubectl don't have access
  * It may vary depoending on cluster provider, but for minikube - Audit Policy is passed to the cluster when it starts
  * Almost anything you can create or do in K8S, can be audited

* The audit backend
  * Exports the audit events to external storage
  * Out of box, kube-apiserver writes the events to local disk (Logs) or sends the events to an external API (Webhooks)

## Apply The Audit Policy for Minikube cluster
* Stop Minikube clister:
```
minikube stop
```
* Create the following folder on your local machine:
```
mkdir -p ~/.minikube/files/etc/ssl/certs
```
* Copy `audit-policy.yaml` file to that folder:
```
cp deployments/auditing/audit-policy.yaml ~/.minikube/files/etc/ssl/certs
```
* Start Minikube cluster using the following command:
```
minikube start \
  --extra-config=apiserver.audit-policy-file=/etc/ssl/certs/audit-policy.yaml \
  --extra-config=apiserver.audit-log-path=-
```
* Review `kube-apiserver-minikube` log:
```
kubectl logs kube-apiserver-minikube -n  kube-system | grep audit.k8s.io/v1
```

 
