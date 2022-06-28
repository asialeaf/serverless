### 自动缩放


Knative Serving 为应用程序提供自动缩放或自动缩放，以匹配传入的需求。这是默认情况下使用 Knative Pod Autoscaler (KPA) 提供的。

例如，如果应用程序没有接收到流量并且启用了缩放为零，则 Knative Serving 将应用程序缩小到零副本。如果禁用缩放为零，则应用程序将缩小到为集群上的应用程序指定的最小副本数。如果应用程序的流量增加，副本会扩大以满足需求。





#### 功能

- 自动缩放器（KPA、HPA）
- Autoscaler 监视的指标类型(concurrency、rps、cpu、memory)
- Autoscaler 监视的指标类型的目标
- 缩放至0
- 配置缩放范围(min-scale和max-scale)
- 额外自动扩缩配置(window和panic)
- 配置container-freezer
- 流量切分
