# Knative

Knative在我们公司内以及很多容器，K8S相关的文章里经常被提及，我除了对这个名字百分百熟悉外，基本对它做什么是一无所知😶，不过晚知道强过不知道，今天赶紧来补一补课，先了解下基本概念。

## 为什么

我们团队一直在使用Kubernates，而Knative中的K代表的就是Kubernates，既然已经有这么火爆的k8s了，怎么又来一个Knative？这本电子书的[前言](https://www.servicemesher.com/getting-started-with-Knative/preface.html)已经给了一些介绍。k8s是一个事实上的容器集群管理平台，它负责帮用户妥善的运行和管理容器，但是它不关心容器的基础镜像是怎么构建的，容器内的应用程序怎么运行？如何扩展？这些都是由用户来决定的，为了提供一个对开发人员更友好的平台，Knative基于k8s之上尝试来解决这些问题，为整个开发生命周期提供帮助。

## 是什么

Knative使开发人员能够以想要的语言和想要的方式来编写代码，其次帮助开发人员构建和打包应用程序，最后帮助运行和伸缩应用程序。它有三个关键组件：

* build（构建）从源到容器的构建编排应用程序。
* serving（服务）为应用程序提供流量，请求驱动的计算，可以缩放到零。
* event（事件）确保应用程序能够轻松地生产和消费事件。

### Build(构建)

Build组件解决如何从源代码到容器.核心组件有三个: service account,build resource,  build template.

* Service Account（服务账户）
利用k8s原生的资源类型secret和service account来提供构建时获得需要验证的服务的凭证。比如从私有代码库拉取源代码，push构建好的镜像到docker hub或者私有仓库。
* Build Resource（构建资源）
驱动构建过程的自定义Kubernetes资源。在定义构建时，您需要定义如何获取源代码以及如何创建容器镜像来运行代码。
* Build Template（构建模板）
构建模板是可共享的、封装的、参数化的构建步骤集合。目前支持的template包括：

1. Kaniko:在运行的容器中构建容器镜像，而不依赖于运行Docker daemon 。
2. Jib:为 Java 应用程序构建容器镜像。
3. Buildpack: 自动检测应用程序的运行时，并建立一个容器镜像使用Cloud Foundry Buildpack。

### Serving(服务)

Serving 模块定义一组特定的对象以控制所有功能：Revision（修订版本）、Configuration （配置）、Route（路由）和 Service（服务）。Knative 使用 Kubernetes CRD（自定义资源）的方式实现这些 Kubernetes 对象。这里的Service管理工作负载的整个生命周期。包括部署、路由和回滚。和k8s里的service属于两种资源，详细介绍参考[这里](https://www.servicemesher.com/getting-started-with-knative/serving.html)。估计大部分功能实现可能都依赖于istio，等下次动手实验的时候再详细了解。

### Eventing(事件)

Eventing组件的三个核心概念是Source（源）、Channel（通道）和 Subscription（订阅）。

1. Source是事件的来源，它是我们定义事件在何处生成以及如何将事件传递给关注对象的方式。  
  目前支持GCP PubSub，Kubernetes Event，GitHub，Container Source。
1. Channel通道处理缓冲和持久性，即使该服务已被关闭时也确保将事件传递到其预期的服务。  
  支持in-memory-channel，GCP PubSub，Kafka，NATS
1. Subscriptions订阅是通道和处理来自通道的events的服务之间的纽带。  

_20191021_