# StorageClass
## Role
Une StorageClass permet aux administrateurs de décrire les "classes" de stockage qu'ils proposent. 
Différentes classes peuvent correspondre à des niveaux de qualité de service, à des politiques de sauvegarde ou à des politiques arbitraires déterminées par les administrateurs du cluster.

La StorageClass va s'appuyer sur un provisioner qui lui fournira des PersistentVolumes 
voir une liste non exhaustive de provisioner : [https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner)

> On peut définir une StorageClass par défaut dans un cluster (mais qu'une seule à la fois !)

## Structure
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Retain
allowVolumeExpansion: true
mountOptions:
  - debug
volumeBindingMode: Immediate
```
C'est le provisioner qui va comprendre (ou pas) les paramètres.

## ATTENTION !!!
On ne change pas facilement de storageclass une fois en production ! 
C'est un peu comme changer les fondations d'un immeuble : Il y en a qui ont essayé... ils ont eu des problèmes...

> Comme dit en introduction, le stockage est un GROS point dur sur Kubernetes.
Un cluster qui ne fournit pas de stockage n'est pas un cluster sur lequel vous pourrez vous appuyer longtemps.
Bien choisir son fournisseur de stockage est fondamental et prend beaucoup de temps ! 

> Penser à vous appuyer sur votre fournisseur de cluster pour bien comprendre le fonctionnement/stockage qu'il vous propose.
Il faut le sélectionner avec grand soin en fonction de ce qu'il sait faire ou pas et de sa maturité.

Exemple de piège quand on connait mal son stockage CAS (container attach storage):

![schema](https://obeyler.github.io/Formation-K8S/images/storage-ha.svg)


[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/PersistentVolume.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/PersistentVolumeClaim.html)

