# Programme de la formation
## Docker
- [Docker file](Chapitres/DockerFile.md)
- [Principales commandes](Chapitres/DockerCommand.md)
- [Docker registry](Chapitres/DockerRegistry.md) 
- [Docker forces et faiblesses](Chapitres/DockerForceFaiblesse.md)

## Kubernetes
- [Principaux composants K8S](Chapitres/PrincipauxComposants.md)
- [Différentes Topologies de serveur K8S](Chapitres/TopologieK8S.md)
- [Géneralité](Chapitres/Generalite.md)
- [KubeConfig](Chapitres/KubeConfig.md)
- [Principales commandes](Chapitres/Commandes.md)

## Principaux objects / concepts K8S
### Etape 1 mon premier pod
- [Namespace](Chapitres/Namespace.md)
- [Pod](Chapitres/Pod.md)
- [Label Annotation](Chapitres/LabelAnnotation.md) 
- [ConfigMap](Chapitres/ConfigMap.md) 
- [Secret](Chapitres/Secret.md)

### Etape 2 - les autres moyens d'avoir des pods
- [Tour d'horizon](Chapitres/Workload.md)
- [Deployment](Chapitres/Deployment.md)
- [HorizontalPodAutoscaling](Chapitres/HorizontalPodAutoScaling.md)
- [DaemonSet](Chapitres/Daemonset.md)
- [StatefulSet](Chapitres/StatefulSet.md)

### Etape 3 - L'exposition
- [Service](Chapitres/Service.md)
- [Ingress / ingress Controller](Chapitres/Ingress.md)

### Etape 4 - la persistence
- [Tour d'horizon de la persistance](Chapitres/Persistence.md)
- [PersistentVolume](Chapitres/PersistentVolume.md)
- [StorageClass](Chapitres/StorageClass.md)
- [PersistentVolumeClaim](Chapitres/PersistentVolumeClaim.md)
- 
### Etape 5 - le placement des pods
- [NodeSelector,Resource,Affinity](Chapitres/PodPlacement.md)
- [Taint et Tolération](Chapitres/Taint.md)

### Etape 6 - La sécurité
- [RBAC](Chapitres/RBAC.md)
- [SecurityContext](Chapitres/SecurityContext.md)
- [NetworkPolicy](Chapitres/NetworkPolicy.md)

### Etape 7 - Packaging
- [Helm](Tools/Helm)
- [Kustomize](Tools/Kustomize.md)
- [Opérateur]( Tools/Operateur.md)

## Mise en pratique
- [Lab 1](Exercices/Lab-001.md) (deploiement pod configmap service secret )  
- [Lab 2](Exercices/Lab-002.md) (resource probe)
- [Lab 3](Exercices/Lab-003.md) ()
- [Lab 3](Exercices/Lab-004.md) (RBAC)

## Ecosysteme & outils


- [Metric-server](Tools/MetricServer)
- [Cert-manager](Tools/CertManager.md)
- [Prometheus](Tools/Prometheus.md)
- [Grafana](Tools/Grafana.md)
- [Loki](Tools/Loki.md)
- [Longhorn](Tools/Longhorn.md)
- [Harbor](Tools/Harbor.md)
- [Falco](Tools/Falco.md)
- [Artifactory/Jcr](Tools/Artifactory.md)
- [Rancher](Tools/Rancher.md)
- [Artifacthub.io](https://artifacthub.io)
- [Trivy](Tools/Trivy.md)
### Force / Faiblesse

