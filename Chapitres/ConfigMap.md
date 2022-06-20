# ConfigMap
## Role
Un ConfigMap est une collection de données qui pourra être utilisée soit comme des variables d'environnements, soit comme des références vers des fichiers que l'on pourra fournir à des Pods.
Il faut garder comme principe en tête que l'on ne doit pas mettre de données sensibles dans un configmap (un secret est plus approprié pour cela).
Un configmap est lu au démarrage du pod. Une fois celui-ci démarré, un changement dans le configmap ne sera pas forcément répercuté sur le pod : cela dépendra de la configuration de kubelet.

Les données dans un configmap sont en clair (ie non encodées en base64) contrairement au secret.

## Structure
simple:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myconfigmap-env
data:
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"
```
avec des fichiers complets 
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myconfigmap-file
data:
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=13
  exemple.yaml: |
    test:
      parametre: 34
    valeur: 45
```
## Les usages d'un configmap au sein d'un pod
On peut s'en servir pour affecter une variable d'environnement.
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: test
spec:
    containers:
        - name: test-container
          image: k8s.gcr.io/busybox
          command: [ "/bin/sh", "-c", "env" ]
          env:
              - name: SPECIAL_LEVEL_KEY
                valueFrom:
                    configMapKeyRef:
                        name: myconfigmap-env
                        key: player_initial_lives
                        
    restartPolicy: Never
``` 
On peut s'en servir pour affecter un ensemble de variables d'environnements
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: test-volume
spec:
    containers:
        - name: test-container
          image: k8s.gcr.io/busybox
          command: [ "/bin/sh", "-c", "env" ]
          envFrom:
              - configMapRef:
                  name: myconfigmap-env
                        
    restartPolicy: Never
```
On peut s'en servir pour monter un repertoire avec certains fichiers de la configMap dans un pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-volume-file
spec:
  containers:
    - name: test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh","-c","cat /etc/config/exemple.yaml" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: myconfigmap-file
        items:
        - key: exemple.yaml
          path: exemple.yaml
        - key: exemple2.yaml
          path: exemple2.yaml        
  restartPolicy: Never
```

> Attention : on a toujours dans ce cas un montage d'un repertoire complet (qui ne contient qu'un fichier) 

On peut s'en servir pour monter un fichier unique de la configMap dans un pod 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-volume-file
spec:
  containers:
    - name: test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh","-c","cat /etc/config/exemple.yaml" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config/example.yaml     #<==== Un seul fichier sera posé dans le repertoire
        subPath: example.yaml                   #<==== Un seul fichier contenu dans la CM 
  volumes:
    - name: config-volume
      configMap:
        name: myconfigmap-file
  
  restartPolicy: Never
```
 
On peut s'en servir pour monter des fichiers dans un pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod

spec:
    containers:
        - name: test-container
          image: k8s.gcr.io/busybox
          command: [ "/bin/sh", "-c", "ls /etc/foo" ]
          volumeMounts:
            - name: foo
              mountPath: "/etc/foo"
              readOnly: true
  volumes:
  - name: foo
    configMap:
      name: myconfigmap-file
```

## Commandes utiles
Création d'un configmap à partir d'une série de clef/valeur:
```
kubectl create cm -n NameSpace MyCM --from-literal=test=valeur --from-literal=test2=valeur2
```

Création d'un configmap à partir de fichier:
```
kubectl create cm -n NameSpace MyCM --from-file=toto.txt
```
## Exercices
- créer un configmap nommé "maconfigmap" à partir avec une propriété "couleur=rouge" ainsi que des fichiers suivant

monapplication.properties
```properties
test = "345"
ville = "toulouse"
```

fichier.yaml
```yaml
test: "345"
legume:
  couleur: "orange"
  nom: "carotte"
ville: "toulouse"
```

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/LabelAnnotation.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Secret.html)
