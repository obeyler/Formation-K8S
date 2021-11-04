# Principaux composants K8S
![schema](../images/architecture-K8S.svg)
- Control plane / Master
    - Apiserver
    - Kube-controller
    - Etcd
    - Scheduler
    - Kube-proxy
    - CRI Container Runtime Interface (Docker, ContainerD, CrIo)
    - Kubelet
- Data plane / Worker
    - KubeProxy
    - Kubelet
