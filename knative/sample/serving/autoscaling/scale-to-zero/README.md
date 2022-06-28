### 将缩放配置为零

##### 注意

只有在使用 KnativePodAutoscaler (KPA) 时才能启用缩放为零，并且只能全局配置。

#### 启用比例为零

缩放为零值控制 Knative 是否允许副本缩小到零（如果设置为`true`），或者如果设置为 则停止在 1 个副本`false`

- **全局键：** `enable-scale-to-zero`
- **Revision键：**无每修订设置。
- **值：**布尔值
- **默认：** `true`

#### 缩放到零宽限期

此设置指定了一个上限时间限制，系统将在内部等待从零开始缩放的机器在删除最后一个副本之前就位。

##### 注意

这是一个控制内部网络编程允许花费多长时间的值，并且只有在修订版缩放到零副本时遇到请求被丢弃的问题时才应进行调整。

此设置不会调整最后一个副本在流量结束后保留多长时间，也不能保证该副本实际上会保留整个持续时间。

- **全局键：** `scale-to-zero-grace-period`
- **Revision键：**不适用
- **值：**持续时间
- **默认：** `30s`

#### 缩放到零最后一个 pod 保留期

该`scale-to-zero-pod-retention-period`标志确定在 Autoscaler 决定将 pod 缩放为零后最后一个 pod 保持活动的最短时间。

这与标志形成对比，该`scale-to-zero-grace-period`标志确定在 Autoscaler 决定将 pod 缩放为零后最后一个 pod 保持活动状态的最长时间。

- **全局键：** `scale-to-zero-pod-retention-period`
- **Revision键：** `autoscaling.knative.dev/scale-to-zero-pod-retention-period`
- **值：**非负持续时间字符串
- **默认：** `0s`