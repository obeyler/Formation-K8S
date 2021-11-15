# PersistentVolume
## Role
Un PersistentVolume (PV) est un élément de stockage dans le cluster qui a été provisionné par un administrateur ou provisionné dynamiquement à l'aide de Storage Classes. 
Il s'agit d'une ressource dans le cluster, tout comme un nœud est une ressource de cluster. 
Les PV seront monté dans les pod comme des Volumes, mais ont un cycle de vie indépendant de tout pod individuel qui utilise le PV ie la destruction d'un Pod, n'entraine pas la destruction du PV qu'il utilise.

## Structure
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Persistence.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/PeristentVolumeClaim.html)

