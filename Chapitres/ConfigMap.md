# ConfigMap
## Role
Une ConfigMap est une collection de donnée qui pourront être utilisé soit comme des variables d'environnements soit pour fournir des fichiers à un Pod.
Il faut garder comme principe que l'on ne doit pas mettre de donnée sensible dans une configmap (un secret est plus approprié pour cela).
Une configmap est lue au démarrage du pod. Une fois celui-ci démarré, un changement dans le configmap ne sera pas forcément répercuté sur le pod : cela dependra de la configuration de kubelet.

Les données dans une configmap sont en clairs (ie non encodées en base64).
## Structure
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myconfigmap
data:
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5  
```
## Les usages d'un configmap au sein d'un pod
On peut s'en servir pour affecter une variable d'environnement.
```yaml
``` 
On peut s'en servir pour affecter un ensemble de variables d'environnements
```yaml
```
On peut s'en servir pour monter un fichier unique dans un pod
```yaml
```
On peut s'en servir pour monter des fichiers dans un pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: redis
    volumeMounts:
    - name: foo
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: foo
    configMap:
      name: myconfigmap
```

## Commandes utiles
Création d'une configmap à partir d'une serie de clef/valeur:
`kubectl create cm -n NameSpace MyCM --from-literal=test=valeur`

Création d'une configmap à partir de fichier:
`kubectl create cm -n NameSpace MyCM --from-file=toto.txt`
