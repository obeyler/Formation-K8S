# Secret
## Roles
Ils ont plusieurs usages :
Comme pour les configmap les secrets sont utilisés par les pods comme variables d'environnement ou comme fichiers.
Ils servent aussi pour les ingress pour fournir les certificats.
Ils servent à donner les crédentials pour être en mesure de récuperer des images sur un registry docker protégé.
S'ils ont leur données encodées (en base 64), il faut bien réaliser que les données d'un secret ne sont pas encryptées.

## Structure
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-sa-sample
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm  
```
## Les différents types possibles
- Opaque (on y mettra ce qu'on veut) 
- kubernetes.io/tls (fournit des fichiers type `tls.cert`, `tls.key`, `ca.cert`)
- kubernetes.io/service-account-token (un jeton de service Kubernetes)
- kubernetes.io/dockerconfigjson (un Docker sérialisé config. json fichier, pour fournir les informations d'identification Docker) 
- kubernetes.io/ssh-auth (fournir les informations d'identification SSH).
- kubernetes.io/basic-auth (fournit des crédentials type basic auth)
## Commandes utiles
décoder un secret
`kubectl get secret secret-sa-sample -o jsonpath="{.data.username}"|base64 --decode`
## Exercices
- créer un secret nommée "monsecret" à partir avec une propriété "couleur=rouge" ainsi que des fichiers suivant
  monapplication.properties
```properties
test = "345"
ville = "toulouse"
```

fichier.yaml
```yaml
test: "345"
legume:
  couleur: "orange"
  nom: "carotte"
ville: "toulouse"
```
- comparer le yaml du secret monsecret avec celui de la configmap maconfigmap 
- créer le secret suivant :
````yaml
apiVersion: v1
kind: Secret
metadata:
  name: monsecret
type: Opaque
data:
  password: YmluZ28gZsOpbGljaXRhdGlvbg==
````
- décoder ce secret en utilisant une ligne de commande

[Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Workload.html)
