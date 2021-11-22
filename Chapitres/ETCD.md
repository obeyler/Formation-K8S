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

## S'assurer que votre ETCD est encrypté
On peut faire en sorte que des resources mis dans l'ETCD par l'api-server soient encryptés.
Pour cela il suffit d'ajouter le flag `--encryption-provider-config <PATH vers votre config>` soit positionné au lancement de l'api-server

Le format du fichier de configuration pour le cryptage, liste tous les encodages possibles pour: 
````yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
    - secrets        # <== liste des resources qu'on veut crypter
    providers:
    - identity: {}   # <== pas de cryptage
    - aesgcm:
        keys:
        - name: key1
          secret: c2VjcmV0IGlzIHNlY3VyZQ==
        - name: key2
          secret: dGhpcyBpcyBwYXNzd29yZA==
    - aescbc:
        keys:
        - name: key1
          secret: c2VjcmV0IGlzIHNlY3VyZQ==
        - name: key2
          secret: dGhpcyBpcyBwYXNzd29yZA==
    - secretbox:
        keys:
        - name: key1
          secret: YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXoxMjM0NTY=
````

L'ordre des providers est important ! 
L'API-server va les utiliser pour crypter/décrypter dans l'ordre, en s'arrêtant sur le premier qui fonctionne.
Ici comme `identity: {}` est en tête de liste les données ne seront pas crypté. 
Il sera par contre capable de décrypter en aesgcm, aescbc, secretbox.


