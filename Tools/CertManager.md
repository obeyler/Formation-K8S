# CertManager
## Role
Bien que ne faisant pas partie de Kubernetes, on le retrouve souvent installé sur les clusters. 
Il va servir à construire des certificats, les mettre dans des secrets, et s'assure qu'ils seront renouvelé à temps.
Les secrets portant des certificats sont utilisés par les ingress pour exposer en TLS. 
En annotant un ingress, le certmanager aura toutes les informations nécessaires pour les construire.

## Issuer, ClusterIssuer, Certificate

Les `issuers` de cert-manager définissent comment va être généré les certificats qui en découlent.
On peut les considérer comme des fournisseurs de certificats.
En cela on va définir que les certificats générés seront par exemple :
- auto signé,
- basé sur une CA,
- basé sur des protocoles type ACME, etc.

Il existe deux types d'Issuer :
- les `ClusterIssuer` (accessible depuis n'importe quel namespace)
- les `Issuer` (accessible uniquement depuis leur namespace)

Les `Certificate` de cert-manager donnent 
- la définition du certificat que le fournisseur devra utiliser 
- le nom du secret où sera stocké ce certificat.

## Exemple

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: selfsigned-issuer
spec:
  selfSigned: {}           # <==== ici on signale que ce fournisseur de certificat produira des autosignés 
```

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: monca
spec:
  isCA: true                 
  duration: 87600h0m0s
  renewBefore: 730h0m0s
  commonName: MonCA
  secretName: monca
  issuerRef:
    name: selfsigned-issuer
    kind: ClusterIssuer
    group: cert-manager.io
```
Ce qui donnera un certificat stocké dans un secret `monca`qui joue le role de CA, il est autosigné.

```yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: ca-issuer
spec:                     
ca:                           # <== ici on va s'appuyer sur une CA pour fournir des certificats
    secretName: monca
```

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: moncertificat
spec:
  duration: 8760h0m0s
  renewBefore: 730h0m0s
  secretName: lab2-tls
  issuerRef:
    name: ca-issuer
    kind: Issuer
  commonName: "mon site"

  subject:
    organizations:
      - mon.organisation.com
  dnsNames:
    - "monsite.mon.organisation.com"
```
On aura un certificat qui est signé par notre CA, (elle est elle-même autosignée donc non valide sur internet).

Il nous restera plus qu'à incorporer le secret dans notre ingress (cf ingress)

### Via une annotation dédiée au cert-manager
Il existe une facilité ( une fois qu'on a défini un issuer).
Au lieu de définir l'objet `Certificate`, on peut annoter un Ingress. 
Avec le contenu d'un Ingress, Certmanager à toutes les infos nécessaires  (nom de host, de domaine) pour générer la suite.

```yaml
annotations:
  cert-manager.io/cluster-issuer: selfsigned-cluster-issuer
```

ou

```yaml
annotations:
  cert-manager.io/issuer: ca-issuer
```

Le cert-manager va gérer pour vous la création :
- d'un object Certificate 
- le secret qui le portera
