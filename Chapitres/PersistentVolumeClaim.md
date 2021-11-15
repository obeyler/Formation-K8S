# PersistentVolumeClaim
## Role
Son rôle est de définir les besoins de volume persistant d'un pod (taille, accessMode,...).


Si une storageclass est mentionnée, K8S peut s'appuyer dessus pour demander la création d'un PV à la taille exacte du PVC.

sinon 
Kubernetes va choisir le PersistentVolume disponible le plus en adéquation avec la demande.
Ce qui veut dire que si seulement des PV de 10Go ou 100Go sont mis à disposition par l'administrateur, 
- un PVC de 3Go se verra affecté un PV de 10Go 
- un PVC de 11Go se verra affecté un PV de 100Go
- un PVC de 200Go ne pourra être satisfait.

## Structure
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  #selector:
  #  matchLabels:
  #    release: "stable"
  #  matchExpressions:
  #    - {key: environment, operator: In, values: [dev]}
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```


L'usage de `selector`  peut permettre de sélectionner plus finement ses PV par des labels posés dessus.

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/StorageClass.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/PodPlacement.html)

