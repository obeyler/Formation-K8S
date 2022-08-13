# Label

## Un role de lien entre ressource ou selector 
Les labels sont très utilisés dans kubernetes. Ils permettent de sélectionner des resources. 
Ils servent également à les associer par exemple à un service et à ses pods.
Une `resource` kubernetes peut avoir 0,1 ou plusieurs labels. Certains labels sont réservés par kubernetes.
Tous les `resources` kubernetes peuvent avoir des labels/ 
Les labels sont placés dans la section `metadata` :
```yaml
...
metadata:
  name: <NOM-DE-LA-RESOURCE>
  labels:
    toto: Carabo
    app: Velo
    version: 1
spec:
...
```
## Un role lié à la sécurité
Depuis la version 1.23 de K8S un nouveau usage, lié à la sécurité, est apparu sur les namespace :
```
pod-security.kubernetes.io/<MODE>: <NIVEAU>
pod-security.kubernetes.io/<MODE>-version: <VERSION>
```

exemple: 
```yaml
...
apiVersion: v1
kind: Namespace
metadata:
  name: mon_namespace
  labels:
    pod-security.kubernetes.io/enforce: baseline
    pod-security.kubernetes.io/enforce-version: v1.24
    pod-security.kubernetes.io/audit: restricted
    pod-security.kubernetes.io/audit-version: v1.24
    pod-security.kubernetes.io/warn: restricted
    pod-security.kubernetes.io/warn-version: v1.24
...
```


Pour éviter d'être surpris par des changement de contrainte sur une monté de version on peut fixer la version qui sera utilisée.
On peut utiliser differents modes préconfigurés :
- enforce : Tous les pods qui violent la politique seront rejetés
- audit : Les violations seront enregistrées comme une annotation dans les journaux d'audit, mais n'affecteront pas l'autorisation du pod.
- warn : Les violations enverront un message d'avertissement à l'utilisateur, mais n'affecteront pas l'autorisation du pod.
  
On peut utiliser differents niveaux préconfigurés :

- privileged: ouvert et sans restriction
- baseline: Couvre les escalades de privilèges connues tout en minimisant les restrictions
- restricted: Fortement restreint, renforcement contre les escalades de privilèges connues et inconnues et peut causer des problèmes de compatibilité.


## Exemples de labels réservés
- kubernetes.io/hostname
- kubernetes.io/os
- storageclass.kubernetes.io/is-default-class



Pour aller plus loin : [https://kubernetes.io/docs/reference/labels-annotations-taints/](https://kubernetes.io/docs/reference/labels-annotations-taints/)

# Annotation
## Role
Contrairement à un Label, on ne peut pas faire de recherche selective sur une annotation. 
L'annotation va servir 
- à donner des indications pour d'autre processus. 
- à garder une trace de l'historique des changements sur l'object

Tout comme les labels, elles sont placées dans la section `metadata` :
```yaml
...
metadata:
  name: <NOM-DE-LA-RESOURCE>
  annotations:  
    backup.velero.io/backup-volumes: <NOM-DU-VOLUME>
spec:
...
```

## Exemples
- L'application 'Velero' va savoir qu'il faut sauvegarder les volumes d'un pod, par l'annotation `backup.velero.io/backup-volumes`
- L'application 'Cert-manager' va savoir qu'il faut utiliser une certaine methode pour générer un certificat puis secret sur Ingress `cert-manager.io/cluster-issuer: "hello-deployment-tls`.
- L'application 'Ingress-Nginx' va utiliser les annotations posées sur l'ingress pour paramétrer son nginx 

> Une annotation ne porte pas d'action par elle-même, si aucun pod ne s'y intéresse elle ne fait rien !
> Il faut connaitre l'application cible pour savoir l'effet que peut avoir ou pas une annotation. 

## Commandes utiles
- pour ajouter un label à un pod 
```kubectl label pods my-pod new-label=awesome```
- pour enlever un label
```kubectl label pods my-pod new-label-```
- pour voir les labels
```kubectl get pods --show-labels```
- pour sélectionner des pods par leur label
```kubectl get pods -l run ```
on peut préciser la valeur que doit prendre le label
  ```kubectl get pods -l run=timer ```
- pour annoter un pod
```kubectl annotate pods my-pod icon-url=http://goo.gl/XXBTWq```

# Exercices:
- rechercher tous les pods qui ont le label "run"
- rechercher tous les pods qui ont le label "run=mytimer"
- annoter tous les pods de votre namespace qui ont le label "run=mytimer" avec l'annotation 'myname=moi'

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Pod.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/ConfigMap.html)
