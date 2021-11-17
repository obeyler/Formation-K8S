# HorizontalPodAutoscaling
Le principe de l'autoscaling horizontal de pods est d'adapter le nombre d'instances d'un pod en fonction de paramètre tel que la charge CPU ou la charge memoire des pods.

> Pour fonctionner un Horizontal Pod Autoscaler aura besoin qu'un fournisseur de metrique (le plus souvent un "metric server") soit opérationnel sur le cluster. 
> On peut facilement tester sa présence par l'usage d'un `kubectl top pod` ou un `kubectl top node` qui afficherons respectivement la consomation cpu et mémoire des pods ou des nodes.

> Une mauvaise configuration du "metric server" aura aussi comme effet délétère de bloquer la destruction des namespaces en mode `Terminating` (comme tout autre apiservice défaillant d'ailleurs).

## Structure
```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: php-apache
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: php-apache
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

## Commandes utiles
```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10 
```

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Deployment.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Daemonset.html)
