# Namespace
## Role
On peut voir les namespaces comme un moyen de classer et compartimenter au sein d'un K8S. On va souvent créer un namespace par produit installé.
On peut également appliquer des règles de sécurités au sein d'un namespace et ainsi permettre une isolation entre plusieurs projets.

Il existe des namespaces spécifiques :
- default (celui où vous vos objects kubernetes iront si vous ne spécifiez pas de namespace et si vous ne l'avez pas changé via votre context)
- kube-system (celui utilisé pour les composants system)
- kube-public (ns visible par tous)
> On ne peut pas déplacer un objet kubernetes d'un namespace vers un autre namespace.

## Structure
```yaml
 apiVersion: v1
 kind: Namespace
 metadata:
     name: monnamespace
```

## Commandes utiles
Création d'un namespace
```
kubectl create ns monnamespace
```
Voir le yaml décrivant votre namespace
```
kubectl get ns monnamespace -o yaml
```

Lister les namespaces
```
kubectl get ns
```
Supprimer un namespace
```
kubectl delete ns monnamespace
```
## Namespace bloqué en mode _Terminating_

Il arrive que lors d'une suppression de namespace celui-ci reste bloqué en mode `Terminating` cela peut être dû à plusieurs causes.
- une api de service est cassée (comme toutes les api de service vont être sollicitées lors de la destruction si une dysfonctionne cela bloque la destruction)  
=> vérifier l'état de vos api `kubectl get apiservice`
- un pod reste bloqué en destruction
=> 'kubectl delete pod NOMDUPOD -n NAMESPACE --force`
- une resource custom est présente et n'est pas détruite 
=> trouver la CR et la supprimer
- Seulement en dernier recours supprimer le finalizer du namespace

## Exercice:
- Création d'un namespace "TEST-NOM-PRENOM"
- Création d'un deuxième namespace "NOM-PRENOM"
- Affichage du yaml de votre namespace
- Lister les namespaces
- Modifier votre contexte (cf KubeConfig) pour prendre comme namespace par défaut votre namespace "NOM-PRENOM"

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Commandes.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Pod.html) 
