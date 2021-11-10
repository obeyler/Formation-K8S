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
`kubectl create sa monsa -n monspace `

## Role
### Structure
```yaml

```
### Commandes utiles
` `
## RoleBinding
### Structure
```yaml

```
### Commandes utiles
` `
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
