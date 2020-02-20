# cloudnative-technical-manual

## 《云原生技术开发手册》

[![GitHub license](https://img.shields.io/github/license/shikanon/cloudnative-technical-manual)](https://github.com/shikanon/cloudnative-technical-manual/blob/master/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/shikanon/cloudnative-technical-manual)](https://github.com/shikanon/cloudnative-technical-manual/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/shikanon/cloudnative-technical-manual)](https://github.com/shikanon/cloudnative-technical-manual/network)


TOC
---------

- 基础篇
  - 基本概念
    - 什么是云原生
    - 后端软件架构的发展演变
    - 云原生发展历史
  - 云原生开发工程师的职责和基础技能
    - 云原生的技术范畴
    - 云原生开发工程师的工作职责
    - 云原生应用开发流程
  - 软件开发规范
    - 代码开发规范
      - 前端开发规范
      - 后端开发规范
        - [Python编程规范](./papers/development-specification/Python编程规范.md)
      - 数据库设计规范
    - 代码管理规范
      - 代码分支管理规范
      - 代码提交规范
      - CI/CD的使用规范
    - 代码审计规范
      - 如何做好code review
      - 代码审计目标和原则
      - 代码安全性审计
  - 运维规范及注意事项
- 进阶篇
  - 云原生的基石：容器与虚拟化技术
    - 虚拟化技术的发展历史
    - 容器基本命令
    - 容器引擎的实现及生命周期管理
  - 容器编排：Kubernetes
    - Kubernetes架构
    - Kubernetes安装
    - Kubernetes资源对象
    - Pod和容器设计模式
    - 无状态应用编排Deployment
    - 工作任务Job和DaemonSet
    - 有状态应用编排StatefulSet
    - 应用配置config
    - 存储资源Volume
    - Kuberntes网络及策略控制
  - Kubernetes开放接口
    - 容器运行时接口CRI
    - 容器网络接口CNI
    - 容器存储接口CSI
  - [Kubernetes集群故障排查](./papers/kubernetes-failure-analysis/)
    - [常见的问题排查方法](./papers/kubernetes-failure-analysis/常见的问题排查命令.md)
    - Pod异常排查
    - 网络异常排查
    - 持久化异常排查
  - 持续集成与持续交付
    - 代码管理: gitlab
    - CI/CD: Jenkins 和 git runner
    - 镜像管理: harbor
  - 服务网格: Istio
    - 什么是服务网格
    - Istio架构
    - Pilot服务发现
    - Gateway网关
    - 限流与熔断
    - 灰度发布
    - 分布式追踪
  - 监控系统：Prometheus
    - Prometheus架构
    - 搭建Prometheus系统
    - 监控应用程序
  - [机器学习工具集Kubeflow](./papers/machine-learning-toolset-kubeflow/)
    - [kubeflow核心组件](./papers/machine-learning-toolset-kubeflow/kubeflow核心组件.md)
    - TFJob
    - 模型部署：TF-Serving
    - 任务编排：Argo
    - [搭建kubeflow系统](./papers/machine-learning-toolset-kubeflow/搭建kubeflow系统.md)
- 实战篇
  - 向云原生迁移的第一个项目
    - 事前准备工作
      - 项目预研与集群规划
      - 准备集群环境
    - 容器化改造
    - 搭建自有镜像仓库
    - 编写资源配置文件
    - 构建持续集成环境
