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

[Retour](https://obeyler.github.io/Formation-K8S/) [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Workload.html)
