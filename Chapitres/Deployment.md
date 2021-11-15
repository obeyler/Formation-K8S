# Deployment
## Role
Son role est de maintenir opérationnel un nombre pod (replica) en respectant une spécification. 
Il permet de mettre à jour les images et la scalabilité du nombre d'instance du pod

## Structure
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      securityContext: {}
```
## Différences par rapport à un pod
Contrairement à un pod créé sans déploiement, si on demande à kubernetes de tuer un pod, comme le deploiement veut respecter sa specification, il va automatiquement demander une recréation d'un nouveau pod.   
Le déploiement va créer un replicaset qui portera les pods. À chaque changement de specification, le déploiement créera un nouveau replicaset.

## Nommage
Le nom des pods et des replicaset n'est pas choisi au hasard et on peut facilement relier les uns aux autres grâce à leur nom.
Si Deployment porte le nom `mondeployment`
alors ses Replicaset porte le nom `mondeployment-HASHREPLICASET`
et les pods d'un replicaset porte le nom `mondeployment-HASHREPLICASET-HASHPOD`


## Commandes utiles
Pour créer rapidement un déploiement via une commande :
```shell
kubectl create deploy nginx --image nginx
deployment.apps/nginx created
```

Pour updater rapidement une image dans un déploiement :
```shell
kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1 --record
```
Pour changer le nombre de pod rapidement dans un déploiement :
```
kubectl scale deployment/nginx-deployment --replicas=10
```

Pour voir l'évolution d'un déploiement :
```shell
kubectl describe deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment
```

Pour revenir en arriere d'un déploiement :
```shell
kubectl rollout undo deployment/nginx-deployment
```

Pour revenir en arriere d'un déploiement vers une révision particuliere :
```shell
kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

## Exercices
- Créer un déploiement à partir de l'image `nginx` avec un replica de 1
- Upgrader via `kubectl edit ...` le nombre de replica à 2
- Changer l'image pour l'image `nginx:stable-alpine` sans passer par `edit` mais par `set image`
- Consulter la description du déploiement
- Consulter l'historique des revisions et faire un rollout vers la premiere revision.
  (noter l'influence de l'option `--record` en faisant les actions avec ou sans cette option)



[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Workload.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/HorizontalPodAutoScaling.html)
