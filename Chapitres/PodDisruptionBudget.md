# PodDisruptionBudget
## Role
Pour assurer un maximum de sécurité, on peut utiliser des PodDisruptionBudget.
On va ainsi informer Kubernetes qu'il n'a pas le droit de supprimer tous les Pods d'un ensemble (Deployment, StatefulSet, Replicaset).
Deux paramètres sont disponibles :
- minAvailable 

Donne en valeur absolue ou en pourcentage le nombre minimal d'instance de pod à maintenir en état Ready.

- maxUnavailable

Donne en valeur absolue ou en pourcentage le nombre maximal d'instance de pod que l'on accepte d'avoir non Ready.

## Structure
```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: taz
spec:
  minAvailable: 2
  maxUnavailable: 1
  selector:
    matchLabels:
      app: taz
```

## Attention
Un usage non maitrisé d'un PodDisruptionBudget peut avoir des conséquences fâcheuses.

Attention à ne pas mettre le minAvailable égal au nombre de replica si stratégie d'update ne permet pas d'avoir plus de pod que demandé.

Exemple: 

Sur un déploiement avec un Replica égal à 3, vous avez un PodDisruptionBudget paramétré avec minAvailable = 2 ou un maxUnavailable = 1.
Un des pods tombe en erreur, vous ne pourrez pas updater votre cluster automatiquement sans avoir retiré le PodDisruptionBudget (ou avoir corrigé l'erreur).
En effet, Kubernetes qui va vouloir relancer ses Nodes une par une. Il va chercher à déplacer vos pods (drain node).
Il n'aura de base pas le droit de le faire : il violerait le contrat posé par le PodDisruptionBudget.

