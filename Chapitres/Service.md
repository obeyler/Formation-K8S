# Service
## Role
Un pod a une ip non prédictible et il est très volatile.
Il peut être détruit à tout moment pour être reconstruit ailleurs.
Il est impossible donc de se fier à son ip pour discuter avec lui, c'est la raison d'être d'un service.
Il joue le role de loadbalancer entre les pods qu'il représente.
## Structure
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```
## le DNS
Au sein d'un cluster Kubernetes les services sont joignables en
- nomduservice # au sein du meme namespace
- nomduservice.nomDeSonNamespace.svc # depuis n'importe quel namespace 

## Les différents types 
### ClusterIP (valeur par défaut quand non spécifiée)
Le service n'est joignable que depuis le cluster et via du port-forwarding, il possédera une IP sur le réseau de service.` 
>Le fait de préciser un NodePort n'est pas compatible avec le type ClusterIP. Il faut donc penser à supprimer le nodeport alloué si vous voulez changer le type du service pour ClusterIP
### NodePort
Le service est joignable sur chaque Node du cluster depuis un port compris entre 30000 et 32767.
Le NodePort peut être fixé lors de la création du service, à défaut Kubernetes en allouera automatiquement.
## Loadbalancer
Le service va monter un NodePort et demander au Cluster d'être mappé sur un LoadBalancer externe fournit par le iaas.
### ExternalName
Le service va router vers un service externe via son FQDN.
## Commandes utiles
```
kubectl expose deployment nginx --port=80 --target-port=8000
```
