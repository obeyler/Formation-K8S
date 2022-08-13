# Sécurité

La sécurité est un véritable enjeu dans Kubernetes. 
Un cluster peut aussi bien être attaqué de l'extérieur que de l'intérieur. 

De l'extérieur :

il va falloir que votre cluster soit bien configuré, pour éviter les attaques. 
Il existe des outils tels que [KubeBench](https://github.com/aquasecurity/kube-bench) qui vous aideront à vérifier les bonnes pratiques.

De l'intérieur :

Il faut bien comprendre que l'on va faire tourner dans le cluster des images dockers dont on ne maitrisera pas toujours le contenu.
Il faut donc se prémunir de ce que le container des actions qu'il ne devrait pas faire.

Plusieurs éléments ou outils vont vous permettre de réaliser cela : 
- Les RBAC (Role base access control), 
- Les PodSecurity [via les labels sur les namespace](https://obeyler.github.io/Formation-K8S/Chapitres/LabelAnnotation.md#un-role-lié-à-la-sécurité)
- Les securityContext, 
- Les networkPolicy,
- Les admissionController, 
- Les mécanismes d'audits, 
- Les scanners d'images docker [snyk](https://snyk.io/), [trivy](https://github.com/aquasecurity/trivy), [clair](https://github.com/quay/clair),...
- Des outils comme [appArmor](https://apparmor.net) [secComp](https://www.kernel.org/doc/html/v4.16/userspace-api/seccomp_filter.html#) qui bloqueront certaines actions,
- Des outils comme [falco](http://falco.org/) qui vous aideront à détecter des comportements anormaux.


[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Taint.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/RBAC.html)
