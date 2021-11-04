# Topologie
En fonction de l'usage que l'on va avoir du cluster, une topologie différente sera à choisir

## Minimaliste
All in one (Minikub) Data plane et Control plane sont sur une même machine. 
petite information à savoir sur un minikube, le stockage sera fait sur la machine hôte. 
## Classique 
ETCD&Control Plane + n Worker, 
on trouvera souvent ce type de cluster avec 3 noeuds master qui portent à la fois ETCD et la partie Control plane et des noeuds workers qui portent les pods applicatifs 
## Ha/Prod 
On aura un cluster ETCD de n machines + un Control Plane de 3 machines + n Workers
Une approche dédiée à la production sera de déporté, hors du control plane, un cluster ETCD composé de 3 nœuds ou plus.

Note: Un nombre impair de nœud pour le cluster ETCD est toujours nécessaire. Pour être fonctionnel un cluster ETCD à besoin que la majorité de ses noeuds soient d'accord. 
- ETCD avec 1 nœud = pas de tolérance au panne (la majorité correspond à 1/1 nœud)
- ETCD avec 3 nœuds = tolérance d'une panne (la majorité correspond à 2/3 nœuds )
- ETCD avec 5 nœuds = tolérance de deux pannes  (la majorité correspond à 3/5 nœuds)