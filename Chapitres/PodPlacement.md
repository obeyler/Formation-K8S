# Le placement d'un Pod
Plusieurs éléments vont permettre au scheduler de savoir sur quel Node placer un Pod.

### Les NodeSelector
On peut placer un pod en fonction des labels qui sont posés sur les Nodes. Si un Node n'a pas le label demandé, il ne peut pas accepter le Pod.
```yaml
  nodeSelector:
    disktype: ssd
```
Il est a noté que les Nodes ont, de base, plusieurs labels présents.
```shell
kubectl get node --show-labels

NAME       STATUS   ROLES    AGE    VERSION   LABELS
minikube   Ready    <none>   330d   v1.22.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=minikube,kubernetes.io/os=linux
```

### Les resources
Les resources demandées par le pod vont être utiles au scheduler pour savoir où placer le pod mais pas seulement.
- Si pour un Pod les resources sont non définies (ni request ni limit), il pod est considéré comme non prioritaire.
  Le pod fera partie des premiers à être déplacé au besoin.
- Si le pod a des resources définissent des requests mais pas de limits il fera partie des seconds à être déplacé.
- Si les resources sont définies au niveau request et limit, le Pod est dit prioritaire.
  Il ne sera tué que s'il dépasse ses limites.

```yaml
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

#### Requests
Pour placer les Pods, les resources demandées par un pod ont une importance capitale.
Le Pod signale qu'il a besoin d'une quantité de CPU ou d'une quantité de mémoire.
Si le Node n'a pas les resources demandées, le Pod ne sera pas installé dessus.
Si aucun Node n'a les resources demandées le Pod reste en statut "Pending".
> La somme des Requests des pods présents sur un node ne peut pas dépasser la capacité de celui-ci.

#### Limits
Le Pod peut signaler également qu'il ne doit pas dépasser une quantité de CPU ou d'une quantité de mémoire.
Si le Pod dépense plus il sera tué automatiquement.
Un Node peut faire de la sur-allocation car tous les Pods n'atteignent pas leur limit en même temps.

### Les affinity
### Node Affinity
 Il existe deux types de NodeAffinity : 
 `requiredDuringSchedulingIgnoredDuringExecution` ou `preferredDuringSchedulingIgnoredDuringExecution`
l'un oblige le scheduler, l'autre l'incite au placement. Comme indiqué dans leur nom, ils n'ont aucun effet une fois le pod lancé.


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd            
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
```

### Pod Affinity/AntiAffinity
Là on va jouer sur les attirances/répulsions entre Pod. Il peut être intéressant de placer des Pods ensembles. Par exemple, mettre le backend à coté de sa bdd.
Il peut aussi intéressant d'éviter que des pods soient sur le même Node. Par exemple, si on lance plusieurs Pods pour faire de la HauteDispo, les laisser se mettre sur le même Node serait une erreur.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: redis-cache
spec:
    selector:
        matchLabels:
            app: store
    replicas: 3
    template:
        metadata:
            labels:
                app: store
        spec:
            affinity:
                podAntiAffinity:
                    requiredDuringSchedulingIgnoredDuringExecution:
                        - labelSelector:
                              matchExpressions:
                                  - key: app
                                    operator: In
                                    values:
                                        - store
                          topologyKey: "kubernetes.io/hostname"
            containers:
                - name: redis-server
                  image: redis:3.2-alpine
```
Ici les Pods Redis seront disposés de telle manière qu'ils ne seront jamais 2 sur le même Node en même temps. 
On peut mettre `podAffinity` et `podAntiAffinity`  


[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/PersistentVolumeClaim.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Taint.html)
