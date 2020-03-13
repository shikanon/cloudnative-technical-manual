# 搭建kubeflow系统

搭建kubelfow云原生机器学习系统是十分简单的，这里主要介绍两种情况：
- [国外站点安装kubeflow](#国外站点安装kubeflow)
- [国内站点安装kubeflow](#国内站点安装kubeflow)

## 国外站点安装kubeflow

国外站点安装kubeflow是十分容易的，只需要安装[官网的步骤](https://www.kubeflow.org/docs/started/k8s/kfctl-k8s-istio/)逐步操作即可，但需要注意 kubernetes的版本和kubeflow版本号的对应关系。

### 环境准备

kubeflow 为环境要求很高，看官方要求：
at least *one worker* node with a minimum of:
* 4 CPU
* 50 GB storage
* 12 GB memory

[kubernetes 与 kubeflow对应版本号](https://www.kubeflow.org/docs/started/k8s/overview/#minimum-system-requirements)：

| Kubernetes Versions | Kubeflow 0.4 |	Kubeflow 0.5 |	Kubeflow 0.6 |	Kubeflow 0.7 |	Kubeflow 1.0 |
|:---|:---|:---|:---|:---|:---|
|1.11 |	compatible |	compatible |	incompatible |	incompatible |	incompatible
|1.12 |	compatible |	compatible |	incompatible |	incompatible |	incompatible
|1.13 |	compatible |	compatible |	incompatible |	incompatible |	incompatible
|1.14 |	compatible |	compatible |	compatible |	compatible |	compatible
|1.15 |	incompatible |	compatible |	compatible |	compatible |	compatible
|1.16 |	incompatible |	incompatible |	incompatible |	incompatible |	incompatible


### 安装步骤

参考[官方文档](https://www.kubeflow.org/docs/started/k8s/kfctl-k8s-istio/)

#### 安装动态PVC

Kubelfow部署要求动态PV，这里采用[local-path-provisioner](https://github.com/rancher/local-path-provisioner)

安装`local-path-provisioner`[动态分配PV](https://github.com/rancher/local-path-provisioner)：
```
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

如果想直接在kubeflow中使用，还需要将`StorageClass`改为默认存储:
```
...
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-path
  annotations: #添加为默认StorageClass
    storageclass.beta.kubernetes.io/is-default-class: "true"
provisioner: rancher.io/local-path
volumeBindingMode: WaitForFirstConsumer
reclaimPolicy: Delete
...
```

完成后可以建一个PVC试试：
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: local-path-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

*注：如果没有设为默认storageclass需要在PVC加入`storageClassName: local-path`进行绑定*


### 用kfctl安装kubeflow

1. 下载 [kfctl](https://github.com/kubeflow/kfctl/releases)

2. 解压：
```
tar -xvf kfctl_<version>_<platform>.tar.gz
```
3. 创建环境变量，为kubeflow安装做准备：
```
# The following command is optional. It adds the kfctl binary to your path.
# If you don't add kfctl to your path, you must use the full path
# each time you run kfctl.
# Use only alphanumeric characters or - in the directory name.
export PATH=$PATH:"<path-to-kfctl>"

# Set KF_NAME to the name of your Kubeflow deployment. You also use this
# value as directory name when creating your configuration directory.
# For example, your deployment name can be 'my-kubeflow' or 'kf-test'.
export KF_NAME=<your choice of name for the Kubeflow deployment>

# Set the path to the base directory where you want to store one or more 
# Kubeflow deployments. For example, /opt/.
# Then set the Kubeflow application directory for this deployment.
export BASE_DIR=<path to a base directory>
export KF_DIR=${BASE_DIR}/${KF_NAME}

# Set the configuration file to use when deploying Kubeflow.
# The following configuration installs Istio by default. Comment out 
# the Istio components in the config file to skip Istio installation. 
# See https://github.com/kubeflow/kubeflow/pull/3663
export CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v1.0-branch/kfdef/kfctl_k8s_istio.v1.0.0.yaml"
```


1. 用kfctl安装kubeflow:
```
mkdir -p ${KF_DIR}
cd ${KF_DIR}
kfctl apply -V -f ${CONFIG_URI}
```

5. 查看kubeflow:
```
kubectl -n kubeflow get all
```

6. 删除kubeflow

kfctl删除kubeflow:
```
cd ${KF_DIR}
# If you want to delete all the resources, run:
kfctl delete -f ${CONFIG_FILE}
```

### 用Manifest部署kubeflow

除了用kfctl安装kbueflow，也可以直接下载 [Manifest](https://www.kubeflow.org/docs/started/getting-started/#configuration-quick-reference) 部署 kubeflow.


## 国内站点安装kubeflow

国内站点安装kubeflow主要存在镜像资源无法拉取的问题，这里可以我们在阿里云上传的容器镜像安装，这里我制作了一个[git项目](https://github.com/shikanon/kubeflow-manifests)。（这个项目是通过`kfctl build`生成的kustomize文件改造而来）

首先是[环境准备](#环境准备)，这里的环境准备和上一篇是一样的，主要是需要准备k8s集群和[动态PVC](#安装动态PVC)，这里不累赘。

1. 克隆项目:
```
git clone https://github.com/shikanon/kubeflow-manifests.git
```

2. 生成项目 YAML 文件
```
python run.py
```

3. 用 kubectl 安装 YAML 文件
```
cd yaml
kubectl apply -f .
```
完成！可以通过`kubectl get pod -nkubeflow`查看结果。
注：如果需要用local-path文件夹下的yaml文件生成：kubectl apply -f local-path-storage.yaml

4. 查看 kubeflow:
```
kubectl -n kubeflow get all
```

## 参考文献

- [Kubeflow官方文档](https://www.kubeflow.org/docs/started/k8s/kfctl-k8s-istio/): https://www.kubeflow.org/docs/started/k8s/kfctl-k8s-istio/