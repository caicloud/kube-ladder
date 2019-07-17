# Kubernetes 学习路径

## 目录

   * [Kubernetes 学习路径](#kubernetes-学习路径)
      * [背景](#背景)
      * [学习路径](#学习路径)
         * [第一阶段 炼气期（2-4 周，每周 3-5 小时）](#第一阶段-炼气期2-4-周每周-3-5-小时)
            * [目标](#目标)
            * [路径](#路径)
            * [心法](#心法)
         * [第二阶段 筑基期（4-6 周，每周 8-10 小时）](#第二阶段-筑基期4-6-周每周-8-10-小时)
            * [目标](#目标-1)
            * [路径](#路径-1)
            * [心法](#心法-1)
         * [第三阶段 金丹期（2-4 周，每周 3-5 小时）](#第三阶段-金丹期2-4-周每周-3-5-小时)
            * [目标](#目标-2)
            * [路径](#路径-2)
            * [心法](#心法-2)
         * [第四阶段 元婴期（4-6 周，每周 8-10 小时）](#第四阶段-元婴期4-6-周每周-8-10-小时)
            * [目标](#目标-3)
            * [路径](#路径-3)
            * [心法](#心法-3)
         * [第五阶段 化神期（4-6 周，每周 8-10 小时）](#第五阶段-化神期4-6-周每周-8-10-小时)
            * [目标](#目标-4)
            * [路径](#路径-4)
            * [心法](#心法-4)
         * [第六阶段 大乘期（终身学习）](#第六阶段-大乘期终身学习)
            * [目标](#目标-5)
            * [路径](#路径-5)
            * [心法](#心法-5)
      * [许可协议](#许可协议)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

## 背景

**本文由 [才云科技（Caicloud）](https://caicloud.io/)  于 2019 年内部推出，现以开源的形式进行维护**

目前云计算行业对于 Kubernetes 学习的需求日益增加，但市面上关于 Kubernetes 的资源良莠不齐，存在几个问题：

- 官方文档缺少明确的"梯度"，信息错综复杂
- 资料较为分散，查找信息费时费力
- Kubernetes 发展很快，书籍或者网上教程容易过时

本文档旨在为广大从业者提供一个 Kubernetes 学习路径，为大家提供一定的指引。我们最终的目标是让所有人剥茧抽丝般地了解 Kubernetes，不仅仅知道怎么用 Kubernetes，还知道 Kubernetes 各个功能是如何设计的。在学习路径后期，我们还可以很"自然"的联想到正确的设计思路。

## 学习路径

**注意**：

- 术语来自才云科技工程师 [gaocegege](https://github.com/gaocegege) 所著《[适合系统工程师的"机器学习"学习路径](https://github.com/caicloud/mlsys-ladder)》
- 注意这是学习路径，不是一个教程！
- 大多数概念都会给出官方链接，如果需要深入了解请自行查找！

### 第一阶段 炼气期（2-4 周，每周 3-5 小时）

#### 目标

- Kubernetes 的背景
- 安装 Kubernetes 环境
- Kubernetes 基本概念和使用方法

#### 路径

学习任何系统的之前，了解其出现的背景和意义都是必不可少的，为什么会出现 Kubernetes？它解决了什么问题？有没有其他类似的系统？这里推荐阅读才云科技 CEO 张鑫在 2017 年文章《[从风口浪尖到十字路口，写在Kubernetes 两周年之际](https://mp.weixin.qq.com/s/hrgXzt7YKVf6ZCFzJ-WTFA)》。

接下来，在了解 Kubernetes 系统本质之前，我们需要对 Kubernetes 有一个较为"感性"的认识，打消对 Kubernetes 的畏难情绪。这里，我们推荐使用 [minikube](https://github.com/kubernetes/minikube) 或 [kind](https://github.com/kubernetes-sigs/kind) 部署一个本地环境，然后开始部署一个"真实"的应用（minikube 安装需要使用科学上网）。如果想一开始就挑战更高难度的安装方式（不推荐），可以使用 kubeadm，local-cluster 或者手动部署所有组件。关于安装，可以参考文档 [lab1-installation](https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab1-installation.md)。

在安装好环境之后，可以开始动手实践最基本的 Kubernetes 概念。在第一阶段，我们推荐熟练使用以下常用资源和概念：Pod、Node、Label、Event、Service、Configmap & Secret、Deployment、Namespace。相关学习可以参考文档 [lab2-application-and-service](https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab2-application-and-service.md)。

（可选）仅完成上述内容可能还不足以让我们非常熟悉 Kubernetes 的基本概念，下面列出其他可以参考的资料，大家也可以按照自己的方式去搜索相关的资料：

- 官方 Tutorial：[Learn Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- 官方 Guestbook 样例：[Guestbook Example](https://kubernetes.io/docs/tutorials/stateless-application/guestbook/)

#### 心法

<p align="center" style="font-size: 50px">🤨</p>

- 请反复加深对上面资源的操作熟练度。如果你是第一次接触 Kubernetes，或者仅了解过一点 Kubernetes 的知识，那么基（ken）本（ding）是不明白 Kubernetes 底层到底发生了什么。请不要心急，姑且把它当成一个黑盒工具即可 🛠。
- 你可能会在网上看到更多的概念，如 PVC、Ingress、Priority 等。练气阶段，请不要尝试学习过多的资源类型。Kubernetes 有非常多的概念和类似的资源，我们这里熟悉最核心的概念即可，否则易走火入魔 👻，切记。当我们打通任督二脉之时，所有的新概念都不过尔尔。

### 第二阶段 筑基期（4-6 周，每周 8-10 小时）

#### 目标

- Kubernetes 的基本架构
- Kubernetes 容器调度的基本流程

#### 路径

短暂接触 Kubernetes 概念之后，我们需要知其然并且知其所以然，因此在第二阶段我们开始学习 Kubernetes 基本架构。学习 Kubernetes 基本架构至少需要了解以下内容：

- Master & Node
  - 知道什么是 Kubernetes Master，什么是 Node
  - 知道两者的关系，知道他们是如何通信的
- Master 组件
  - API Server。Kubernetes 如何接收请求，又是如何将结果返回至客户端。
  - Controller Manager。Kubernetes 控制器是其架构中最为核心的一环，我们需要了解控制器的原理，List-Watch 的基本原理，知道 Kubernetes 默认情况下大致包含哪些类型的控制器。
  - Scheduler。熟悉 Kubernetes 的调度流程是怎样的，调度器在整个调度流程中的角色。
- Node 组件
  - Kubelet。知道 Kubelet 是如何接受调度请求并启动容器的。
  - Kube-proxy。了解 Kube-proxy 的作用，提供的能力是什么。
- 核心 Addons & Plugins
  - DNS。DNS 为集群的服务发现提供的支持，最新版 Kubernetes 使用 CoreDNS。
  - Network Plugin。Kubernetes 多节点环境需要部署网络插件才可以使用，默认情况下使用 flannel 即可。

首先可以阅读书籍或网上博客，推荐阅读：

- 官方文档：[Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)
- feisky 的博客：[Kubernetes 指南之核心原理](https://kubernetes.feisky.xyz/he-xin-yuan-li/index)
- kubectl run 的背后流程（难）：[What happens when I type kubectl run?](https://github.com/jamiehannaford/what-happens-when-k8s)

接下来，推荐从 0 开始部署一个 Kubernetes 集群（不使用任何工具），来加深对各个组件的理解：解决部署中出现的各种问题，查看组件启动日志等等。如果时间有限，也可以尝试使用 kubeadm 等工具来部署集群。目前 Kubernetes 集群部署自动化已经做得比较完善，但出于学习目的，再次墙裂推荐手动安装。关于手动安装集群，可以参考文档 [lab3-manual-installtion](https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab3-manual-installtion.md)。

在本阶段修炼结束后，我们至少应该对以下问题了如指掌：Kubernetes 组件是如何交互，来启动容器，并对外提供服务的？

#### 心法

<p align="center" style="font-size: 50px">💪</p>

- 请不要死记硬背 Kubernetes 架构，要开动大脑 🧠去理解其背后设计的原因。

- 筑基期是比较困难的一个阶段，如果感觉一头雾水，请不要气垒，你不是一个人。当你感觉进入了瓶颈时，可以尝试寻找身边的战友，总结一些你的问题并寻求答案 🍻。

### 第三阶段 金丹期（2-4 周，每周 3-5 小时）

#### 目标

- Kubernetes API 结构
- 熟悉 Kubernetes 各个子系统
- 熟悉 Kubernetes 排错相关内容

#### 路径

当我们可以熟练使用 Kubernetes 的基本资源，并且对 Kubernetes 的基本架构有了充足了认识，接下来需要对 Kubernetes 的 API 结构和子系统要有一个比较全面的认识，同时也要开始更加系统的了解排查问题相关的内容。

要知道 [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) 是其最引以为傲的设计，掌握 API 的关键是需要了解：

- Kubernetes 的 API 是如何控制版本的
- Kubernetes 的 API 是如何分组的
- [Kubernetes 对象](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)的表示方法与设计理念
- [Kubernetes API 的访问控制](https://kubernetes.io/docs/reference/access-authn-authz/controlling-access/)

我们可以通过浏览 [Kubernetes API](https://github.com/kubernetes/api) 代码仓库来了解 Kubernetes API 组（Group）的信息。所有的资源定义代码都遵循 `<group>/<version>/types.go` 的规范，例如上述 Deployment 资源是定义在 [apps group](https://github.com/kubernetes/api/tree/master/apps) 中。我们可以在 [apps/v1/types.go](https://github.com/kubernetes/api/blob/master/apps/v1/types.go) 中查找到关于 Deployment 的定义。

接下来，我们可以通过浏览 [Kubernetes/Community](https://github.com/kubernetes/community) 代码仓库来了解各个兴趣小组（SIG）。Kubernetes 的演进都是通过 SIG 来推动的，因此了解 SIG 的分工对我们理解 Kubernetes 非常重要。一般来讲，一个 SIG 对应着一个 Kubernetes 子系统，例如，[sig-apps](https://github.com/kubernetes/community/tree/master/sig-apps) 负责决定是否引入新的 API，或者现有 API 是否需要升级等等。我们通过查看 Community 中带有 "sig-" 前缀的目录来了解 SIG 的工作内容、会议纪要等等。这里简单列举 Kubernetes 重要的子系统：

- 架构 Architecture
- 应用 Apps
- 存储 Storage
- 网络 Network
- 权限 Auth
- 节点 Node
- 调度 Scheduling
- 命令行 CLI
- 多集群 MultiCluster
- 云平台 CloudProvider
- 扩展性 Scalability
- 弹性伸缩 Autoscaling
- 监控日志 Instrumentation

（可选）细心的你可能会发现以 "wg-" 开头的目录，例如 "wg-resource-management"。"wg" 简称 working group，是针对需要涉及多个 SIG 合作而展开的一种工作组，独立于任何 SIG 。例如 resource management 会涉及到 Node、Storage、Scheduling 等 SIGs。

作为一个承上启下的阶段，我们需要总结一些 Kubernetes 排错的能力，推荐阅读：

- 官方文档：[Kubernetes Troubleshooting](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/)

- feisky 的博客：[Kubernetes 集群排错指南](https://feisky.gitbooks.io/kubernetes/troubleshooting/)

#### 心法

<p align="center" style="font-size: 50px">🌱</p>

- 本阶段的重点是"耳听六路 🙈眼观八方" 。前两个阶段接触到了很多 Kubernetes 的细节，本阶段需要对 kubernetes 的全貌有个更加清晰的认识。很多内容可能看不太懂，但请在你的心中埋下一颗种子。

### 第四阶段 元婴期（4-6 周，每周 8-10 小时）

#### 目标

- 加深对各个资源的理解
- 学习 Kubernetes 集群能力
- 学习更多 Kubernetes 概念和知识

#### 路径

不出意外，你现在对 Kubernetes 基本的资源已经很熟练了，对 Kubernetes 内部组件和他们的交互比较清晰，还对 Kubernetes 的 API 全貌和组织结构也有一定的了解。如果出了意外 🤔，请重新回顾你的学习过程。

本阶段，我们围绕几个关键方向来学习 Kubernetes，加深对其各个技术点的认识（这里只列出本阶段需要学习的核心能力，其他功能请量力而学）：

- 计算
  - [Pod Lifecycle & Healthcheck & RestartPolicy](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)
  - [Pod Init-Container](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)
  - [Horizontal Pod Autoscaler (HPA)](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
  - 更多控制器：[Job](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)、[CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)、[Daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)、[StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)、[ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

- 网络
  - [Service](https://kubernetes.io/docs/concepts/services-networking/service/) 实现，主要了解 [iptables](https://linux.die.net/man/8/iptables) 的实现（可选：[ipvs](https://kubernetes.io/blog/2018/07/09/ipvs-based-in-cluster-load-balancing-deep-dive/) 模式）
  - [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) 原理和常用的 [nginx ingress](https://github.com/kubernetes/ingress-nginx) 操作
  - [DNS](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/) 服务的原理，以及 [CoreDNS](https://coredns.io/) 方案

- 存储
  - [Volume](https://kubernetes.io/docs/concepts/storage/volumes/) 以及底层存储类型
  - [PV/PVC](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)、[StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/)

- 安全
  - [AuthN](https://kubernetes.io/docs/reference/access-authn-authz/authentication/), [AuthZ](https://kubernetes.io/docs/reference/access-authn-authz/authorization/) & [Admission Control](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)
  - [NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
  - [ServiceAccount](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)
  - [Pod/Container SecurityContext](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)
  - [Pod Security Policy (PSP)](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)

- 调度
  - [Pod/Node Affinity & Anti-affinity](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity)
  - [Taint & Toleration](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)
  - [Priority & Preemption](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)
  - [Pod Disruption Budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)

总结一下，1）本阶段我们接触到更多的资源，包括：HPA、Job、CronJob、DaemonSet、StatefulSet、Ingress、Volume、PV/PVC、StorageClass、NetworkPolicy、PSP。2）更加深入了解已学资源的使用，例如 Init-Container、SecurityContext、Affinity 等。这些能力最终都会体现在各个资源的 API 上，例如 Affinity 是 Pod API 结构的一个字段，Scheduler 通过解析这个字段来进行合理的调度。未来如果有更多的能力，我们都可以通过解读不同资源的 API 字段来一探究竟。

本阶段相关学习可以参考文档 [lab4](https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab4-concepts.md)。

#### 心法

<p align="center" style="font-size: 50px">🧘‍♂️🧘‍♀️</p>

- 本阶段难度指数高，请合理调整你的心境。渡劫成功后，你对 Kubernetes 的掌握将会进入一个新的台（tian）阶（keng）。
- 学习相关功能时，可以回顾其所在 SIG，看看能不能发现有用的资源。

### 第五阶段 化神期（4-6 周，每周 8-10 小时）

#### 目标

- 对 Kubernetes 的扩展机制了如指掌
- 可以编写 Kubernetes 控制器，能够基于扩展机制灵活地二次开发

#### 路径

本阶段我们可以开始了解 Kubernetes [各种扩展机制](https://kubernetes.io/docs/concepts/extend-kubernetes/extend-cluster/)。如果说 Kubernetes 的 API 和架构设计是其重要的基石，那么扩展机制使得 Kubernetes 在各个生态领域开花结果。下面我们尝试列举出所有的扩展方式，每一种扩展都有其优势和局限性，请自行思考。注意这里提到的扩展机制指的是架构上的扩展，而非功能层面的扩展，例如 Pod 支持各种 Probe 来进行健康检查，包括自定义，这里我们不归为扩展机制的能力。

API 资源扩展能力

- [Annotation](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)：保存少量非结构化第三方数据
- [Finalizer](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/#finalizers)：资源删除时，用户调用外部系统的钩子
- [CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/)：自定义 Kubernetes API
- [API Aggregation](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/)：多个 API Server 聚合，适用于较大量的 API 定制

学习 API 资源扩展的一个重要方式是创建一个扩展资源，或者编写一个自己的控制器。强烈推荐自行编写一个控制器，这里列出几个常见的工具：

- [kubebuilder](https://book.kubebuilder.io/)：来自 Kubernetes 官方的 API 扩展项目
- [sample-controller](https://github.com/kubernetes/sample-controller)：来自 Kubernetes 官方的一个样例
- [operator-sdk](https://github.com/operator-framework/operator-sdk)：来自红帽的一个 operator 库
- [shell-operator](https://github.com/flant/shell-operator)：适合运维开发使用的 shell operator 库
- [meta-controller](https://metacontroller.app/)：来自 Google 的一个更加"傻瓜"式编写控制器的库

API 访问扩展能力

- [认证 Webhook](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication)：用户认证时，调用外部服务，仅支持静态配置
- [鉴权 Webhook](https://kubernetes.io/docs/reference/access-authn-authz/webhook/)：用户鉴权时，调用外部服务，仅支持静态配置
- 动态[访问控制 Webhook](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/)：请求访问控制时，调用外部服务，支持动态增加外部服务

Kubernetes API 访问扩展主要是通过 Webhook 来实现。注意只有访问控制支持动态增加外部服务，认证鉴权的外部服务在启动 API Server 的时候就注册完毕，无法在后续增加，主要原因是动态增加外部认证鉴权服务，带来的安全风险过大。

调度器扩展能力

- [扩展接口（Scheduler Extender）](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/scheduler_extender.md)：类似 Webhook，调用外部服务进行调度决策
- [多调度器](https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/)：支持在 Kubernetes 运行多个调度器调度不同作业

针对简单场景，我们可以直接使用 Scheduler Extender 即可，例如按 GPU 型号调度。复杂调度场景可以使用多调度器，例如基于流图的调度器 [poseidon](https://kubernetes.io/docs/concepts/extend-kubernetes/poseidon-firmament-alternate-scheduler/)。一般而言，使用 Extender 即可满足大多数场景。

网络扩展能力

- [网络插件 CNI](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)：使用 CNI 插件，可以选择任何我们需要的[网络方案](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
- [自定义 Ingress 控制器](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)：Ingress 定义了一套 API 接口，我们可以选择任意实现
- [自定义 NetworkPolicy 控制器](https://kubernetes.io/docs/concepts/services-networking/network-policies/)：同上，可选实现包括 [Calico](https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/calico-network-policy/)、[Cilium](https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/cilium-network-policy/) 等
- [自定义 DNS 控制器](https://github.com/kubernetes/dns/blob/master/docs/specification.md)：Kubernetes 定义了一套 DNS 规范，我们可以选择任意实现

网络插件 CNI 是容器网络标准，Kubernetes 提供了良好的支持，常用插件包括 flannel、Calico 等等。对于 Ingress、NetworkPolicy、DNS，相信到目前为止大家应该可以理解，其本质上是 Kubernetes 定义的一套 API，底层实现可插拔，用户可以有自己的选择。

存储扩展能力

- [FlexVolume](https://kubernetes.io/docs/concepts/storage/volumes/#flexVolume)：Kubernetes 提供的一种动态对接存储方案，支持用户自定义存储后端
- [存储插件 CSI](https://kubernetes-csi.github.io)：使用 CSI 插件，可以选择任何我们需要的存储方案

FlexVolume 是 Kubernetes 自带的对接外部存储的方案，用户编写少量的代码即可加入自定义存储后端，适用于简单场景。存储插件 CSI 是容器网络标准，Kubernetes 提供了良好的支持，同时为方便第三方实现，还提供了一整套 SDK 解决方案。所有底层存储相关的能力都与 CSI 密切相关。

运行时扩展能力

- [运行时接口 CRI](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)：使用 CRI 插件，可以选择任何我们需要的运行时方案

运行时接口 CRI 是 Kubernetes 提出，为解决支持多种运行时而提供的方案。任何运行时，只需实现 CRI 相关的接口，即可接入 Kubernetes 中，包括 Docker、Containerd、gVisor、Kata 等。

特殊硬件或资源扩展能力

- [扩展资源 Extended Resource](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#extended-resources)：通过 Kubernetes 原生 API 方式支持添加自定义资源
- [设备插件 Device Plugin](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/device-plugins/)：使用 Device Plugin 插件，可以对接任何我们需要的硬件

对于简单场景，例如静态汇报资源数量，可以直接使用 Extended Resource 扩展 Kubernetes 所支持的硬件。Device Plugin 的核心是自动接入各种特殊硬件如 Nvidia、Infiniband、FPGA 等。在资源汇报层面 Device Plugin 目前也使用了 Extended Resource 的能力，但由于 Extended Resource 的局限性，Device Plugin 未来也可以与其他 API 对接。目前使用最多的 Device Plugin 主要是 Nvidia 的 [GPU device plugin](https://github.com/NVIDIA/k8s-device-plugin)。

监控扩展能力

- [自定义监控](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#support-for-metrics-apis)：支持使用自定义监控组件如 Prometheus 提供监控指标

自定义监控包括 Custom Metrics 和 External Metrics，例如 [Prometheus adaptor](https://github.com/DirectXMan12/k8s-prometheus-adapter)。

云供应商扩展能力

- [云控制器 Cloud Controller Manager](https://kubernetes.io/docs/concepts/architecture/cloud-controller/)：支持可插拔云服务提供商

云扩展能力的目标是使各个云供应商可以在不改变 Kubernetes 源码的情况下，接入其服务。每个云供应商都有[独立的项目](https://github.com/kubernetes?utf8=%E2%9C%93&q=cloud-provider&type=&language=)。

命令行插件

- [Kubectl Plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/)：kubectl plugin 支持扩展 kubectl 子命令，与 API 扩展能力结合可以提供近乎原生的使用方法。

#### 心法

<p align="center" style="font-size: 50px">😇</p>

- 推荐实现一个端到端的 Kubernetes 控制器，可以对整个 Kubernetes 的二次开发有更加深入的了解。此外，针对所有的扩展能力，建议先建立一个全面的认识，再根据需要深入某一项能力。
- 我们除了通过用户手册来学习上面的技术，也可多参考 Kubernetes 的花式设计文档，主要是 [Design Proposals](https://github.com/kubernetes/community/tree/master/contributors/design-proposals)、[KEPs](https://github.com/kubernetes/enhancements/tree/master/keps)。

### 第六阶段 大乘期（终身学习）

#### 目标

- 了解 Kubernetes 生态项目
- 跟踪 Kubernetes 社区发展
- 跟踪 CNCF 社区发展

#### 路径

目前为止，我们学习了很多 Kubernetes 的概念，但也只是其最重要的部分。在本阶段，我们需要专注以下几个问题：

- 如何跟进 Kubernetes 的新功能，以及现有功能的更多细节？
- 如何了解 Kubernetes 整个生态环境的发展？

首先，让我们一起来学习几个重要的项目。围绕 Kubernetes 的生态环境建设是其成为容器标准的关键。

- [Helm](https://github.com/helm/helm)：作为 Kubernetes 生态里的 brew、dnf、dpkg，Helm 为 Kubernetes 提供了包管理能力，方便用户快速部署安装各种服务。
- [Harbor](https://github.com/goharbor/harbor)：Harbor 与 Kubernetes 无直接关系，但作为云原生环境下最常用的镜像仓库解决方案，了解 Harbor 十分重要。
- [Prometheus](https://prometheus.io/)：Prometheus 是云原生环境下最重要的监控组件。
- [Istio](https://istio.io/)：istio 是服务网格的关键项目，但较为复杂，可以尝试简单了解。

以上，我们仅列出了极少量的重要项目，Kubernetes 周边的项目十分之多，令人咂舌 😱。因此大乘期的你，需要开始持续跟踪 Kubernetes 及其生态的发展，甚至可以推动其发展，接下来我们列举一些靠谱资源：

GitHub 仓库

- [Kubernetes Enhancement](https://github.com/kubernetes/enhancements/)：关注新特性的讨论
- [Kubernetes Community](https://github.com/kubernetes/community)：关注社区组织情况
- [CNCF TOC](https://github.com/cncf/toc/)：关注 CNCF 进展，各种新项目讨论等
- [Awesome Kubernetes](https://github.com/ramitsurana/awesome-kubernetes)：Kubernetes 项目之学不动系列

关注 GitHub 仓库可以让你了解最一手的进展，但是信息量一般较大，讨论很多难度也比较大。不过对于大乘期的你来讲，应该不是问题 😉。另外，这里还包含很多 Kubernetes 系统内部的设计，例如调度器的优化方案、资源垃圾回收方案等，值得了解和学习。

Twitter 账号

下面推荐几个 Kubernetes 项目的核心人员。大牛都喜欢用 Twitter 交（si）流（bi），可以关注一波。感兴趣的话题可以去交流，大牛都十分耐撕（nice。

- [Tim Hockin](https://twitter.com/thockin)
- [Clayton Coleman](https://twitter.com/smarterclayton)
- [Daniel Smith](https://twitter.com/originalavalamp)
- [Brian Grant](https://twitter.com/bgrant0607)
- [Vishnu Kannan](https://twitter.com/vishnukanan)
- [Saad Ali](https://twitter.com/the_saad_ali)
- [Kelsey Hightower](https://twitter.com/kelseyhightower)
- [Joe Beda](https://twitter.com/jbeda)
- [Brendan Burns](https://twitter.com/brendandburns)
- [Michelle Noorali](https://twitter.com/michellenoorali)

除此之外，Twitter 上还有不少项目和其他 Weekly 性质的 Twitter，推荐几个账号关注：

- [Kube Weekly](https://twitter.com/kubeweekly)
- [Kube List](https://twitter.com/readkubelist)

Twitter 会根据你的喜好推荐其他相关内容，接下来就自由发挥。

Blog 账号

- [Kubernetes Blog](https://kubernetes.io/blog/)
- [CNCF Blog](https://www.cncf.io/category/blog/)

可以关注的优秀 Blog 很多，这里就不一一列举。

#### 心法

<p align="center" style="font-size: 50px">🐉</p>
<p align="center" style="font-size: 18px">请坚持学习！送上一句黑鸡汤：</p>
<p align="center" style="font-size: 30px">"The last thing you want is to look back on your life and wonder... if only."
</p>

## 许可协议

- 本文遵守[创作共享 CC BY-NC-SA 3.0协议](https://creativecommons.org/licenses/by-nc-sa/3.0/cn/)
- 商业目的转载，请联系 <marketing@caicloud.io>
- 如有任何版权问题，请联系 <deyuan@caicloud.io> 和 <baomengjiang@caicloud.io>
