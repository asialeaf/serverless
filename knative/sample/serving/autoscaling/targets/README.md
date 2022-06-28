# 目标

配置目标会为 Autoscaler 提供一个值，它会尝试为Revision的配置指标维护该值。

`target`用于配置每个Revision目标的注释。目标只是一个整数值，可以应用于任何指标类型。

## 配置目标

- **全局设置键：** `container-concurrency-target-default`
- **Revision注释键：** `autoscaling.knative.dev/target`
- **值：**一个整数
- **默认值：** `"100"`对于`container-concurrency-target-default`