# Mise à jour d'un cluster Kubernetes
Quand on rentre dans le monde Kubernetes, il est fondamental de prendre en compte cet aspect dans votre organisation. 
Les évolutions touchent différents aspects aussi bien objets que cli kubectl. 
Il est impensable en effet de ne pas provisionner du temps pour cela sur votre projet.

## Politique d'upgrade
> Attention on ne doit upgrader que d'une version 1.X.n vers 1.X.m (patch) ou 1.X.n vers 1.X+1.m
Il ne faut pas trop attendre sous peine de devoir en faire série.

exemple:  
- 1.19.7 => 1.19.16 Ok 
- 1.19.7 => 1.20.2 Ok
- 1.19.7 => 1.21.0 **KO** !

## Fréquence
Mais pourquoi upgrader ?

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

### Kubernetes

|Version de Kubernetes |Fin du support|
|:------------ |:----------------:|
|1.19 et versions anterieures |Fin de vie|
|1.20	  |	     2022-02|
|1.21	  |	     2022-06|
|1.22	  |	     2022-10|


### exemple Amazon EKS: 

|Version de Kubernetes|	Version en amont	Amazon EKS |Fin du support|
|:------------ |:---------------:|:----------------:|
|1,17	                |9 décembre 2019	|    2 novembre 2021|
|1.18	                |23 mars 2020	   |18 février 2022|
|1.19	                |26 août 2020	   |   Avril 2022|
|1.20	                |8 décembre 2020|	      Juin 2022|


### exemple AKS (azure)

|Version de K8s|	Sortie en amont|	Préversion d’AKS|	Version GA d’AKS|	Fin de vie|
|:------------ |:---------------:|:----------------:|:---------------:|:----------:|
|1.19*	       |4 août 20        |	Septembre 2020  |	Nov. 2020	      | 1.22 GA|
|1.20	  |8 décembre2020	|Janvier 2021	|Mars 2021	|1.23 GA|
|1.21	|8 avril 2021	|Mai 2021|	Juil. 2021	|1.24 GA|
|1,22	|04 août 21	|Septembre 2021|	Novembre 2021	|1.25 GA|
|1.23	|Décembre 2021|	Janvier 2022|	Février 2022	|1.26 GA|

> :warning: La version 1.19 sera déconseillée et supprimée d’AKS à la fin du mois de janvier 2022.


### exemple  GCP

|Version de Kubernetes	|Date de disponibilité de Kubernetes	|Démarrage de la disponibilité|	Démarrage des mises à niveau|	Fin de vie|
|:------------ |:---------------:|:----------------:|:---------------:|:----------:|
|1.16 et versions antérieures|	Fin de vie|
|1.17|	2019-12-09|	2020-09-15|	2020-08-26|	2022-01|
|1.18|	2020-03-23|	2021-01-15|	2022-02-17|	2022-03|
|1.19|	2020-08-26|	2021-04-18|	2021-08-12|	2022-06|
|1.20|	2020-12-08|	2021-06-15|	2022-10-29|	2022-08|
|1.21|	2021-04-08|	2021-10-01|	2022-01|	2022-10|
|1.22|	2021-08-04|	2021-12|	2022-04|	2023-01|
|1.23|	2021-12-14|	2022-03|	2022-07|	2023-05|


### exemple Rke

Pour Rancher/RKE, il est difficile de savoir... Suze faisant un subtil mélange entre RKE et Rancher.

- Rancher s'installe sur _n_ versions de RKE, 
- Rancher gére **l'usage** de _m_ versions de RKE / _z_ versions de K3S
- RKE pour une version donnée peut utiliser differentes versions de K8S cf https://github.com/rancher/rke/releases/tag/v1.3.2 
 
Actuellement Suze communique sur le cycle de vie de rancher mais pas sur ceux de RKE/RKE2/K3S

il faut consulter le site https://www.suse.com/lifecycle/ 

## CVE
Chaque patch amène son lot de correction de bug, mais aussi quelques fois des correctifs de CVE.
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
Sur une version 1.X.Y, les patchs ne posent pas de problèmes et n'apportent que des correctifs (bug/fix de CVE).
Le changement version X apportera son lot de nouveauté et de changement : changement de politique sécurité, dépréciation de certaines API...
La communauté liste toujours les évolutions (nombreuses) des api, mais il n'est pas rare que les produits que l'on installe sur un Kubernetes n'anticipent pas ses changements.
Des changements liés à des ajouts de sécurités peuvent empêcher le fonctionnement des produits posés sur votre cluster.
On installe souvent des produits via des helm charts, l'upgrade d'un cluster Kubernetes vous imposera souvent d'upgrader aussi les charts que vous aviez déployés.
> Il faut toujours consulter le ChangeLog de la version 1.X.0 section `Whats News` pour anticiper les évolutions à prévoir !!

#### Exemple d'évolution :

```yaml
apiVersion: extensions/v1beta1  # <========= !!!
kind: Ingress
metadata:
  name: nginxingress
  annotations:
    nginx.ingress.kubernetes.io/auth-type: basic
    nginx.ingress.kubernetes.io/auth-secret: basic-auth
spec:
  rules:
  - host: nginx.mycompany.com
    http:
      paths:
      - path: /
        backend:
          serviceName: nginxservice
          servicePort: 80
```

puis en version Kubernetes 1.18

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: nginxingress
  annotations:
    ingress.kubernetes.io/auth-type: basic
    ingress.kubernetes.io/auth-secret: mypasswd
spec:
  rules:
  - host: nginx.mycompany.com
    http:
      paths:
      - path: /
        backend:
          serviceName: http-svc
          servicePort: 80
```

à partir de Kubernetes 1.19

```yaml
apiVersion: networking.k8s.io/v1  # <========= !!!
kind: Ingress
metadata:
  name: nginxingress
  annotations:
    ingress.kubernetes.io/auth-type: basic      
    ingress.kubernetes.io/auth-secret: mypasswd 
spec:
  rules:
  - host: nginx.mycompany.com
    http:
      paths:
      - path: /
        backend:
          service:             # <========= !!!
            name: nginxservice # <========= !!!
            port:              # <========= !!!
              number: 80       # <========= !!!
```

Lors d'un changement de fournisseur de Kubernetes, il n'est pas rare de devoir revoir ses fichiers yaml car chaque fournisseur va avoir sa politique de sécurité.

#### Attention aux changements même dans l'usage de la kubectl 

**Jusqu’à la version 1.17**

kubectl run toto –i busybox  => Crée et lance un deployment !
(mais aussi un pod / job /cron job cela dépendra des options “restart” et “schedule” )

**Après la version 1.17**

Kubectl run toto –i busybox  => Crée et lance un pod !


[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/NetworkPolicy.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/AdmissionController.html)
