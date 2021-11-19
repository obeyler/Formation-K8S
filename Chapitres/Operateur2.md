# Opérateur

Notion relativement récente et fortement poussée par RedHat, l'opérateur vise à automatiser un ensemble d'opérations.
Là où helm se contente de faire l'installation, la mise à jour, la suppression,
l'opérateur va s'occuper de l'installation, la maintenance, la migration, l'auto réparation, la destruction, etc.
Un opérateur ne va pas forcément s'occuper de toutes ces phases.

L'opérateur va utiliser des objects ? lui, ce qu'on appelle des `Custom Resource`.
La spécification de ces nouveaux objects sera faite dans ce qu'on nomme les `CRD` ou `Custom Resource Definition`.
Une fois ces CRD connues par Kubernetes, on pourra ajouter, supprimer, lister, éditer sur ces objects ou même poser des droits d'usage (RBAC) comme n'importe quel objet Kubernetes.

L'opérateur ne se limite pas à ces objets nouveaux, il va contenir un controller (sous forme de pod) qui va être à l'écoute de leurs changements (apparition/modification/suppression).
Ainsi, quand il voit une modification, il va pouvoir cr�er des objects Kubernetes "standard" (Deployment, serviceAccount, RBAC...), voir des Jobs ou des CronJob qui auront des actions bien précises à réaliser.

Exemple totalement fictif mais, qui aide à comprendre :
Une fois l'opérateur `operateur-beyler-fictif` installé :-)

On peut créer un nouvel objet :
```yaml
apiVersion: beyler.io/V1
kind: baseDeDonn�e
metadata:
  name: MaBDD
spec: 
 taille: 20 Go
 version: 13
 type: Postgresql
```

Le controller va être notifié qu'un objet `baseDeDonnée` a été ajouté.
Il va décider de créer tout ce qu'il faut pour faire une base de donn�e avec les caractéristiques fournies.
?? savoir, une base Posgresql en version 13 de taille 20Go. Elle sera compos�e d'un StatefulSet, d'un serviceAccount, d'un service, etc.

Maintenant imaginons que j'édite mon yaml ainsi :
```yaml
apiVersion: beyler.io/V1
kind: baseDeDonnee
metadata:
  name: MaBDD
spec: 
 taille: 20 Go
 version: 14           <===== Modification
 type: Postgresql
```
Le controller (s'il a été codé pour ! ) va voir qu'une modification a été apporté. Il va instancier une nouvelle base de donnée Postgresl de taille 20Go en version 14 et un Job de migration pour prendre les donn�es de la premiere base et les migrer dans la seconde puis détruire la premiere.

On pourrait même imaginer (en informatique tout est possible si on peut le coder), que la modification porte sur le type de base.
On aurait dans ce cas un Job qui viendrait porter les data d'une base Postgres vers une base Mysql.
Autre modification possible l'ajout d'un paramètre de sauvegarde :

```yaml
apiVersion: beyler.io/V1
kind: baseDeDonnee
metadata:
  name: MaBDD
spec: 
 taille: 20 Go
 version: 14           
 type: Postgresql
 sauvegarde:
   type: s3 
   frequenceParJour: 2
   bucket: monBucket
 ```
Dans ce cas mon controller rajouterait un CronJob, dont le but serait de réaliser les backups !

Il existe de nombreux opérateurs pour Postgresql (Crunchy, Zalando,...), Minio, Kafka, Cassandra, etc.
La maturité et la pérennité sont ? évaluer pour chacun.



[Retour](https://obeyler.github.io/Formation-K8S/Tools/Kustomize.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Retour](https://obeyler.github.io/Formation-K8S/Chapitres/PodDisruptionBudget.html)
