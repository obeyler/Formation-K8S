# DaemonSet
## Role
Le but d'un daemonSet est d'installer un pod sur chaque node du cluster sans avoir à se préoccuper de la cardinalité contrairement à un statefulSet ou un deployment.
Quand un Node est rajouté au cluster, le daemonSet cherchera à y installer un pod. 
Quand un Node est retiré du cluster, le Pod qui était présent sur ce Node ne va pas être "re schedulé" ailleurs.
L'usage courant d'un daemonSet est d'offrir une fonctionnalité par rapport à chaque node.
Par exemples :
- la récupération et agrégation des logs des containers situés sur l'hôte du node, 
- l'installation d'un outil sur chaque node,
- l'installation d'une couche réseau.

![schema](https://obeyler.github.io/Formation-K8S/images/daemonset.svg)
> Attention : Le daemonSet ne s'affranchit pas des contraintes posées sur le cluster : taint/resources/...
> ie si un pod n'a pas le droit de s'installer sur un Node il n'y sera pas !

## Structure
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-ingress
spec:
  selector:
    matchLabels:
      app: nginx-ingress
  template:
    metadata:
      labels:
        app: nginx-ingress
    spec:
      containers:
      - image: nginx/nginx-ingress:2.0.3
        imagePullPolicy: IfNotPresent
        name: nginx-ingress
        ports:
        - name: http
          containerPort: 80
          hostPort: 80
        periodSeconds: 1
        securityContext:
          allowPrivilegeEscalation: true
          runAsUser: 101 #nginx
          capabilities:
            drop:
            - ALL
            add:
            - NET_BIND_SERVICE
        env:
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        args:
          - -nginx-configmaps=$(POD_NAMESPACE)/nginx-config
          - -default-server-tls-secret=$(POD_NAMESPACE)/default-server-secret
```


## Exercices :
- instancier le daemonset donné dans l'exemple
- trouver pourquoi les pod ne sont pas `Ready`

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/HorizontalPodAutoScaling.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/StatefulSet.html)
