<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [单机部署](#%E5%8D%95%E6%9C%BA%E9%83%A8%E7%BD%B2)
  - [安装 kubectl](#%E5%AE%89%E8%A3%85-kubectl)
  - [使用 Minikube 部署 Kubernetes](#%E4%BD%BF%E7%94%A8-minikube-%E9%83%A8%E7%BD%B2-kubernetes)
    - [安装](#%E5%AE%89%E8%A3%85)
    - [验证](#%E9%AA%8C%E8%AF%81)
  - [使用 Kind 部署 Kubernetes](#%E4%BD%BF%E7%94%A8-kind-%E9%83%A8%E7%BD%B2-kubernetes)
    - [安装](#%E5%AE%89%E8%A3%85-1)
    - [验证](#%E9%AA%8C%E8%AF%81-1)
  - [其它开源安装工具](#%E5%85%B6%E5%AE%83%E5%BC%80%E6%BA%90%E5%AE%89%E8%A3%85%E5%B7%A5%E5%85%B7)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 单机部署

## 安装 kubectl

Kubectl 是 Kubernetes 自带的命令行工具，可以用它直接操作 Kubernetes。

macOS，执行：

```bash
# using brew https://brew.sh/
brew install kubernetes-cli
```

Linux，执行：

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

Windows，执行：

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/windows/amd64/kubectl.exe
```

## 使用 Minikube 部署 Kubernetes

[Minikube](https://github.com/kubernetes/minikube) 用于本地部署 kubernetes 集群，支持 macOS，Linux，和 Windows。

**注意**：**科学上网**是必须的，否则 minikube iso 镜像文件，gcr.io 的 Docker 镜像等将无法下载。

### 安装

**下载依赖**：

* *macOS 10.12 (Sierra)*
  * 要求安装 hypervisor，比如 [hyperkit](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#hyperkit-driver) （推荐）或 [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  * 使用 [brew](https://brew.sh/) ： `brew cask install minikube`
  * 或者使用 curl： `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 && sudo install minikube-darwin-amd64 /usr/local/bin/minikube`

* *Windows 10*
  * 要求安装 hypervisor，比如 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) （推荐）或 [HyperV](https://docs.docker.com/machine/drivers/hyper-v/)
  * BIOS 中必须开启 VT-x/AMD-v virtualization
  * 使用 [chocolatey](https://chocolatey.org/) `choco install minikube`
  * 或者通过链接下载： Download and run the [installer](https://storage.googleapis.com/minikube/releases/latest/minikube-installer.exe)

* *Linux*
  * 要求安装 [kvm2 driver](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#kvm2-driver) （推荐）或 [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  * BIOS 中必须开启 VT-x/AMD-v virtualization
  * 使用 curl： `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && sudo install minikube-linux-amd64 /usr/local/bin/minikube`

**确认你的 minikube 至少是 v1.2.0**：

```sh
$ minikube version
minikube version: v1.2.0
```

**启动 Minikube**：

**注意**： 这里我们使用的是 VirtualBox，如果你用的其它，可能会需要另外的配置，请按照上一节 👆 的链接查找。

```sh
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

### 验证

执行下面的命令：

```sh
$ kubectl get nodes
NAME       STATUS   ROLES    AGE    VERSION
minikube   Ready    master   4m5s   v1.15.0
```

若输出正常，则表示创建成功。

## 使用 Kind 部署 Kubernetes

[Kind](https://github.com/kubernetes-sigs/kind) 是另一个 Kubernetes 集群部署工具，通过 Docker 容器 "nodes" 完成部署。

**注意**： 在这之前，你必须安装 [go](https://golang.org/) 和 [docker](https://www.docker.com/)，并且 go 的版本至少是 1.12.6。

### 安装

```sh
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

**注意**: 请务必执行输出中的命令，以配置 kubeconfig。

### 验证

执行下面的命令：

```sh
$ kubectl get nodes
NAME                 STATUS   ROLES    AGE     VERSION
kind-control-plane   Ready    master   2m54s   v1.15.0
```

## 其它开源安装工具

- https://github.com/bsycorp/kind
- https://github.com/ubuntu/microk8s
- https://github.com/kinvolk/kube-spawn
- https://github.com/danderson/virtuakube
- https://github.com/kubernetes-sigs/kubeadm-dind-cluster
