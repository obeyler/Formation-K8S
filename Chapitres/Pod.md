# Pod
## Role
Unité de base dans kubernetes, il peut être instancié seul, au sein d'un déploiement ou d'un replicaset ou d'un statefulset ou d'un daemonset.
Il peut même être `static` quand il est directement instancié par le kubelet via un manifest.

## Structure
```yaml

```
## Multi-containers
Un pod peut contenir un ou plusieurs containers. Ils se verront comme des processus sur une même machine et pourront se parler en "localhost" 

## Init-container
On peut mettre un ou plusieurs containers pour effectuer des tâches en pré-lancement du/des containers principaux. Cela peut être utile par exemple pour copier ou mettre des droits sur des fichiers mais aussi pour faire des boucles d'attentes d'un autre composant du systeme. (exemple attendre qu'une BDD soit active)

## Commandes utiles
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

## Les affinity
