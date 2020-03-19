# Introduction
This guide aims to provide some "hits and tips", useful examples and other information which would help the beginners to learn and start using Kubernetes, famous open-source container-orchestration system.

# Monitoring
Kubernetes native stack for Usage & Resource monitoring consists of:

* Heapster - collects the data
* InfluxDB - stores the data
* Grafana - fetches the data from InfluxDB and visualize it

Heapster supports broad variety of DB backends, not only InfluxDB

InfluxDB is an open-source time series database
A time series database (TSDB) is a software system that is optimized for storing and serving time series through associated pairs of time(s) and value(s).

Heapster talks to each kubelet on each node
Kubelet itself collects the data from C advicer, an internal kubernetes system
