# Metric Server
Metrics Server collecte les métriques de ressources à partir des Kubelets et les expose dans l'apiserver Kubernetes par le biais de l'API Metrics pour une utilisation par l'Autoscaler Horizontal Pod et l'Autoscaler Vertical Pod. L'API Metrics est également accessible par kubectl top, ce qui facilite le débogage des pipelines d'autoscaling.

Metrics Server n'est pas destiné à des fins autres que l'autoscaling. Par exemple, ne l'utilisez pas pour transmettre des métriques aux solutions de surveillance, ou comme source de métriques de solutions de surveillance. Dans de tels cas, veuillez collecter les métriques directement à partir du point de terminaison Kubelet /metrics/resource.

Les offres de Metrics Server :

Un déploiement unique qui fonctionne sur la plupart des clusters (voir les exigences).
Une mise à l'échelle automatique rapide, collectant les métriques toutes les 15 secondes.
Efficacité des ressources, en utilisant 1 milli core de CPU et 2 MB de mémoire pour chaque nœud d'un cluster.
Un support évolutif jusqu'à 5 000 nœuds de clusters.

(traduction de https://github.com/kubernetes-sigs/metrics-server)

## Commandes utiles
```shell
kubectl top node
```

```shell
kubectl top pod
``` 
