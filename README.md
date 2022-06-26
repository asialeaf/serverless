### FaaS

#### Serverless Computingd定义

Serverless computing = FaaS + BaaS



#### Serverless领域开源项目

- **Knative**：起步较早，标准的Serverless平台，但仅能运行应用，不能运行函数，不能完全定义为FaaS。Knative 使用 Build 提供云原生“从源代码到容器”的镜像构建能力，通过 Serving 部署容器并提供通用的服务模型，同时以 Eventing 提供事件全局订阅、传递和管理能力，实现事件驱动。这是 Knative 呈现给我们的标准 Serverless 编排框架。
  - ***Build***：一个底层的构建模块，基于现有的 Kubernetes 能力，支持 Source 源挂载，如Git仓库；支持BuildTemplate；支持 K8s ServiceAccount 身份验证
  - ***Serving*** ：快速部署 Serverless 容器；支持自动扩缩容和缩到 0 实例；基于 Istio 组件，提供路由和网络编程；支持部署快照
    - Autoscaler：支持从N ->0
  - Knative ***Eventing***：优秀的事件管理框架
    - 事件源（Event Source）
    - 支持事件接收/转发（Flow）
    - 事件消费者（Event Consumer）
- **OpenFaaS**：流行的FaaS项目，但是依赖Prometheus和Alertmanager来进行Autoscaling，这一点不是不是很专业，相比Knative有点不足。OpenFaaS 分为 CE、PRO、Enterprise。PRO需要 OpenFaaS评审获取License，社区版功能较少，PRO才会提升功能
  - ***Serving***：API Gateway / UI Portal ；Template Store or a Dockerfile
    - Autoscaler：Prometheus + Alertmanager
  - ***Build***： OCI-compatible/Docker image
  - ***Events***：多种事件源触发
- **OpenFunction**：国内开源FaaS项目，较新，整合了Knative的Serving Runtime。
  - ***Serving***
    - Async runtime ：Dapr（微软捐赠的分布式运行时） + KEDA（自动扩缩容工具，支持各种数据源）
    - Sync runtime：Knative + KEDA-HTTP
  - ***Build***：使用Shipwright（Kubernetes 容器镜像构建框架），Shipwright整合了Kaniko、BuildKit、buildah、Buildpacks
  - ***Events***：自研





##### Function 参考架构/生命周期

<div align=center><img src=https://github.com/asialeaf/markdown/blob/main/images/function-lifecycle.svg></div>

##### Faas实现

- Function Framework：将函数转换成应用

- Function Build：函数构建

  - Build pipline：如Tekton
  - Build Image：Buildpacks、buildah、buildkit、kaniko；Shipwright

- Function Serving

  - 同步函数
    - 运行时
  - 异步函数
    - 运行时

- Function Autoscaling

  ​	Kubernets HPA (Horizontal Pod Autoscaler) ：1->N。不能从N->0，也不能从0->N

  - Knative Serving：N->0，HTTP事件源

  - KEDA、KEDA-HTTP：多种事件源，0->N，N->0


- Function EventSource

    

    