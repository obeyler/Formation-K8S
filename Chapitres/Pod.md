# Pod
## Role
C'est l'unité de base active dans kubernetes, il peut être instancié seul, au sein d'un déploiement ou d'un replicaset ou d'un statefulset ou d'un daemonset.
Il peut même être `static` quand il est directement instancié par le kubelet via un manifest dans /var/lib/kubelet/manifest.
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


## Les probes Startup/Liveness/Readyness
Le pod offre la possibilité de détecter s'il est opérationnel ou pas par le biais de probe (sonde).
Si un conteneur ne fournit pas de probe, Kubernetes considère que l'état par défaut est Success.
![schema](https://obeyler.github.io/Formation-K8S/images/probe.drawio.svg)

### startupProbe:
Indique si l'application à l'intérieur du conteneur a démarré.
Toutes les autres probes sont désactivées si une startup probe est fournie, jusqu'à ce qu'elle réponde avec succès. Si la startup probe échoue, le kubelet tue le conteneur, et le conteneur est assujetti à sa politique de redémarrage.

### livenessProbe
Indique si le Conteneur est en cours d'exécution. 
Si la liveness probe échoue, kubelet tue le Conteneur et le Conteneur est soumis à sa politique de redémarrage (restart policy). 

### readinessProbe 
Indique si le Conteneur est prêt à servir des requêtes. 
Si la readiness probe échoue, le contrôleur de points de terminaison (Endpoints) retire l'adresse IP du Pod des points de terminaison de tous les Services correspondant au Pod. 
L'état par défaut avant le délai initial est Failure.
Une fois passé en mode not ready, Kubernetes attendra que la sonde retourne N fois en positif avant de réintégrer le pod dans le service.
N devant être supérieur ou égal à la valeur de `successThreshold` (qui par défaut est égale à 1)

Quelles actions peuvent être faites par une sonde ?
Plusieurs actions existent :
- ExecAction: Exécute la commande spécifiée à l'intérieur du Conteneur. Le diagnostic est considéré réussi si la commande se termine avec un code de retour de 0.
exemple :
```yaml
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```

- TCPSocketAction: Exécute un contrôle TCP sur l'adresse IP du Conteneur et sur un port spécifié. Le diagnostic est considéré réussi si le port est ouvert.
exemple :
```yaml
    readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
```

- HTTPGetAction: Exécute une requête HTTP Get sur l'adresse IP du Conteneur et sur un port et un chemin spécifiés. Le diagnostic est considéré réussi si la réponse a un code de retour supérieur ou égal à 200 et inférieur à 400.
exemple :
```yaml
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
```



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
