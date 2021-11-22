# Velero et  Restic
Si `ETCD` fournit un moyen de backup/restauration, il peut être délicat à utiliser.
`Velero` est un outil qui vous permet facilement de sauvegarder les resources Kubernetes. 
On peut programmer des sauvegardes régulières vers un espace type S3.
Il s'appuie sur `Restic` pour le backup des volumes.

Une fois mis en place, vous pourrez avoir un moyen restaurer tout ou parti de votre cluster.
