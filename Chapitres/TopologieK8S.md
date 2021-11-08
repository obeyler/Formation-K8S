# Topologie
En fonction de l'usage que l'on va avoir du cluster, une topologie différente sera à choisir. Voici les plus standard

## Minimaliste
![schema](https://obeyler.github.io/Formation-K8S/images/topologie-simple-K8S.drawio.svg)

All in one (Minikub) Data plane et Control plane sont sur une même machine. 
petite information à savoir sur un minikube, le stockage sera fait sur la machine hôte. Cette topologie ne doit pas être utilisé en production mais est très utile pour se former et apprendre les bases de fonctionnement d'un cluster.
> à noter pour apprendre
> Il existe d'autres distributions qui permettent d'apprendre Kubernetes à moindre coût. Par exemple : k3s qui peut s'apparenter à un kubernetes "light" (remplacement de l'ETCD par un sqlite)
## Classique 
![schema](https://obeyler.github.io/Formation-K8S/images/topologie-classique-K8S.drawio.svg)

ETCD&Control Plane + n Worker, 
on trouvera souvent ce type de cluster avec 3 noeuds master qui portent à la fois ETCD et la partie Control plane et des noeuds workers qui portent les pods applicatifs 
## Ha/Prod 
![schema](https://obeyler.github.io/Formation-K8S/images/topologie-ha-K8S.drawio.svg)
Une approche dédiée à la production sera de déporté, hors du control plane.
On aura un cluster ETCD de 2xN+1 machines + un Control Plane de 3 machines + M Workers

Note: Un nombre impair de nœud pour le cluster ETCD est toujours nécessaire. Pour être fonctionnel un cluster ETCD a besoin que la majorité de ses nœuds soient d'accord. 
- ETCD avec 1 nœud = pas de tolérance au panne (la majorité correspond à 1/1 nœud)
- ETCD avec 3 nœuds = tolérance d'une panne (la majorité correspond à 2/3 nœuds)
- ETCD avec 5 nœuds = tolérance de deux pannes (la majorité correspond à 3/5 nœuds)
- etc. 

[Retour](https://obeyler.github.io/Formation-K8S/)
