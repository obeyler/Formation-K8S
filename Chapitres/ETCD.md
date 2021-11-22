# ETCD
S'il est souvent plus commode d'utiliser des outils tel que `Velero` pour faire ses backups, il ne faut pas pour autant ignorer que l'on peut faire sans .

## Création d'un snapshot d'une base ETCD
```shell
sudo ETCDCTL_API=3 \
etcdctl  snapshot save snapshot.db \
--endpoints https://<HOST>:2379 \
--cacert /etc/kubernetes/pki/etcd/ca.crt \
--cert  /etc/kubernetes/pki/etcd/server.crt \
--key  /etc/kubernetes/pki/etcd/server.key 
```

## Restoration d'un snapshot d'une base ETCD.

```shell
ETCDCTL_API=3 \
etcdctl snapshot restore snapshotdb \
 --endpoints https://<HOST>:2379 \
 --cacert /etc/kubernetes/pki/etcd/ca.crt \ 
 --cert  /etc/kubernetes/pki/etcd/server.crt \
 --key  /etc/kubernetes/pki/etcd/server.key
```

## Récupération d'un secret directement dans une base ETCD

```shell
ETCDCTL_API=3 \
etcdctl \
--cert /etc/kubernetes/pki/apiserver-etcd-client.crt \
--key /etc/kubernetes/pki/apiserver-etcd-client.key \
--cacert /etc/kubernetes/pki/etcd/ca.crt 
get /registry/secrets/<NAMESPACE>/<SECRET-NAME>
```
