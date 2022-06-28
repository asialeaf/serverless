### 指标

指标配置定义了 Autoscaler 监视的指标类型。

#### 为每个Revision设置指标

对于每个Revision配置，是使用`autoscaling.knative.dev/metric`注释确定的。每个修订版可以配置的可能指标类型取决于您使用的 Autoscaler 实现类型：

- 默认的 KPA Autoscaler 支持`concurrency`和`rps`指标
- HPA Autoscaler 支持`cpu`指标




- **Revision注释键：** `autoscaling.knative.dev/metric`
- **值：** `"concurrency"`、`"rps"`、`"cpu"`或`"memory"`任何自定义指标名称，具体取决于您的 Autoscaler 类型。The `"cpu"`, `"memory"`, 和`"custom"` 指标仅在使用 HPA 类的Revision上受支持
- **默认：** `"concurrency"`

