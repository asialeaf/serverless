### 支持的自动缩放器类型

Knative Serving 支持 Knative Pod Autoscaler (KPA) 和 Kubernetes 的 Horizo​​ntal Pod Autoscaler (HPA) 的实现。本主题列出了每个自动缩放器的功能和限制，以及如何配置它们。

##### 注意

如果要使用 Kubernetes Horizo​​ntal Pod Autoscaler (HPA)，则必须在安装 Knative Serving 之后安装它。

有关如何安装 HPA，请参阅安装可选服务扩展。

#### Knative Pod 自动缩放器 (KPA)

Knative Serving 核心的一部分，安装 Knative Serving 后默认启用。
支持缩放到零功能。
不支持基于 CPU 的自动缩放。

#### 水平 Pod 自动缩放器 (HPA)

不是 Knative Serving 核心的一部分，您必须先安装 Knative Serving。
不支持缩放到零功能。
支持基于 CPU 的自动缩放。

#### 配置 Autoscaler 实现

可以使用class注解配置 Autoscaler 实现的类型（KPA 或 HPA）。

- 全局设置键： pod-autoscaler-class
- 每修订注释键： autoscaling.knative.dev/class
- 值： "kpa.autoscaling.knative.dev"或"hpa.autoscaling.knative.dev"
- 默认： "kpa.autoscaling.knative.dev"