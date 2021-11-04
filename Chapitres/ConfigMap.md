# ConfigMap
## Role
Un ConfigMap est une collection de donnée qui pourront être utilisé soit comme des variables d'environnements soit pour fournir des fichiers à un Pod.
Il faut garder comme principe en tête que l'on ne doit pas mettre de donnée sensible dans une configmap (un secret est plus approprié pour cela).
Un configmap est lu au démarrage du pod. Une fois celui-ci démarré, un changement dans le configmap ne sera pas forcément répercuté sur le pod : cela dependra de la configuration de kubelet.

Les données dans un configmap sont en clairs (ie non encodées en base64).
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
On peut s'en servir pour monter un fichier unique dans un pod
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
Création d'une configmap à partir d'une serie de clef/valeur:
```
kubectl create cm -n NameSpace MyCM --from-literal=test=valeur
```

Création d'une configmap à partir de fichier:
```
kubectl create cm -n NameSpace MyCM --from-file=toto.txt
```
