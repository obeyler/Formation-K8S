# Daemonset
## Role
Le but d'un daemonset est d'installer un pod sur chaque node du cluster sans avoir à se préoccuper de la cardinalité contrairement à un statefulset ou un deploiement.
Quand un Node est rajouté au cluster, le daemonset cherchera à y installer un pod. 
Quand un Node est retiré du cluster, le Pod qui était présent sur ce Node ne va pas être "re schedulé" ailleurs.
L'usage courant d'un daemonset est d'offrir une fonctionnalité par rapport à chaque node.
Par exemples :
- la récupération et agrégation des logs des containers situés sur l'hôte du node, 
- l'installation d'un outil sur chaque node,
- l'installation d'une couche réseau.

> Attention : Le daemonset ne s'affranchi pas des contraintes posées sur le cluster : taint/resources/...
> ie si un pod n'a pas le droit de s'installer sur un Node il n'y sera pas 


[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/HorizontalPodAutoScaling.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/StatefulSet.html)
