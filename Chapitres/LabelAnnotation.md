# Label
## Role
Les labels sont tres utilisés dans kubernetes. Ils permettent de sélectionner des objects. Ils servent également à les associés par exemple un service et ses pods.
Un object kubernetes peut avoir 0,1 ou plusieurs labels. Certains labels sont réservés par kubernetes.
Tous les objects kubernetes peuvent avoir des labels.
## Exemples de labels réservés
- kubernetes.io/hostname
- kubernetes.io/os
- storageclass.kubernetes.io/is-default-class

pour aller plus loin : https://kubernetes.io/docs/reference/labels-annotations-taints/

# Annotation
# Role
Contrairement à un Label, on ne peut pas faire de recherche selective sur une annotation. L'annotation va servir
à donner des indications pour d'autre processus. 
# Exemples
- L'application 'Velero' vont savoir qu'il faut sauvegarder les volumes d'un pod, 
- L'application 'Cert-manager' va savoir qu'il faut utiliser une certaine methode pour générer un certificat puis secret sur Ingress.
- L'application 'Ingress-Nginx' va utiliser les annotations posées sur l'ingress pour paramétrer son nginx 

## Commandes utiles
- pour ajouter un label à un pod 
```kubectl label pods my-pod new-label=awesome```
- pour enlever un label
```kubectl label pods my-pod new-label-```
- pour voir les labels
```kubectl get pods --show-labels```
- pour annoter un pod
```kubectl annotate pods my-pod icon-url=http://goo.gl/XXBTWq```


[Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/ConfigMap.html)
