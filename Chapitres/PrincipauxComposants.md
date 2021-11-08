# Principaux composants K8S
Kubernetes un légo pour développeur ? Oui, on peut le penser.

![schema](https://obeyler.github.io/Formation-K8S/images/architecture-K8S.drawio.svg)

Tous les composants se parlent entre eux en utilisant des certificats pour chiffrer leur conversation et donc sont donc identifiés mutuellement. 
Cela permet également de limiter au strict nécessaire les actions que peuvent faire un composant sur le système. Nous verrons cela plus en détail quand on regardera l'aspect RBAC.
On a aussi d'autres composants qui même s'ils ne font pas partie du cœur de Kubernetes sont essentiels à son fonctionnement.
Par exemple de nombreuses implémentations existent pour la couche réseau ou la couche de stockage chacun ayant ses forces, ses faiblesses. Impossible d'être exhaustifs tant l'univers Kubernetes évolue vite.
Rien que pour la couche réseau, on peut citer par exemple
- [Flannel](https://github.com/flannel-io/flannel) (simple mais efficace :-) )
- [Calico](https://www.tigera.io/project-calico/) (implémente les Network policy, ...)
- [Canal](https://docs.projectcalico.org/getting-started/kubernetes/flannel/flannel) (Flannel+Calico)
- [Cilium](https://cilium.io) (implémente les Network policy, l'encryption, n'utilise plus kubeproxy, ...)
- [Weave net](https://github.com/weaveworks/weave) (implémente les Network policy, ... )

Coté couche de stockage la liste est tellement longue et évolue vite que je vous invite à la consulter sur le site de kubernetes.

[Retour](https://obeyler.github.io/Formation-K8S/)
