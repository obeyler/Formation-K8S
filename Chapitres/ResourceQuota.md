# ResourceQuota & LimitRange

# Role
Le but est de définir combien de resources ont le droit d'être créé dans un namespace.
Cela s'applique aux ressources suivantes :
- `pods`	Pod
- `services`	Service
- `replicationcontrollers`	ReplicationController
- `resourcequotas`	ResourceQuota
- `secrets`	Secret
- `configmaps`	ConfigMap
- `persistentvolumeclaims`	PersistentVolumeClaim
- `services.nodeports`	Service of type NodePort
- `services.loadbalancers`	Service of type LoadBalancer

mais aussi aux resources que peut demander l'ensemble des Pods d'un namespace 
- `requests.cpu` 
- `requests.memory` 
- `limits.cpu`
- `limits.memory`
- 
Dans ce cas de resourceQuota, les Pods seront obligé de donner leur request et/ou leur limit.

La création d'une resource échouera si vous dépassez le quota attribué dans le namespace. 
Attention cela compte aussi pour les resources qui sont créés par Kubernetes.

Exemple: 

Lors de la création d'un nouveau serviceAccount, un secret va être créé pour y mettre son token.
Si le quota n'est pas suffisant, le secret ne sera pas créé. 

# Structure
````yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-quota-demo
spec:
  hard:
    persistentvolumeclaims: "1"
    services.loadbalancers: "2"
    services.nodeports: "0"
````

# LimitRange
Pour aider, il existe un moyen de fixer des valeurs par défaut pour éviter d'avoir à les mettre sur chaque container.

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1        #<=== valeur limite par défaut 
    defaultRequest:   
      cpu: 0.5      #<=== valeur demandé par défaut
    max:
      cpu: 2        #<=== valeur Maximum autorisée
    min :
      cpu: 0.1      #<=== valeur Minimum autorisée 
    type: Container
```

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/PodDisruptionBudget.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/ETCD.html)_
