# Mise à jour d'un cluster Kubernetes
Quand on rentre dans le monde Kubernetes il est fondamental de prendre en compte cet aspect dans votre organisation. 
Il est impensable en effet de ne pas provisionner du temps pour cela sur votre projet.

## Politique d'upgrade
> Attention on ne doit upgrader que d'une version 1.X.n vers 1.X.m (patch) ou 1.X.n vers 1.X+1.m

exemple:  
- 1.19.7 => 1.19.16 Ok 
- 1.19.7 => 1.20.2 Ok
- 1.19.7 => 1.21.0 **KO** !

## Fréquence
La communauté Kubernetes sort régulièrement des releases majeurs ou/et mineurs. Elle ne maintient que les 3 dernières branches de release (actuellement 1.22/1.21/1.20).
voir [https://kubernetes.io/releases/](https://kubernetes.io/releases/)

```
Release History 
1.22

Latest Release: 1.22.3 (released: 2021-10-27) 
End of Life: 2022-10-28 
Patch Releases: 1.22.3, 1.22.2, 1.22.1, 
Complete 1.22 Schedule and Changelog
1.21

Latest Release: 1.21.6 (released: 2021-10-27) 
End of Life: 2022-06-28 
Patch Releases: 1.21.6, 1.21.5, 1.21.4, 1.21.3, 1.21.2, 1.21.1, 
Complete 1.21 Schedule and Changelog
1.20

Latest Release: 1.20.12 (released: 2021-10-27) 
End of Life: 2022-02-28 
Patch Releases: 1.20.12, 1.20.11, 1.20.10, 1.20.9, 1.20.8, 1.20.7, 1.20.6, 1.20.5, 1.20.4, 1.20.3, 1.20.2, 1.20.1, 
Complete 1.20 Schedule and Changelog
```

Il faut parfois se rapprocher de votre fournisseur qui aura une vision différente de la fin de support :
Charge à vous de vous assurer qu'il apporte bien les correctifs nécessaires. 

exemple Amazon EKS: 
```
Version de Kubernetes	Version en amont	Amazon EKS Fin du support
1,17	                9 décembre 2019	    2 novembre 2021
1.18	                23 mars 2020	   18 février 2022
1.19	                26 août 2020	      Avril 2022
1.20	                8 décembre 2020	      Juin 2022
```
Pour Rancher/RKE, il faut consulter le site https://www.suse.com/lifecycle/ 

## CVE
Chaque patch amène son lot de correction de bug mais aussi quelques fois des correctifs de CVE.
Avoir un cluster en dehors des 3/4 dernières versions vous laisse à la merci des CVE qui ne seront pas corrigées.

### Exemple :
```
CVE-2021-25741: Symlink Exchange Can Allow Host Filesystem Access

A security issue was discovered in Kubernetes where a user may be able to create a container with subpath volume mounts to access files & directories outside of the volume, including on the host filesystem.

Affected Versions:

kubelet v1.22.0 - v1.22.1
kubelet v1.21.0 - v1.21.4
kubelet v1.20.0 - v1.20.10
kubelet <= v1.19.14
Fixed Versions:

kubelet v1.22.2
kubelet v1.21.5
kubelet v1.20.11
kubelet v1.19.15
```
Les versions 1.18 ne bénéficieront pas de correctifs pour cette CVE.

## Difficultés à anticiper
Sur une version 1.X.Y, les patchs ne posent pas de problèmes et n'apportent que des correctifs (bug/CVE).
Le changement version X apportera son lot de nouveauté et de changement : changement de politique sécurité, dépréciation de certaines API...
La communauté liste toujours les évolutions (nombreuses) des api, mais il n'est pas rare que les produits que l'on installe sur un Kubernetes n'anticipent pas ses changements.
Des changements liés à des ajouts de sécurités peuvent empêcher le fonctionnement des produits posés sur votre cluster.
On installe souvent des produits via des helm charts, l'upgrade d'un cluster Kubernetes vous imposera souvent d'upgrader aussi les charts que vous aviez déployés.
> Il faut toujours consulter le ChangeLog de la version 1.X.0 section `Whats News` pour anticiper les évolutions à prévoir !!


[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Maj.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Retour](https://obeyler.github.io/Formation-K8S/Tools/Helm.html)
