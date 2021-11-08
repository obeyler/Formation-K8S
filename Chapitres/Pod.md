# Pod
## Role
C'est l'unité de base active dans kubernetes, il peut être instancié seul, au sein d'un déploiement ou d'un replicaset ou d'un statefulset ou d'un daemonset.
Il peut même être `static` quand il est directement instancié par le kubelet via un manifest.
Hormis le cas ou le pod est static, si vous détruisez un pod il ne reconstruira pas tout seul.
> On ne peut pas éditer un pod, mais il y a une astuce : on récupère son contenu, on détruit le pod, on modifie le contenu et enfin on le reconstruit :-)

## Structure
minimale:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: monPod
  namespace: monNamespace
spec:
  containers:
  - name:  container
    image: monImage:tag
```

## Multi-containers
Un pod peut contenir un ou plusieurs containers. Ils se verront comme des processus sur une même machine et pourront se parler en "localhost" 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: monPod
  namespace: monNamespace
spec:
  containers:
  - name:  container-1
    image: monImage:tag
  - name:  container-2
    image: monImage2:tag
```

## Init-container
On peut mettre un ou plusieurs containers pour effectuer des tâches en pré-lancement du/des containers principaux.
Cela peut être utile par exemple pour copier ou mettre des droits sur des fichiers mais aussi pour faire attendre un autre composant.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: monPod
  namespace: monNamespace
spec:
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup myservice.default.svc.cluster.local; do echo en attente de myservice; sleep 2; done"]  
  containers:
  - name:  container
    image: monImage:tag
```
## Commandes utiles
Pour créer un pod à partir d'une image docker : 
`kubectl run --image nginx nginx`

## Logs
On peut facilement voir les logs d'execution d'un pod.
- cas un seul container dans le pod :
`kubectl logs podName`
- cas un plusieurs containers dans le pod :
`kubectl logs podName -c containerName`


## Les probes Readyness/Liveness 
Le pod offre la possibilité de détecter s'il est opérationnel ou pas par le biais de probe (sonde)

## Les resources

Si pour un Pod les resources sont non définies (ni request ni limit), il pod est considéré comme non prioritaire. 
Le pod fera partie des premiers à être déplacé au besoin.
Si le pod a des resources définissent des requests mais pas de limits il fera partie des seconds à être déplacé.
Si les resources sont définies au niveau request et limit, le Pod est dit prioritaire. 
Il ne sera tué que s'il dépasse ses limites.


### Requests
Pour placer les Pods, les resources demandées par un pod auront une importance capitale.
Le Pod signale qu'il a besoin d'une quantité de CPU ou d'une quantité de mémoire.
Si le Node n'a pas les resources demandées, le Pod ne sera pas installé dessus. 
Si aucun Node n'a les resources demandées le Pod restera en statut "Pending".
La somme des Requests des pods présents sur un node  ne peut pas dépasser la capacité de celui-ci.
### Limits
Le Pod peut signaler également qu'il ne doit pas dépasser une quantité de CPU ou d'une quantité de mémoire.
Si le Pod dépense plus il sera tué automatiquement. 
Un Node peut faire de la sur-allocation car tous les Pods n'atteignent pas leur limit en même temps.

## Les affinity


[Retour](https://obeyler.github.io/Formation-K8S/)
