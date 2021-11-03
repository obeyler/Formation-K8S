# ConfigMap
## Role
Une ConfigMap est une collection de donnée qui pourront être utilisé soit comme des variables d'environnements soit pour fournir des fichiers à un Pod.
Il faut garder comme principe que l'on ne doit pas mettre de donnée sensible dans une configmap (un secret est plus approprié pour cela).
Une configmap est lue au démarrage du pod. Une fois celui-ci démarré, un changement dans le configmap ne sera pas répercuté sur le pod (jusqu'à son prochain redémarrage).
Les données dans une configmap sont en clairs (ie non encodées).
## Structure
```yaml

```
## Les usages d'un configmap au sein d'un pod
### pour affecter une variable d'environnements
### pour affecter un ensemble de variables d'environnements
### pour monter un fichier dans un pod
### pour monter des fichiers dans un pod

## Commandes utiles
Création d'une configmap à partir d'une serie de clef/valeur:
`kubectl create cm -n NameSpace MyCM --from-literal=test=valeur`

Création d'une configmap à partir de fichier:
`kubectl create cm -n NameSpace MyCM --from-file=toto.txt`
