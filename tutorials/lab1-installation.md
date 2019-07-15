<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Single Node Installation](#single-node-installation)
  - [Install kubectl Binary](#install-kubectl-binary)
  - [Minikube](#minikube)
    - [Installation](#installation)
    - [Start Minikube](#start-minikube)
    - [Verify installation](#verify-installation)
  - [Kind](#kind)
    - [Installation](#installation-1)
    - [Verify installation](#verify-installation-1)
  - [Alternatives](#alternatives)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Single Node Installation

## Install kubectl Binary

Kubectl is a command line interface for running commands against Kubernetes clusters.

For macOS, run:

```bash
# using brew https://brew.sh/
brew install kubernetes-cli
```

For Linux, run:

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

For Windows,

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/windows/amd64/kubectl.exe
```

## Minikube

Using [Minikube](https://github.com/kubernetes/minikube) to create a single node Kubernetes cluster on your laptop.

**Note**: a lot of resources are blocked by GFW, e.g. minikube iso, gcr.io docker images. Simply following
upstream guide is not enough to run a local kubernetes.

### Installation

* *macOS 10.12 (Sierra)*
  * Requires installing a hypervisor, such as [hyperkit](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#hyperkit-driver) (recommended) or [VirtualBox]
  * using [brew](https://brew.sh/): `brew cask install minikube`
  * manually: `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 && sudo install minikube-darwin-amd64 /usr/local/bin/minikube`

* *Windows 10*
  * Requires a hypervisor, such as [VirtualBox] (recommended) or HyperV
  * VT-x/AMD-v virtualization must be enabled in BIOS
  * using [chocolatey](https://chocolatey.org/) `choco install minikube`
  * manually: Download and run the [installer](https://storage.googleapis.com/minikube/releases/latest/minikube-installer.exe)

* *Linux*
  * Requires either the [kvm2 driver](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#kvm2-driver) (recommended), or [VirtualBox]
  * VT-x/AMD-v virtualization must be enabled in BIOS
  * manually: `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && sudo install minikube-linux-amd64 /usr/local/bin/minikube`

### Start Minikube

Make sure minkube version is at least v1.2.0:

```txt
$ minikube version
minikube version: v1.2.0
```

If not, please run upgrade (re-install) it first.

Then, start minikube:

**NOTE**: we use VirtualBox here, if you are using hyperkit, please run with `minikube start --vm-driver hyperkit`

```txt
$ minikube start
😄  minikube v1.2.0 on darwin (amd64)
🔥  Creating virtualbox VM (CPUs=2, Memory=2048MB, Disk=20000MB) ...
🐳  Configuring environment for Kubernetes v1.15.0 on Docker 18.09.6
💾  Downloading kubeadm v1.15.0
💾  Downloading kubelet v1.15.0
🚜  Pulling images ...
🚀  Launching Kubernetes ...
⌛  Verifying: apiserver proxy etcd scheduler controller dns
🏄  Done! kubectl is now configured to use "minikube"
```

### Verify installation

We can verify installation via:

```txt
$ kubectl get nodes
NAME       STATUS   ROLES    AGE    VERSION
minikube   Ready    master   4m5s   v1.15.0
```

## Kind

Another way to deploy a local kubernetes cluater is [kind], it is a tool for running local Kubernetes clusters using Docker container "nodes".

**NOTE**: you must have [go] and [docker] installed before doing this, and go 1.12.6 or greater is needed.

### Installation

```txt
$ GO111MODULE="on" go get sigs.k8s.io/kind && kind create cluster
...
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.15.0) 🖼
 ✓ Preparing nodes 📦
 ✓ Creating kubeadm config 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Cluster creation complete. You can now use the cluster with:

export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"
kubectl cluster-info
```

### Verify installation

**Notice**: you must set kubeconfig via:

```bash
$ export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"
```

We can verify installation via:

```txt
$ kubectl get nodes
NAME                 STATUS   ROLES    AGE     VERSION
kind-control-plane   Ready    master   2m54s   v1.15.0
```

## Alternatives

Some other open source projects with slightly different but overlapping use cases, features etc.

- https://github.com/bsycorp/kind
- https://github.com/ubuntu/microk8s
- https://github.com/kinvolk/kube-spawn
- https://github.com/danderson/virtuakube
- https://github.com/kubernetes-sigs/kubeadm-dind-cluster

[VirtualBox]: https://www.virtualbox.org/wiki/Downloads
[go]: https://golang.org/
[docker]: https://www.docker.com/
[kind]: https://github.com/kubernetes-sigs/kind
