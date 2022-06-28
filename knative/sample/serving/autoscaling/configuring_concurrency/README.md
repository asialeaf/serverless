### 配置并发

并发性决定了应用程序的每个副本在任何给定时间可以同时处理的请求数。

对于每次Revision并发，您必须同时添加`autoscaling.knative.dev/metric`和`autoscaling.knative.dev/target`配置对于软限制， `containerConcurrency`配置对于硬限制

对于全局并发，您可以设置该`container-concurrency-target-default`值



#### 软并发限制与硬并发限制

如果同时指定了软限制和硬限制，则将使用两个值中较小的一个。这可以防止自动缩放器具有硬限制值不允许的目标值。

软限制是有针对性的限制，而不是严格执行的限制。在某些情况下，特别是在请求突然*爆发*的情况下，可能会超过此值。

硬限制是强制的上限。如果并发达到硬限制，多余的请求将被缓冲，并且必须等到有足够的可用容量来执行请求。

##### 注意

仅当您的应用程序有明确的用例时，才建议使用硬限制配置。指定低硬限制可能会对应用程序的吞吐量和延迟产生负面影响，并可能导致额外的冷启动。



#### 软限制

- **全局键：** `container-concurrency-target-default`
- **Revision键：** `autoscaling.knative.dev/target`
- **值：**一个整数。
- **默认：** `"100"`

#### 硬限制

使用Revision规范中的字段为每个Revision指定硬限制。`containerConcurrency`此设置不是注释。

自动缩放 ConfigMap 中的硬限制没有全局设置，因为`containerConcurrency`它具有自动缩放之外的含义，例如请求的缓冲和排队。当然，可以在订在`config-defaults.yaml`中设置`containerConcurrency`字段的默认值

默认值为`0`，表示允许流入Revision的请求数量没有限制。大于 的值`0`指定允许在任何时候流向副本的请求的确切数量。

- **全局键：（** `container-concurrency`在`config-defaults.yaml`）
- **Revision键：** `containerConcurrency`
- **值：**整数
- **默认值：** `0`，表示没有限制