# StatefulSet
## Role

Les StatefulSets sont utiles pour des applications qui nécessitent une ou plusieurs des choses suivantes :
- Des identifiants réseau stables et uniques.
- Un stockage persistant stable.
- Un déploiement et une mise à l'échelle ordonnés et contrôlés.
- Des mises à jour continues (rolling update) ordonnées et automatisées.

> Ci-dessus, stable est synonyme de persistance suite au (re)scheduling de Pods. Si une application ne nécessite aucun identifiants stables ou de déploiement, suppression ou mise à l'échelle stables, vous devriez déployer votre application en utilisant un objet de charge de travail fournissant un ensemble de réplicas sans état (stateless).
Un Deployment ou ReplicaSet peut être mieux adapté pour vos applications sans états.
(dixit https://kubernetes.io/fr/docs/concepts/workloads/controllers/statefulset/)

## Structure
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx # doit correspondre à .spec.template.metadata.labels
  serviceName: "nginx"
  replicas: 3 # est 1 par défaut
  template:
    metadata:
      labels:
        app: nginx # doit correspondre à .spec.selector.matchLabels
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "longhorn"
      resources:
        requests:
          storage: 1Gi
```

## Nommage
Le nom des pods n'est pas choisi au hasard et on peut facilement relier les uns aux autres grâce à leur nom.
Si StatefulSet porte le nom `monstateful`
alors ses pod portent le nom `monstateful-indice` avec indice qui vaut : 0, 1, 2, 3 ...

## Attention
- Les PVC d'un statefulSet ne sont pas supprimés automatiquement lors de la suppression d'un statefulset !
- Les Pods sont créés séquentiellement xx-0 puis xx-1 puis xx-2 ....et détruit dans l'ordre inverse : xx-2, xx-1, xx-0
- Les Pods ne sont pas garantis d'être supprimés lors de la suppression d'un statefulset
- Contrairement à un déploiement chaque pod aura ses propres PVC et donc ses propres PV !

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Daemonset.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/JobCronJob.html)
