# Serving 

### 自动伸缩

### 路由和网络编程

### 实时指向部署的代码和配置

### 滚动升级，A/B测试



### API

------



**Route**：定义网络端口，映射一个或多个revision

**Revision**：每次修改代码和配置产生的快照，不可更改。每个revision对应一次部署，并可以根据流量自动扩展

**Configuration**：维护部署的期望状态，每次修改configuration产生一个新的revision

**Service**：管理应用的生命周期，确保应用拥有configuration和route，并可以应用拥有configuration和route，并可以定义应用是使请求导向特定的revision（traffic配置）

<div align=center><img src=https://github.com/asialeaf/markdown/blob/main/images/knative-service.svg></div>