# Secret
## Roles
Ils ont plusieurs usages :
- Comme pour les configmap les secrets sont utilisés par les pods comme variables d'environnement ou comme fichiers.
- Ils servent aussi pour les ingress pour fournir les certificats.
- Ils servent à donner les crédentials pour être en mesure de récupérer des images sur un registry docker protégé ou acceder à un serveur ssh ou du basic auth.

S'ils ont leurs données encodées (en base 64), il faut bien réaliser que les données d'un secret ne sont pas chiffrées.
Ainsi, il suffit d'utiliser l'utilitaire `base64` pour décoder leur contenu.

## Structure
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-sa-sample
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm  
```

On peut s'affranchir d'encoder les données nous même. Dans ce cas on troquera `data` pour `stringData` et Kubernetes l'encodera en base64 pour vous.
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-sa-sample
type: Opaque
stringData:
  username: admin
  password: 1f2d1e2e67df  
```

## Les différents types possibles
- Opaque (on y mettra ce qu'on veut) 
- kubernetes.io/tls (fournit des fichiers type `tls.cert`, `tls.key`, `ca.cert`)
- kubernetes.io/service-account-token (un jeton de service Kubernetes)
- kubernetes.io/dockerconfigjson (un Docker sérialisé config. json fichier, pour fournir les informations d'identification Docker) 
- kubernetes.io/ssh-auth (fournir les informations d'identification SSH).
- kubernetes.io/basic-auth (fournit des crédentials type basic auth)

## Les usages d'un secret au sein d'un pod
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
                    secretKeyRef:
                        name: secret-env
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
              - secretRef:
                  name: secret-env
                        
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
      command: [ "/bin/sh","-c","cat /etc/secret/exemple.yaml" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/secret
  volumes:
    - name: config-volume
      secret:
        name: secret-file
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
    secret:
      name: secret-file
```


## Commandes utiles
Décoder un secret avec `base64` :
```shell
kubectl get secret secret-sa-sample -o jsonpath="{.data.username}"|base64 --decode
```

## Exercices
- créer un secret nommé "monsecret" à partir avec une propriété "couleur=rouge" ainsi que des fichiers suivant 

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
- comparer le yaml du secret monsecret avec celui de la configmap maconfigmap 
- créer le secret suivant :

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: monsecret
type: Opaque
data:
  password: YmluZ28gZsOpbGljaXRhdGlvbg==
```
- décoder ce secret en utilisant une ligne de commande

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/ConfigMap.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Workload.html)
