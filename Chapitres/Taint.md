# Taint & Tolération
## Role de la taint
Elles servent à empêcher un pod d'être déployé sur le node si le pod ne tolère pas ses taints.
Les Taints sont posées exclusivement sur les nodes. Elles peuvent être posées/enlevées par l'administrateur ou par les controllers de kubernetes. 
Une Taint va se servir par exemple :  
- pour signifier un hardware spécifique du node (ex architecture ARM,AMD,...),  seule des images compatibles avec ce hardware sont acceptées sur ce node.
- pour dédier des Nodes à certains pods (en évitant ainsi que les autres Pod ne s'y installent)
- pour palier à un problème temporaire, un controller Kubernetes va poser des taints pour signifier que :
    - `node.kubernetes.io/not-ready`  Le Node n'est pas opérationnel.
    - `node.kubernetes.io/unreachable` Le Node n'est pas joignable is unreachable from the node controller. This corresponds to the NodeCondition Ready being "Unknown".
    - `node.kubernetes.io/memory-pressure`: Le Node est n'a plus assez de mémoire disponible.
    - `node.kubernetes.io/disk-pressure` Le Node n'as plus assez d'espace disque disponible.
    - `node.kubernetes.io/pid-pressure` Le Node manque de PID disponible.
    - `node.kubernetes.io/network-unavailable` Le réseau du Node est indisponible.
    - `node.kubernetes.io/unschedulable` Le Node n'accepte pas d'assignation de pod par le scheduler.

Une taint consiste en une clef, une valeur (optionnelle), et un effet ( `NoSchedule`, `PreferNoSchedule` ou `NoExecute`).

Regardons en détail les effets :
- `NoSchedule` Les pods qui ne tolèrent pas cette taint ne seront pas éligibles à l'installation sur ce Node, mais s'ils sont déjà présents ils y restent.
- `PreferNoSchedule` C'est une version best effort de la taint NoSchedule. Les pods qui ne tolèrent pas cette taint ne s'y installeront quand dernier recours s'ils ne trouvent pas un autre Node mieux disposé à les accueillir.
- `NoExecute` Les pod qui ne tolèrent pas cette taint sont expulsés du node 

## Role de la toleration
Elles servent à un pod pour dire qu'il accepte la contrainte posée par une taint. Un Pod peut tolérer une taint seulement un temps limité. 
Cas exemple : il peut tolérer d'être sur un Node Master le temps que son Worker se reconstruise.

> Attention tolérer une taint n'implique pas pour le pod de forcement aller sur le node qui porte cette taint.
## Structure
```yaml


```

[Retour](https://obeyler.github.io/Formation-K8S/)
