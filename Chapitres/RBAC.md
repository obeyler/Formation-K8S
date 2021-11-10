# RBAC

![schema](https://obeyler.github.io/Formation-K8S/images/k8s-rbac.svg)

## ServiceAccount
### Structure
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: monsa
  namespace: monnamespace
```
### Commandes utiles
Création d'un service account `monsa`dans un namespace `monnamespace`

```shell
kubectl create sa monsa -n monspace 
```

## Role
### Structure
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    name: monrole
    namespace: monspace
rules:
    - apiGroups:
          - ""
      resources:
          - pods
      verbs:
          - list
          - get
    - apiGroups:
          - rbac.authorization.k8s.io
      resources:
          - roles
      verbs:
          - list
          - get

```
### Commandes utiles
Pour créer rapidement un role en donnant les verbes utilisables et le nom des resources visées :
```shell
kubectl create role monrole -n monspace --verb="list,get" --resource="pod,role"
```
## RoleBinding

### Structure
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: monrolebinding
  namespace: monspace
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: monrole
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: toto
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: testeur
- kind: ServiceAccount
  name: monsa
  namespace: monspace
```
### Commandes utiles
Pour associer un role et un serviceaccount
```shell
kubectl create rolebinding monrolebinding -n monspace --role="monrole" --serviceaccount="monspace:monsa" --serviceaccount="monspace:monsa" --user="toto" --group="testeur"
```
## ClusterRole
### Structure
```yaml

```
### Commandes utiles
` `
## ClusterRoleBinding
### Structure
```yaml

```
### Commandes utiles
` `
[Pour aller plus loin ](../Exercices/Lab-003.md)

[Retour](https://obeyler.github.io/Formation-K8S/)
