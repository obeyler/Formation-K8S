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
# Différences par rapport à un pod
Contrairement à un pod créé sans déploiement, si on demande à kubernetes de tuer un pod, comme le deploiement veut respecter sa specification, il va automatiquement demander une recréation d'un nouveau pod.   
## Commandes utiles
pour créer rapidement un deploiement via une commande
```
kubectl create deploy nginx --image nginx
deployment.apps/nginx created
```


## FAQ



[Retour](https://obeyler.github.io/Formation-K8S/)
