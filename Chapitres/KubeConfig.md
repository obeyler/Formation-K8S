# KubeConfig
## Role 
Le role d'un fichier `.kube/config` est de définir les crédentials pour se connecter à un cluster K8S. Beaucoup d'autres outils vont s'appuyer sur lui pour vous dialoguer avec vos clusters (Kubectl, Lens, Helm, les CLI Velero,... )
Il va définir :
- un ensemble de cluster (url/data),
- un ensemble d'utilisateur avec leur crédentials
- un ensemble de `contexts`.
Un `context` représente couple composé d'un cluster et d'un utilisateur.

```yaml
apiVersion: v1
kind: Config
preferences: {}

clusters:
- cluster:
  name: development
- cluster:
  name: scratch

users:
- name: developer
- name: experimenter

contexts:
- context:
  name: dev-frontend
- context:
  name: dev-storage
- context:
  name: exp-scratch
```

Ce qu'il faut retenir c'est que ce fichier:
- Peut vous permettre de référencer plusieurs clusters.
- Peut vous permettre de référencer plusieurs utilisateurs qui auront chacun des droits différents
- Est utilisé par de nombreux autres outils pour accéder à votre cluster (Helm, ArgoCd, ...)
- Peut être décomposé `n` fichiers
- Peut vous permettre de changer le nom du namespace par défaut 

### Comment définir des users
Comme on peut s'identifier de différentes manières sur un cluster Kubernetes (par Token, par Certificat, par basicauth), il existera différent moyen de renseigner les crédentials d'un utilisateur.
> C'est l'administrateur par un paramétrage du cluster qui détermine les moyens d'identifications autorisés ou pas sur le cluster. 
> En production, il est recommandé d'utiliser seulement 'Token' ou 'Certificat'.

```shell
kubectl config set-credentials <USER> --client-key=~/.kube/user.key

kubectl config set-credentials <USER> --token=weU9l35qcif

kubectl config set-credentials <USER> --username=admin --password=uXFGweU9l35qcif

kubectl config set-credentials <USER> --client-certificate=~/.kube/admin.crt --embed-certs=true
```

### Comment définir des clusters
Pour rajouter un cluster supplémentaire à votre `.kube/config` :
```shell
kubectl config  set-cluster <NOM-DU-CLUSTER> --server=https://1.2.3.4 --certificate-authority=fake-ca-file
```

## Commandes utiles pour jouer avec un .kube/config

Pour lister les utilisateurs ou les clusters :
```shell
kubectl config get-users
kubectl config get-clusters
```

Pour lister les contexts présents :
```shell
kubectl config get-contexts

CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
          default    default    default    
*         minikube   minikube   minikube   default
```
(une '*' vous pointera le context actuellement utilisé)

Pour créer/modifier un `context` :
```shell
kubectl config set-context NOM_DU_CONTEXT --cluster CLUSTER_NAME --user USER_NAME (--namespace=MON_NAMESPACE)
```
Pour passer sur un `context` `NOM_DU_CONTEXT`:
```shell
kubectl config use-context NOM_DU_CONTEXT
```

Pour utiliser un `context` `NOM_DU_CONTEXT` seulement pour une commande Kubectl sans changer le context courant :
```shell
kubectl VOTRE_COMMANDE --context NOM_DU_CONTEXT
```

Pour utiliser un fichier kubeconfig différent de '.kube/config` seulement pour une commande Kubectl sans changer le context courant :
```
kubectl VOTRE_COMMANDE  --kubeconfig .kube/config-server-1
```

Pour changer sur le `context` courant le nom du namespace par défaut pour `MYNAMESPACE` :
```
kubectl config set-context --current --namespace=MYNAMESPACE
```
## Exercice :
- Afficher les noms des utilisateurs 
- Afficher les noms des clusters 
- Créer un nouveau contexte qui prendrait un namespace par défaut le namespace "TOTO" 
- Afficher les contextes
- Sélectionner ce contexte pour qu'il devienne votre contexte par defaut

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Generalite.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Commandes.html)
