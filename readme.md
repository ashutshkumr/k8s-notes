<p align="center"><img height="50px" src="https://kubernetes.io/images/nav_logo.svg" alt="Kubernetes Icon"></img></p>

## Overview

- [Kubernetes](https://kubernetes.io/) is an open-source Container Orchestration System written in Go (originally started by Google as Borg).
- Container Orchestration:
  * Group multiple hosts / nodes together to form a cluster
  * Schedule containers to run on hosts based on resource availability
  * Self healing
  * Networking between containers
  * Storage and Volumes
  * Limit resource usage
  * Policy implementation
  * Service discovery
  * Seamless rollout
- Ensures state of cluster is always maintained per deployment configuration (**Desired State**)
- Greek word for `helmsman` and widely called `k8s` (eight letters between k and s).
- Few other orchestrators are Amazon ECS, ACI, Docker Swarm, etc.
- Part of [Cloud Native Computing Foundation](https://www.cncf.io/) (CNCF) project.
  * CNCF ensures we have enough quality tools available to be used at any point of an application lifecycle
  * [Beginner's Course](https://www.edx.org/course/introduction-kubernetes-linuxfoundationx-lfs158x#!)
- New release every 3 months.
- Highly modular and pluggable. Functionality can be extended by writing:
  * custom resources
  * operators
  * custom API
  * scheduling rules or plugins
- Thriving community

## Architecture

<p align="center"><img src="https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg" alt="Kubernetes Components"></img></p>

- Master Nodes vs Worker Nodes
- Master Node is a single point of failure (brain of cluster), unless HA is configured
- Different control components on **master node** help in managing state of entire cluster
  * Users (e.g. `kubectl`) talk to `kube-apiserver` (**API Server**) via Kubernetes API
  * Cluster state / configuration is stored in `etcd` (**Data Store**). All communication via `kube-apiserver`.
  * `kube-scheduler` (**Scheduler**) schedules container workloads on a suitable worker node
  * **Controller Managers**, i.e. `kube-controller-manager` and `cloud-controller-manager`, continuously monitor cluster's desired state
- Most container workloads pertaining to application are deployed on **worker node**
  * **Pod** is a group of one or more containers (smallest scheduling unit; can't schedule containers)
  * **Container Runtime** (e.g. `containerd`, `docker`) is responsible for managing container lifecycle
  * `kubelet` (**Node Agent**) interacts uniformly with different container runtime via Container Runtime Interface (CRI) to execute container / image related operations (as communicated by API Server)
  * `kube-proxy` (**Proxy**) maintains networking rules allowing communication with pods (from in and out of cluster)
  * **Addons** are something that are not part of k8s yet but required frequently, e.g. DNS, Logging, Monitoring, Dashboard, etc.
- Networking:
  * Intra-Pod - All containers share same network namespace (`Pause container`); Address each other using localhost.
  * Inter-Pod - IP is assigned to each pod (`IP-per-pod`) by Container Network Interface (CNI) plugins. e.g. Flannel, Weave, Calico
  * Pod-to-Service
  * External-to-Service - virtual IP address (using `iptables` and `kube-proxy`)
