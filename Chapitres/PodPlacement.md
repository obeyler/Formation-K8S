# Le placement d'un Pod
Plusieurs éléments vont permettre au scheduler de savoir sur quel Node placer un Pod.

### Les NodeSelector
On peut placer un pod en fonction des labels qui sont posés sur les Nodes. Si un Node n'a pas le label demandé, il ne peut pas accepter le Pod.
```yaml
  nodeSelector:
    disktype: ssd
```
Il est a noté que les Nodes ont de base plusieurs labels present

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
### Node Affinity/AntiAffinity


### Pod Affinity/AntiAffinity

[Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/LabelAnnotation.html)
