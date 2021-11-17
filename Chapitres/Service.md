# Service
## Role
Un pod a une ip non prédictible et il est très volatile.
Il peut être détruit à tout moment pour être reconstruit ailleurs.
Il est impossible donc de se fier à son ip pour discuter avec lui, c'est la raison d'être d'un service.
Le service joue le rôle de loadbalancer entre les pods qu'il représente.
Il peut exposer à l'extérieur du cluster via un NodePort (sur port identique sur tous les nodes)

![schema](https://obeyler.github.io/Formation-K8S/images/service.svg)

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
      targetPort: 8080
      
```

Le `selector` va permettre de faire le lien avec les pods. 
Tous les pods (du même namespace que le service) et qui porteront les labels présents dans `selector` seront éligibles à être joignables par le service.  
Ici, ce sera tous les pods qui portent le label "app: MyApp".

## le DNS
Au sein d'un cluster Kubernetes les services sont joignables en
- nomduservice # au sein du meme namespace
- nomduservice.nomDeSonNamespace.svc # depuis n'importe quel namespace 
- nomduservice.nomDeSonNamespace.svc.cluster.local # depuis n'importe quel namespace

Le DNS est le plus souvent fournit par le composant CoreDNS.


## Les différents types de service
- ClusterIP (valeur par défaut quand non spécifiée)

Le service n'est joignable que depuis le cluster et via du port-forwarding, il possédera une IP sur le réseau de service.` 
>Le fait de préciser un NodePort n'est pas compatible avec le type ClusterIP. Il faut donc penser à supprimer le nodeport alloué si vous voulez changer le type du service pour ClusterIP

- NodePort

Le service est joignable sur chaque Node du cluster depuis un port compris entre 30000 et 32767.
Le NodePort peut être fixé lors de la création du service, à défaut Kubernetes en allouera automatiquement.

- Loadbalancer

Le service va monter un NodePort et demander au Cluster d'être mappé sur un LoadBalancer externe fournit par le iaas.

- ExternalName

Le service va router vers un service externe via son FQDN.

## Commandes utiles
```shell
kubectl expose deployment nginx --port=80 --target-port=8000
```

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/StatefulSet.html), [Menu](https://obeyler.github.io/Formation-K8S/),[Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Ingress.html)

