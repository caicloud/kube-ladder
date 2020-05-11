<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Kubernetes The Hard Way](#kubernetes-the-hard-way)
  - [目标](#%E7%9B%AE%E6%A0%87)
  - [假定](#%E5%81%87%E5%AE%9A)
  - [准备环境](#%E5%87%86%E5%A4%87%E7%8E%AF%E5%A2%83)
  - [安装必要工具](#%E5%AE%89%E8%A3%85%E5%BF%85%E8%A6%81%E5%B7%A5%E5%85%B7)
    - [安装 CFSSL](#%E5%AE%89%E8%A3%85-cfssl)
      - [Linux](#linux)
      - [验证](#%E9%AA%8C%E8%AF%81)
    - [安装 Kubectl](#%E5%AE%89%E8%A3%85-kubectl)
      - [Linux](#linux-1)
      - [验证](#%E9%AA%8C%E8%AF%81-1)
  - [配置 CA 并创建 TLS 证书](#%E9%85%8D%E7%BD%AE-ca-%E5%B9%B6%E5%88%9B%E5%BB%BA-tls-%E8%AF%81%E4%B9%A6)
    - [Certificate Authority](#certificate-authority)
      - [client 与 server 凭证](#client-%E4%B8%8E-server-%E5%87%AD%E8%AF%81)
      - [Admin 客户端凭证](#admin-%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%87%AD%E8%AF%81)
      - [Kubelet 客户端凭证](#kubelet-%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%87%AD%E8%AF%81)
      - [Kube-controller-manager 客户端凭证](#kube-controller-manager-%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%87%AD%E8%AF%81)
      - [Kube-proxy 客户端凭证](#kube-proxy-%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%87%AD%E8%AF%81)
      - [kube-scheduler 证书](#kube-scheduler-%E8%AF%81%E4%B9%A6)
      - [Kubernetes API Server 证书](#kubernetes-api-server-%E8%AF%81%E4%B9%A6)
      - [Service Account 证书](#service-account-%E8%AF%81%E4%B9%A6)
    - [分发客户端和服务器证书](#%E5%88%86%E5%8F%91%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%92%8C%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%AF%81%E4%B9%A6)
  - [配置和生成 Kubernetes 配置文件](#%E9%85%8D%E7%BD%AE%E5%92%8C%E7%94%9F%E6%88%90-kubernetes-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
    - [客户端认证配置](#%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%AE%A4%E8%AF%81%E9%85%8D%E7%BD%AE)
      - [kubelet 配置文件](#kubelet-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
      - [kube-proxy 配置文件](#kube-proxy-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
      - [kube-controller-manager 配置文件](#kube-controller-manager-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
      - [kube-scheduler 配置文件](#kube-scheduler-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
      - [Admin 配置文件](#admin-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
      - [分发配置文件](#%E5%88%86%E5%8F%91%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
  - [配置和生成密钥](#%E9%85%8D%E7%BD%AE%E5%92%8C%E7%94%9F%E6%88%90%E5%AF%86%E9%92%A5)
    - [加密密钥](#%E5%8A%A0%E5%AF%86%E5%AF%86%E9%92%A5)
  - [加密配置文件](#%E5%8A%A0%E5%AF%86%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
  - [部署 etcd 群集](#%E9%83%A8%E7%BD%B2-etcd-%E7%BE%A4%E9%9B%86)
    - [下载并安装 etcd 二进制文件](#%E4%B8%8B%E8%BD%BD%E5%B9%B6%E5%AE%89%E8%A3%85-etcd-%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%96%87%E4%BB%B6)
    - [配置 etcd Server](#%E9%85%8D%E7%BD%AE-etcd-server)
    - [启动 etcd Server](#%E5%90%AF%E5%8A%A8-etcd-server)
    - [验证](#%E9%AA%8C%E8%AF%81-2)
  - [部署 Kubernetes 控制节点](#%E9%83%A8%E7%BD%B2-kubernetes-%E6%8E%A7%E5%88%B6%E8%8A%82%E7%82%B9)
    - [创建 Kubernetes 配置目录](#%E5%88%9B%E5%BB%BA-kubernetes-%E9%85%8D%E7%BD%AE%E7%9B%AE%E5%BD%95)
    - [下载并安装 Kubernetes Controller 二进制文件](#%E4%B8%8B%E8%BD%BD%E5%B9%B6%E5%AE%89%E8%A3%85-kubernetes-controller-%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%96%87%E4%BB%B6)
    - [配置 Kubernetes API Server](#%E9%85%8D%E7%BD%AE-kubernetes-api-server)
    - [配置 Kubernetes Controller Manager](#%E9%85%8D%E7%BD%AE-kubernetes-controller-manager)
    - [配置 Kubernetes Scheduler](#%E9%85%8D%E7%BD%AE-kubernetes-scheduler)
    - [启动控制器服务](#%E5%90%AF%E5%8A%A8%E6%8E%A7%E5%88%B6%E5%99%A8%E6%9C%8D%E5%8A%A1)
    - [验证](#%E9%AA%8C%E8%AF%81-3)
    - [Kubelet RBAC 授权](#kubelet-rbac-%E6%8E%88%E6%9D%83)
  - [部署 Kubernetes Workers 节点](#%E9%83%A8%E7%BD%B2-kubernetes-workers-%E8%8A%82%E7%82%B9)
    - [安装依赖](#%E5%AE%89%E8%A3%85%E4%BE%9D%E8%B5%96)
    - [配置 Kubelet](#%E9%85%8D%E7%BD%AE-kubelet)
    - [配置 Kube-Proxy](#%E9%85%8D%E7%BD%AE-kube-proxy)
    - [启动 worker 服务](#%E5%90%AF%E5%8A%A8-worker-%E6%9C%8D%E5%8A%A1)
  - [配置 Kubectl](#%E9%85%8D%E7%BD%AE-kubectl)
    - [admin kubeconfig](#admin-kubeconfig)
    - [验证](#%E9%AA%8C%E8%AF%81-4)
  - [配置 Pod 网络路由](#%E9%85%8D%E7%BD%AE-pod-%E7%BD%91%E7%BB%9C%E8%B7%AF%E7%94%B1)
    - [安装](#%E5%AE%89%E8%A3%85)
  - [部署 DNS 扩展](#%E9%83%A8%E7%BD%B2-dns-%E6%89%A9%E5%B1%95)
    - [DNS 扩展](#dns-%E6%89%A9%E5%B1%95)
    - [验证](#%E9%AA%8C%E8%AF%81-5)
  - [烟雾测试](#%E7%83%9F%E9%9B%BE%E6%B5%8B%E8%AF%95)
    - [数据加密](#%E6%95%B0%E6%8D%AE%E5%8A%A0%E5%AF%86)
    - [部署](#%E9%83%A8%E7%BD%B2)
      - [端口转发](#%E7%AB%AF%E5%8F%A3%E8%BD%AC%E5%8F%91)
      - [容器日志](#%E5%AE%B9%E5%99%A8%E6%97%A5%E5%BF%97)
      - [执行容器命令](#%E6%89%A7%E8%A1%8C%E5%AE%B9%E5%99%A8%E5%91%BD%E4%BB%A4)
    - [服务（Service）](#%E6%9C%8D%E5%8A%A1service)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Kubernetes The Hard Way

本文主要翻译自 [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)，并做了部分调整，针对裸机（Bare-metal）部署。

## 目标

本教程将指引你在裸机（Bare-metal）上手动部署一套 Kubernetes 集群。它不适用于想要一键自动化部署 Kubernetes 集群的人。如果你想要自动化部署，请参考 [lab1](./lab1-installation.md) 或者 [kubeadm](https://github.com/kubernetes/kubeadm)

本文的主要目的是学习, 也就是说它会花很多时间来保障读者可以真正理解搭建 Kubernetes 的每个步骤。

## 假定

- 本文基于 Kubernetes v1.15.0
- Cluster Pod CIDR: 10.244.0.0/16
- Cluster Service CIDR: 10.250.0.0/24
- `kubernetes` service: 10.250.0.1
- `dns` service: 10.250.0.10

## 准备环境

- 至少两台 2C 4G 的 VM，装有 CentOS 7
- 确保 VM 可以访问 Internet
- 确保 VM 间网络并关闭了防火墙
- 确保 VM 间可以通过 hostname 互相访问，即，假如，VM1 的 hostname 是 master，VM2 的 hostname 是 worker-1，那么 VM1 需要能够成功执行 `ping worker-1`。（或许你需要修改 hostname & `/etc/hosts`）

**并在准备好的所有节点上，执行以下操作：**

如果各个主机启用了防火墙，需要开放 k8s 各个组件所需要的端口，可以查看 [Installing kubeadm - Check required ports](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports)。
这里简单起见直接禁用防火墙：

```bash
systemctl stop firewalld
systemctl disable firewalld
```

禁用 SELinux：

```bash
# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

关闭 swap：

```bash
swapoff -a
```

CentOS 7 用户还需要执行：

```bash
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```

**除非另有说明，以下所有操作都是在 master 上执行。**

## 安装必要工具

本次实验你将会安装一些实用的命令行工具, 用来完成这份指南，这包括 [cfssl](https://github.com/cloudflare/cfssl)、[cfssljson](https://github.com/cloudflare/cfssl) 以及 [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl)。

### 安装 CFSSL

cfssl 和 cfssljson 命令行工具用于提供 PKI Infrastructure 基础设施与生成 TLS 证书。

#### Linux

```bash
# 或许你需要先执行 `yum install -y wget` 以安装 wget
wget --timestamping \
  https://pkg.cfssl.org/R1.2/cfssl_linux-amd64 \
  https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64
chmod +x cfssl_linux-amd64 cfssljson_linux-amd64
sudo mv cfssl_linux-amd64 /usr/local/bin/cfssl
sudo mv cfssljson_linux-amd64 /usr/local/bin/cfssljson
```

#### 验证

验证 cfssl 的版本为 1.2.0 或是更高

```
$ cfssl version
Version: 1.2.0
Revision: dev
Runtime: go1.6
```

**注意**：cfssljson 命令行工具没有提供查询版本的方法。

### 安装 Kubectl

kubectl 命令行工具用来与 Kubernetes API Server 交互，可以在 Kubernetes 官方网站下载并安装 kubectl。

#### Linux

```bash
wget https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

#### 验证

验证 kubectl 的安装版本为 1.15.0 或是更高

```bash
$ kubectl version --client
Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.0", GitCommit:"e8462b5b5dc2584fdcd18e6bcfe9f1e4d970a529", GitTreeState:"clean", BuildDate:"2019-06-19T16:40:16Z", GoVersion:"go1.12.5", Compiler:"gc", Platform:"linux/amd64"}
```

## 配置 CA 并创建 TLS 证书

我们将使用 CloudFlare's PKI 工具 [cfssl](https://github.com/cloudflare/cfssl) 来配置 [PKI Infrastructure](https://en.wikipedia.org/wiki/Public_key_infrastructure
)，然后使用它去创建 Certificate Authority（CA），并为 etcd、kube-apiserver、kubelet 以及 kube-proxy 创建 TLS 证书。

### Certificate Authority

本节创建用于生成其他 TLS 证书的 Certificate Authority。

新建 CA 配置文件

```sh
cat > ca-config.json <<EOF
{
  "signing": {
    "default": {
      "expiry": "8760h"
    },
    "profiles": {
      "kubernetes": {
        "usages": ["signing", "key encipherment", "server auth", "client auth"],
        "expiry": "8760h"
      }
    }
  }
}
EOF
```

新建 CA 凭证签发请求文件:

```sh
cat > ca-csr.json <<EOF
{
  "CN": "Kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "Kubernetes",
      "OU": "Shanghai",
      "ST": "Shanghai"
    }
  ]
}
EOF
```

生成 CA 凭证和私钥:

```sh
cfssl gencert -initca ca-csr.json | cfssljson -bare ca
```

结果将生成以下两个文件：

```sh
ca-key.pem
ca.pem
```

#### client 与 server 凭证

本节将创建用于 Kubernetes 组件的 client 与 server 凭证，以及一个用于 Kubernetes admin 用户的 client 凭证。

#### Admin 客户端凭证

创建 `admin` client 凭证签发请求文件:

```sh
cat > admin-csr.json <<EOF
{
  "CN": "admin",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "system:masters",
      "OU": "Kubernetes",
      "ST": "Shanghai"
    }
  ]
}
EOF
```

创建 `admin` client 凭证和私钥:

```sh
cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  admin-csr.json | cfssljson -bare admin
```

结果将生成以下两个文件

```sh
admin-key.pem
admin.pem
```

#### Kubelet 客户端凭证

Kubernetes 使用 [special-purpose authorization mode](https://kubernetes.io/docs/admin/authorization/node/)（被称作 Node Authorizer）授权来自 [Kubelet](https://kubernetes.io/docs/concepts/overview/components/#kubelet) 的 API 请求。

为了通过 Node Authorizer 的授权, Kubelet 必须使用一个署名为 `system:node:<nodeName>` 的凭证来证明它属于 `system:nodes` 用户组。本节将会给每台 worker 节点创建符合 Node Authorizer 要求的凭证。

给每台 worker 节点创建凭证和私钥：

```sh
# 此处只给 worker-1 创建，若你有多个 worker 节点，请替换相应的值并继续操作
export WORKER1_HOSTNAME=worker-1
# 请替换为你所用节点的 IP
export WORKER1_IP=192.168.133.42

cat > worker-1-csr.json <<EOF
{
  "CN": "system:node:${WORKER1_HOSTNAME}",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "system:nodes",
      "OU": "Kubernetes",
      "ST": "Shanghai"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=${WORKER1_HOSTNAME},${WORKER1_IP} \
  -profile=kubernetes \
  worker-1-csr.json | cfssljson -bare worker-1
```

结果将产生以下几个文件：

```sh
worker-1-key.pem
worker-1.pem
```

#### Kube-controller-manager 客户端凭证

```sh
cat > kube-controller-manager-csr.json <<EOF
{
  "CN": "system:kube-controller-manager",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "system:kube-controller-manager",
      "OU": "Kubernetes",
      "ST": "Shanghai"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-controller-manager-csr.json | cfssljson -bare kube-controller-manager
```

结果将产生以下几个文件：

```sh
kube-controller-manager-key.pem
kube-controller-manager.pem
```

#### Kube-proxy 客户端凭证

```sh
cat > kube-proxy-csr.json <<EOF
{
  "CN": "system:kube-proxy",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "system:node-proxier",
      "OU": "Kubernetes",
      "ST": "Shanghai"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-proxy-csr.json | cfssljson -bare kube-proxy
```

结果将产生以下两个文件：

```sh
kube-proxy-key.pem
kube-proxy.pem
```

#### kube-scheduler 证书

```sh
cat > kube-scheduler-csr.json <<EOF
{
  "CN": "system:kube-scheduler",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "system:kube-scheduler",
      "OU": "Kubernetes",
      "ST": "Shanghai"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-scheduler-csr.json | cfssljson -bare kube-scheduler
```

结果将产生以下两个文件：

```sh
kube-scheduler-key.pem
kube-scheduler.pem
```

#### Kubernetes API Server 证书

为了保证客户端与 Kubernetes API 的认证，Kubernetes API Server 凭证中必需包含 master 的静态 IP 地址。

创建 Kubernetes API Server 凭证签发请求文件:

```sh
cat > kubernetes-csr.json <<EOF
{
  "CN": "kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "Kubernetes",
      "OU": "Kubernetes",
      "ST": "Shanghai"
    }
  ]
}
EOF
```

创建 Kubernetes API Server 凭证与私钥:

```sh
# 请替换为你所用节点的 IP
export MASTER_IP=192.168.133.41

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=10.250.0.1,${MASTER_IP},127.0.0.1,kubernetes.default \
  -profile=kubernetes \
  kubernetes-csr.json | cfssljson -bare kubernetes
```

结果产生以下两个文件:

```sh
kubernetes-key.pem
kubernetes.pem
```

#### Service Account 证书

```sh
cat > service-account-csr.json <<EOF
{
  "CN": "service-accounts",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "China",
      "L": "Shanghai",
      "O": "Kubernetes",
      "OU": "Kubernetes",
      "ST": "Shanghai"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  service-account-csr.json | cfssljson -bare service-account
```

结果将生成以下两个文件

```sh
service-account-key.pem
service-account.pem
```

### 分发客户端和服务器证书

将客户端凭证以及私钥复制到每个 worker 节点上:

```sh
scp ca.pem worker-1-key.pem worker-1.pem worker-1:~/
```

将服务器凭证以及私钥复制到每个控制节点上:

```sh
scp ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem \
    service-account-key.pem service-account.pem master:~/
```

> `kube-proxy`、`kube-controller-manager`、`kube-scheduler` 和 `kubelet` 客户端凭证将会在下一节中用来创建客户端签发请求文件。

## 配置和生成 Kubernetes 配置文件

本部分内容将会创建 [kubeconfig 配置文件](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)，它们是 Kubernetes 客户端与 API Server 认证与鉴权的保证。

### 客户端认证配置

本节将会创建用于 `kube-proxy`、`kube-controller-manager`、`kube-scheduler` 和 `kubelet` 的 kubeconfig 文件。

#### kubelet 配置文件

为了确保 [Node Authorizer](https://kubernetes.io/docs/admin/authorization/node/) 授权，Kubelet 配置文件中的客户端证书必需匹配 Node 名字。

为每个 worker 节点创建 kubeconfig 配置：

```sh
kubectl config set-cluster kubernetes-training \
  --certificate-authority=ca.pem \
  --embed-certs=true \
  --server=https://${MASTER_IP}:6443 \
  --kubeconfig=worker-1.kubeconfig

kubectl config set-credentials system:node:worker-1 \
  --client-certificate=worker-1.pem \
  --client-key=worker-1-key.pem \
  --embed-certs=true \
  --kubeconfig=worker-1.kubeconfig

kubectl config set-context default \
  --cluster=kubernetes-training \
  --user=system:node:worker-1 \
  --kubeconfig=worker-1.kubeconfig

kubectl config use-context default --kubeconfig=worker-1.kubeconfig
```

结果将会生成以下文件：

```sh
worker-1.kubeconfig
```

#### kube-proxy 配置文件

为 kube-proxy 服务生成 kubeconfig 配置文件：

```sh
kubectl config set-cluster kubernetes-training \
  --certificate-authority=ca.pem \
  --embed-certs=true \
  --server=https://${MASTER_IP}:6443 \
  --kubeconfig=kube-proxy.kubeconfig

kubectl config set-credentials system:kube-proxy \
  --client-certificate=kube-proxy.pem \
  --client-key=kube-proxy-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-proxy.kubeconfig

kubectl config set-context default \
  --cluster=kubernetes-training \
  --user=system:kube-proxy \
  --kubeconfig=kube-proxy.kubeconfig

kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig
```

#### kube-controller-manager 配置文件

```sh
kubectl config set-cluster kubernetes-training \
  --certificate-authority=ca.pem \
  --embed-certs=true \
  --server=https://127.0.0.1:6443 \
  --kubeconfig=kube-controller-manager.kubeconfig

kubectl config set-credentials system:kube-controller-manager \
  --client-certificate=kube-controller-manager.pem \
  --client-key=kube-controller-manager-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-controller-manager.kubeconfig

kubectl config set-context default \
  --cluster=kubernetes-training \
  --user=system:kube-controller-manager \
  --kubeconfig=kube-controller-manager.kubeconfig

kubectl config use-context default --kubeconfig=kube-controller-manager.kubeconfig
```

#### kube-scheduler 配置文件

```sh
kubectl config set-cluster kubernetes-training \
  --certificate-authority=ca.pem \
  --embed-certs=true \
  --server=https://127.0.0.1:6443 \
  --kubeconfig=kube-scheduler.kubeconfig

kubectl config set-credentials system:kube-scheduler \
  --client-certificate=kube-scheduler.pem \
  --client-key=kube-scheduler-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-scheduler.kubeconfig

kubectl config set-context default \
  --cluster=kubernetes-training \
  --user=system:kube-scheduler \
  --kubeconfig=kube-scheduler.kubeconfig

kubectl config use-context default --kubeconfig=kube-scheduler.kubeconfig
```

#### Admin 配置文件

```sh
kubectl config set-cluster kubernetes-training \
  --certificate-authority=ca.pem \
  --embed-certs=true \
  --server=https://127.0.0.1:6443 \
  --kubeconfig=admin.kubeconfig

kubectl config set-credentials admin \
  --client-certificate=admin.pem \
  --client-key=admin-key.pem \
  --embed-certs=true \
  --kubeconfig=admin.kubeconfig

kubectl config set-context default \
  --cluster=kubernetes-training \
  --user=admin \
  --kubeconfig=admin.kubeconfig

kubectl config use-context default --kubeconfig=admin.kubeconfig
```

#### 分发配置文件

将 `kubelet` 与 `kube-proxy` kubeconfig 配置文件复制到每个 worker 节点上：

```sh
scp worker-1.kubeconfig kube-proxy.kubeconfig worker-1:~/
```

将 `admin`、`kube-controller-manager` 与 `kube-scheduler` kubeconfig 配置文件复制到每个控制节点上：

```sh
scp admin.kubeconfig kube-controller-manager.kubeconfig kube-scheduler.kubeconfig master:~/
```

## 配置和生成密钥

Kubernetes 存储了集群状态、应用配置和密钥等很多不同的数据。而 Kubernetes 也支持集群数据的加密存储。

本部分将会创建加密密钥以及一个用于加密 Kubernetes Secrets 的 [加密配置文件](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#understanding-the-encryption-at-rest-configuration)。

### 加密密钥

建立加密密钥:

```sh
ENCRYPTION_KEY=$(head -c 32 /dev/urandom | base64)
```

## 加密配置文件

生成名为 `encryption-config.yaml` 的加密配置文件：

```sh
cat > encryption-config.yaml <<EOF
kind: EncryptionConfig
apiVersion: v1
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: ${ENCRYPTION_KEY}
      - identity: {}
EOF
```

将 `encryption-config.yaml` 复制到每个控制节点上：

```sh
scp encryption-config.yaml master:~/
```

## 部署 etcd 群集

Kubernetes 组件都是无状态的，所有的群集状态都储存在 [etcd](https://github.com/coreos/etcd) 集群中。

### 下载并安装 etcd 二进制文件

从 [coreos/etcd](https://github.com/coreos/etcd) GitHub 中下载 etcd 发布文件：

```sh
wget --timestamping \
  "https://github.com/coreos/etcd/releases/download/v3.3.9/etcd-v3.3.9-linux-amd64.tar.gz"
```

解压缩并安装 `etcd` 服务与 `etcdctl` 命令行工具：

```sh
tar -xvf etcd-v3.3.9-linux-amd64.tar.gz
sudo mv etcd-v3.3.9-linux-amd64/etcd* /usr/local/bin/
```

### 配置 etcd Server

```sh
sudo mkdir -p /etc/etcd /var/lib/etcd
sudo cp ca.pem kubernetes-key.pem kubernetes.pem /etc/etcd/
```

每个 etcd 成员必须有一个整集群中唯一的名字，使用 hostname 作为 etcd name：

```sh
ETCD_NAME=$(hostname -s)
```

生成 `etcd.service` 的 systemd 配置文件

*请注意：对于使用公网主机（如云服务器等）的操作者， etcd 绑定的服务端口（即以下的 `${MASTER_IP}` ）应使用内网 IP 地址。*

```sh
cat <<EOF | sudo tee /etc/systemd/system/etcd.service
[Unit]
Description=etcd
Documentation=https://github.com/coreos
[Service]
ExecStart=/usr/local/bin/etcd \\
  --name master \\
  --cert-file=/etc/etcd/kubernetes.pem \\
  --key-file=/etc/etcd/kubernetes-key.pem \\
  --peer-cert-file=/etc/etcd/kubernetes.pem \\
  --peer-key-file=/etc/etcd/kubernetes-key.pem \\
  --trusted-ca-file=/etc/etcd/ca.pem \\
  --peer-trusted-ca-file=/etc/etcd/ca.pem \\
  --peer-client-cert-auth \\
  --client-cert-auth \\
  --initial-advertise-peer-urls https://${MASTER_IP}:2380 \\
  --listen-peer-urls https://${MASTER_IP}:2380 \\
  --listen-client-urls https://${MASTER_IP}:2379,http://127.0.0.1:2379 \\
  --advertise-client-urls https://${MASTER_IP}:2379,http://127.0.0.1:2379 \\
  --data-dir=/var/lib/etcd
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
```

### 启动 etcd Server

```sh
sudo systemctl daemon-reload
sudo systemctl enable etcd
sudo systemctl start etcd
```

### 验证

列出 etcd 的群集成员:

```sh
ETCDCTL_API=3 etcdctl member list
```

> 输出

```sh
45ad218d293b8803, started, master, https://192.168.133.41:2380, http://127.0.0.1:2379,https://192.168.133.41:2379
```

## 部署 Kubernetes 控制节点

本部分将会在控制节点上部署 Kubernetes 控制服务。每个控制节点上需要部署的服务包括：Kubernetes API Server、Scheduler 以及 Controller Manager 等。

### 创建 Kubernetes 配置目录

```sh
sudo mkdir -p /etc/kubernetes/config
```

### 下载并安装 Kubernetes Controller 二进制文件

```sh
wget --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kube-apiserver" \
  "https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kube-controller-manager" \
  "https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kube-scheduler" \
  "https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kubectl"

chmod +x kube-apiserver kube-controller-manager kube-scheduler kubectl
sudo mv kube-apiserver kube-controller-manager kube-scheduler kubectl /usr/local/bin/
```

### 配置 Kubernetes API Server

```sh
sudo mkdir -p /var/lib/kubernetes/

sudo mv ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem \
  service-account-key.pem service-account.pem \
  encryption-config.yaml /var/lib/kubernetes/
```

生成 `kube-apiserver.service` systemd 配置文件：

```sh
cat <<EOF | sudo tee /etc/systemd/system/kube-apiserver.service
[Unit]
Description=Kubernetes API Server
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-apiserver \\
  --advertise-address=${MASTER_IP} \\
  --allow-privileged=true \\
  --audit-log-maxage=30 \\
  --audit-log-maxbackup=3 \\
  --audit-log-maxsize=100 \\
  --audit-log-path=/var/log/audit.log \\
  --authorization-mode=Node,RBAC \\
  --bind-address=0.0.0.0 \\
  --client-ca-file=/var/lib/kubernetes/ca.pem \\
  --enable-admission-plugins=NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,DefaultTolerationSeconds,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota \\
  --enable-swagger-ui=true \\
  --etcd-cafile=/var/lib/kubernetes/ca.pem \\
  --etcd-certfile=/var/lib/kubernetes/kubernetes.pem \\
  --etcd-keyfile=/var/lib/kubernetes/kubernetes-key.pem \\
  --etcd-servers=http://127.0.0.1:2379 \\
  --event-ttl=1h \\
  --experimental-encryption-provider-config=/var/lib/kubernetes/encryption-config.yaml \\
  --insecure-bind-address=127.0.0.1 \\
  --kubelet-certificate-authority=/var/lib/kubernetes/ca.pem \\
  --kubelet-client-certificate=/var/lib/kubernetes/kubernetes.pem \\
  --kubelet-client-key=/var/lib/kubernetes/kubernetes-key.pem \\
  --kubelet-https=true \\
  --runtime-config=api/all \\
  --service-account-key-file=/var/lib/kubernetes/service-account.pem \\
  --service-cluster-ip-range=10.250.0.0/24 \\
  --service-node-port-range=30000-32767 \\
  --tls-cert-file=/var/lib/kubernetes/kubernetes.pem \\
  --tls-private-key-file=/var/lib/kubernetes/kubernetes-key.pem \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### 配置 Kubernetes Controller Manager

生成 `kube-controller-manager.service` systemd 配置文件：

```sh
sudo mv kube-controller-manager.kubeconfig /var/lib/kubernetes/

cat <<EOF | sudo tee /etc/systemd/system/kube-controller-manager.service
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-controller-manager \\
  --address=0.0.0.0 \\
  --allocate-node-cidrs=true \\
  --cluster-cidr=10.244.0.0/16 \\
  --cluster-name=kubernetes \\
  --cluster-signing-cert-file=/var/lib/kubernetes/ca.pem \\
  --cluster-signing-key-file=/var/lib/kubernetes/ca-key.pem \\
  --kubeconfig=/var/lib/kubernetes/kube-controller-manager.kubeconfig \\
  --leader-elect=true \\
  --root-ca-file=/var/lib/kubernetes/ca.pem \\
  --service-account-private-key-file=/var/lib/kubernetes/service-account-key.pem \\
  --service-cluster-ip-range=10.250.0.0/24 \\
  --use-service-account-credentials=true \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### 配置 Kubernetes Scheduler

生成 `kube-scheduler.service` systemd 配置文件：

```sh
sudo mv kube-scheduler.kubeconfig /var/lib/kubernetes/

cat <<EOF | sudo tee /etc/kubernetes/config/kube-scheduler.yaml
apiVersion: kubescheduler.config.k8s.io/v1alpha1
kind: KubeSchedulerConfiguration
clientConnection:
  kubeconfig: "/var/lib/kubernetes/kube-scheduler.kubeconfig"
leaderElection:
  leaderElect: true
EOF

cat <<EOF | sudo tee /etc/systemd/system/kube-scheduler.service
[Unit]
Description=Kubernetes Scheduler
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-scheduler \\
  --config=/etc/kubernetes/config/kube-scheduler.yaml \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### 启动控制器服务

```sh
sudo systemctl daemon-reload
sudo systemctl enable kube-apiserver kube-controller-manager kube-scheduler
sudo systemctl start kube-apiserver kube-controller-manager kube-scheduler
```

> 请等待 10 秒以便 Kubernetes API Server 初始化。

### 验证

```sh
kubectl get componentstatuses --kubeconfig admin.kubeconfig
```

将输出结果

```sh
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health":"true"}
```

### Kubelet RBAC 授权

本节将会配置 API Server 访问 Kubelet API 的 RBAC 授权。访问 Kubelet API 是获取 metrics、日志以及执行容器命令所必需的。

> 这里设置 Kubelet `--authorization-mode` 为 `Webhook` 模式。Webhook 模式使用 [SubjectAccessReview](https://kubernetes.io/docs/admin/authorization/#checking-api-access) API 来决定授权。

创建 `system:kube-apiserver-to-kubelet` [ClusterRole](https://kubernetes.io/docs/admin/authorization/rbac/#role-and-clusterrole) 以允许请求 Kubelet API 和执行大部分来管理 Pods 的任务:

```sh
cat <<EOF | kubectl apply --kubeconfig admin.kubeconfig -f -
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  labels:
    kubernetes.io/bootstrapping: rbac-defaults
  name: system:kube-apiserver-to-kubelet
rules:
  - apiGroups:
      - ""
    resources:
      - nodes/proxy
      - nodes/stats
      - nodes/log
      - nodes/spec
      - nodes/metrics
    verbs:
      - "*"
EOF
```

Kubernetes API Server 使用客户端凭证授权 Kubelet 为 `kubernetes` 用户，此凭证用 `--kubelet-client-certificate` flag 来定义。

绑定 `system:kube-apiserver-to-kubelet` ClusterRole 到 `kubernetes` 用户:

```sh
cat <<EOF | kubectl apply --kubeconfig admin.kubeconfig -f -
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: system:kube-apiserver
  namespace: ""
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:kube-apiserver-to-kubelet
subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: kubernetes
EOF
```

## 部署 Kubernetes Workers 节点

本部分将会部署 Kubernetes Worker 节点。每个节点上将会安装以下服务：

- [Docker](https://www.docker.com)
- [container networking plugins](https://github.com/containernetworking/cni)
- [kubelet](https://kubernetes.io/docs/admin/kubelet)
- [kube-proxy](https://kubernetes.io/docs/concepts/cluster-administration/proxies)

### 安装依赖

安装 OS 依赖组件：

```sh
sudo yum install -y socat conntrack ipset
```

> socat 命令用于支持 `kubectl port-forward` 命令。

安装 Docker：

```sh
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install -y docker-ce docker-ce-cli containerd.io

sudo systemctl enable docker
sudo systemctl start docker
```

下载 worker 二进制文件：

```sh
wget --timestamping \
  https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz \
  https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kubectl \
  https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kube-proxy \
  https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kubelet
```

创建安装目录：

```sh
sudo mkdir -p \
  /opt/cni/bin \
  /var/lib/kubelet \
  /var/lib/kube-proxy \
  /var/lib/kubernetes \
  /var/run/kubernetes
```

安装 worker 二进制文件

```sh
chmod +x kubectl kube-proxy kubelet
sudo mv kubectl kube-proxy kubelet /usr/local/bin/
```

### 配置 Kubelet

```sh
sudo cp worker-1-key.pem worker-1.pem /var/lib/kubelet/
sudo cp worker-1.kubeconfig /var/lib/kubelet/kubeconfig
sudo cp ca.pem /var/lib/kubernetes/
sudo tar -xvf cni-plugins-amd64-v0.6.0.tgz -C /opt/cni/bin/
```

生成 `kubelet.service` systemd 配置文件：

```sh
# The resolvConf configuration is used to avoid loops
# when using CoreDNS for service discovery on systems running systemd-resolved.
cat <<EOF | sudo tee /var/lib/kubelet/kubelet-config.yaml
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
authentication:
  anonymous:
    enabled: false
  webhook:
    enabled: true
  x509:
    clientCAFile: "/var/lib/kubernetes/ca.pem"
authorization:
  mode: Webhook
clusterDomain: "cluster.local"
clusterDNS:
  - "10.250.0.10"
runtimeRequestTimeout: "15m"
tlsCertFile: "/var/lib/kubelet/worker-1.pem"
tlsPrivateKeyFile: "/var/lib/kubelet/worker-1-key.pem"
EOF

cat <<EOF | sudo tee /etc/systemd/system/kubelet.service
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/kubernetes/kubernetes
After=docker.service
Requires=docker.service

[Service]
ExecStart=/usr/local/bin/kubelet \\
  --config=/var/lib/kubelet/kubelet-config.yaml \\
  --image-pull-progress-deadline=2m \\
  --kubeconfig=/var/lib/kubelet/kubeconfig \\
  --pod-infra-container-image=cargo.caicloud.io/caicloud/pause-amd64:3.1 \\
  --network-plugin=cni \\
  --register-node=true \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### 配置 Kube-Proxy

```sh
sudo mv kube-proxy.kubeconfig /var/lib/kube-proxy/kubeconfig
```

生成 `kube-proxy.service` systemd 配置文件：

```sh
cat <<EOF | sudo tee /var/lib/kube-proxy/kube-proxy-config.yaml
kind: KubeProxyConfiguration
apiVersion: kubeproxy.config.k8s.io/v1alpha1
clientConnection:
  kubeconfig: "/var/lib/kube-proxy/kubeconfig"
mode: "iptables"
clusterCIDR: "10.244.0.0/16"
EOF

cat <<EOF | sudo tee /etc/systemd/system/kube-proxy.service
[Unit]
Description=Kubernetes Kube Proxy
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-proxy \\
  --config=/var/lib/kube-proxy/kube-proxy-config.yaml
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### 启动 worker 服务

```sh
sudo systemctl daemon-reload
sudo systemctl enable kubelet kube-proxy
sudo systemctl start kubelet kube-proxy
```

> 记得在所有 worker 节点上面都运行一遍。

## 配置 Kubectl

本部分将生成一个用于 admin 用户的 kubeconfig 文件。

> 注意：在生成 admin 客户端证书的目录来运行本部分的指令。

### admin kubeconfig

为 `admin` 用户生成 kubeconfig 文件：

```sh
# 请替换为你所用节点的 IP
export MASTER_IP=192.168.133.41

kubectl config set-cluster kubernetes-training \
  --certificate-authority=ca.pem \
  --embed-certs=true \
  --server=https://${MASTER_IP}:6443

kubectl config set-credentials admin \
  --client-certificate=admin.pem \
  --client-key=admin-key.pem

kubectl config set-context kubernetes-training \
  --cluster=kubernetes-training \
  --user=admin

kubectl config use-context kubernetes-training
```

### 验证

检查远端 Kubernetes 集群的健康状况:

```sh
kubectl get componentstatuses
```

输出为

```sh
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health":"true"}
```

列出远端 kubernetes cluster 的节点:

```sh
kubectl get nodes
```

输出为

```sh
NAME       STATUS     ROLES    AGE     VERSION
worker-1   NotReady   <none>   9m59s   v1.15.0
```

此时节点已经注册成功了，但节点的状态仍然是 `NotReady`，我们需要继续安装集群网络以使其 `Ready`。

## 配置 Pod 网络路由

每个 Pod 都会从所在 Node 的 Pod CIDR 中分配一个 IP 地址。由于网络还没有配置，跨节点的 Pod 之间还无法通信。

我们可以选择 [Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-achieve-this) 来实现 Kubernetes 网络模型。

### 安装

这里我们选择安装 [flannel](https://github.com/coreos/flannel)：

```sh
kubectl apply -f ./resources/kube-flannel.yaml
```

## 部署 DNS 扩展

本部分将部署 [DNS 扩展](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)，用于为集群内的应用提供服务发现。

### DNS 扩展

部属 `kube-dns` 群集扩展:

```sh
kubectl apply -f ./resources/coredns.yaml
```

列出 `kube-dns` 部署的 Pod 列表:

```sh
kubectl get pods -l k8s-app=kube-dns -n kube-system
```

输出为

```sh
NAME                       READY   STATUS    RESTARTS   AGE
coredns-699f8ddd77-94qv9   1/1     Running   0          20s
coredns-699f8ddd77-gtcgb   1/1     Running   0          20s
```

### 验证

建立一个 `busybox` 部署:

```sh
kubectl run busybox --image=busybox:1.28.3 --command -- sleep 3600
```

列出 `busybox` 部署的 Pod：

```sh
kubectl get pods -l run=busybox
```

输出为

```sh
NAME                      READY   STATUS    RESTARTS   AGE
busybox-d967695b6-29hfh   1/1     Running   0          61s
```

查询 `busybox` Pod 的全名:

```sh
POD_NAME=$(kubectl get pods -l run=busybox -o jsonpath="{.items[0].metadata.name}")
```

在 `busybox` Pod 中查询 DNS：

```sh
kubectl exec -ti $POD_NAME -- nslookup kubernetes
```

输出为

```sh
Server:    10.32.0.10
Address 1: 10.32.0.10 kube-dns.kube-system.svc.cluster.local

Name:      kubernetes
Address 1: 10.32.0.1 kubernetes.default.svc.cluster.local
```

## 烟雾测试

本部分将会运行一系列的测试来验证 Kubernetes 集群的功能正常。

### 数据加密

本节将会验证 [encrypt secret data at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#verifying-that-data-is-encrypted) 的功能。

创建一个 Secret:

```sh
kubectl create secret generic kubernetes-training \
  --from-literal="mykey=mydata"
```

查询存在 etcd 里 16 进位编码的 `kubernetes-training` secret:

```sh
ETCDCTL_API=3 etcdctl get \
  --endpoints=http://127.0.0.1:2379 \
  --cacert=/etc/etcd/ca.pem \
  --cert=/etc/etcd/kubernetes.pem \
  --key=/etc/etcd/kubernetes-key.pem\
  /registry/secrets/default/kubernetes-training | hexdump -C
```

输出为

```sh
00000000  2f 72 65 67 69 73 74 72  79 2f 73 65 63 72 65 74  |/registry/secret|
00000010  73 2f 64 65 66 61 75 6c  74 2f 6b 75 62 65 72 6e  |s/default/kubern|
00000020  65 74 65 73 2d 74 72 61  69 6e 69 6e 67 0a 6b 38  |etes-training.k8|
00000030  73 3a 65 6e 63 3a 61 65  73 63 62 63 3a 76 31 3a  |s:enc:aescbc:v1:|
00000040  6b 65 79 31 3a 3e 0e e6  d2 26 d8 26 1a a1 bf 15  |key1:>...&.&....|
00000050  69 d1 b9 dc dc da 54 26  6f 14 a3 48 4a 5d 00 22  |i.....T&o..HJ]."|
00000060  99 98 02 2a a0 e5 00 62  cc fa 49 de d4 d9 0c 35  |...*...b..I....5|
00000070  4c ed 66 1f 95 98 20 20  a1 0a 6d 1a b9 78 38 db  |L.f...  ..m..x8.|
00000080  14 68 29 5c b0 fb f1 1f  34 f4 c9 10 28 8c 38 09  |.h)\....4...(.8.|
00000090  05 81 81 91 f6 f2 84 5f  a2 8c cd 51 b8 2d 50 c8  |......._...Q.-P.|
000000a0  b2 2f a0 2a 25 0d fa 2a  5f d6 e5 8f 26 9d 67 0e  |./.*%..*_...&.g.|
000000b0  6f 95 85 8c f8 2c ff b6  f1 81 93 cf fa e8 9c ac  |o....,..........|
000000c0  15 1b 39 24 f0 0d 99 dd  70 f9 45 45 d4 3c 36 ee  |..9$....p.EE.<6.|
000000d0  3e d0 19 70 3c 81 1e 15  93 ad af 87 84 e7 64 ed  |>..p<.........d.|
000000e0  21 76 6a ce 0d 0a                                 |!vj...|
000000e6
```

Etcd 的密钥以 `k8s:enc:aescbc:v1:key1` 为前缀, 表示使用密钥为 `key1` 的 `aescbc` 加密数据。

### 部署

本节将会验证建立与管理 [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) 的功能。

创建一个 Deployment 用来搭建 [nginx](https://nginx.org/en/) Web 服务：

```sh
kubectl run nginx --image=nginx
```

列出 `nginx` deployment 的 pods:

```sh
kubectl get pods -l run=nginx
```

输出为

```sh
NAME                     READY   STATUS    RESTARTS   AGE
nginx-7bb7cd8db5-hqlf5   1/1     Running   0          6s
```

#### 端口转发

本节将会验证使用 [port forwarding](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) 从远端进入容器的功能。

查询 `nginx` pod 的全名:

```sh
POD_NAME=$(kubectl get pods -l run=nginx -o jsonpath="{.items[0].metadata.name}")
```

将本地机器的 8080 端口转发到 nginx pod 的 80 端口：

```sh
kubectl port-forward $POD_NAME 8080:80
```

输出为

```txt
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

开一个新的终端来做 HTTP 请求测试:

```sh
curl --head http://127.0.0.1:8080
```

输出为

```sh
HTTP/1.1 200 OK
Server: nginx/1.17.1
Date: Fri, 05 Jul 2019 09:58:49 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Tue, 25 Jun 2019 12:19:45 GMT
Connection: keep-alive
ETag: "5d121161-264"
Accept-Ranges: bytes
```

回到前面的终端并按下 `Ctrl + C` 停止 port forwarding 命令：

```sh
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
Handling connection for 8080
^C
```

#### 容器日志

本节会验证 [获取容器日志](https://kubernetes.io/docs/concepts/cluster-administration/logging/) 的功能。

输出 nginx Pod 的容器日志：

```sh
kubectl logs $POD_NAME
```

输出为

```sh
127.0.0.1 - - [05/Jul/2019:09:56:26 +0000] "HEAD / HTTP/1.1" 200 0 "-" "curl/7.54.0" "-"
```

#### 执行容器命令

本节将验证 [在容器里执行命令](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/#running-individual-commands-in-a-container) 的功能。

使用 `nginx -v` 命令在 `nginx` Pod 中输出 nginx 的版本：

```sh
kubectl exec -ti $POD_NAME -- nginx -v
```

输出为

```sh
nginx version: nginx/1.17.1
```

### 服务（Service）

本节将验证 Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/)。

将 `nginx` 部署导出为 [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport) 类型的 Service：

```sh
kubectl expose deployment nginx --port 80 --type NodePort
```

> LoadBalancer 类型的 Service 不能使用是因为没有设置 [cloud provider 集成](https://kubernetes.io/docs/getting-started-guides/scratch/#cloud-provider)。 设定 cloud provider 不在本教程范围之内。

查询 `nginx` 服务分配的 Node Port：

```sh
NODE_PORT=$(kubectl get svc nginx \
  --output=jsonpath='{range .spec.ports[0]}{.nodePort}')
```

使用 Node IP 地址 + nginx 服务的 Node Port 做 HTTP 请求测试：

```sh
curl -I http://${NODE_IP}:${NODE_PORT}
```

输出为

```sh
HTTP/1.1 200 OK
Server: nginx/1.17.1
Date: Sun, 07 Jul 2019 03:22:44 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Tue, 25 Jun 2019 12:19:45 GMT
Connection: keep-alive
ETag: "5d121161-264"
Accept-Ranges: bytes
```
