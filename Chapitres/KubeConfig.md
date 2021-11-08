# KubeConfig
## Role 
Le role d'un fichier `.kube/config` est de définir les crédentials pour se connecter à un cluster K8S.
Il va définir :
- un ensemble de cluster (url/data),
- un ensemble d'utilisateur avec leur crédential 
- un ensemble de contexts.
Un context représente couple composé d'un cluster et d'un utilisateur.
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
Ce qu'il faut retenir c'est que :
- ce fichier peut vous permettre de référencer plusieurs clusters,
- ce fichier peut vous permettre de référencer plusieurs utilisateurs qui auront chacun des droits differents
- ce fichier est utilisé par de nombreux autres outils pour accéder à votre cluster (Helm, ArgoCd, ...)
- on peut décomposer ce fichier en n fichiers 
- peut vous permettre de changer le nom du namespace par défaut 

## Commandes utiles

Pour passer sur un context `NOM_DU_CONTEXT`:
```
kubectl config use-context NOM_DU_CONTEXT
```
Pour lister les contexts présents :
```
kubectl config get-contexts
```
(une '*' vous pointera le context actuellement utilisé)

Pour utiliser un context `NOM_DU_CONTEXT` seulement pour une commande Kubectl sans changer le context courant :
```
kubectl VOTRE_COMMANDE --context NOM_DU_CONTEXT
```

Pour utiliser un fichier kubeconfig différent de '.kube/config` seulement pour une commande Kubectl sans changer le context courant :
```
kubectl VOTRE_COMMANDE  --kubeconfig .kube/config-server-1
```

Pour changer sur le context courant le nom du namespace par défaut pour `MYNAMESPACE` :
```
kubectl config set-context --current --namespace=MYNAMESPACE
```


[Retour](https://obeyler.github.io/Formation-K8S/)
