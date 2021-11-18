# Docker Registry
## Role
Une registry docker est un lieu où l'on va stocker et récupérer des images Docker.
La registry la plus connue est `dockerhub` (https://hub.docker.com) où l'on trouve la plupart des images des produits connus.
Ce n'est pas la seule ! Une autre registry très connue et très utilisé quay.io (RedHat). Chaque grand fournisseur service Cloud (AWS/AZURE,GOOGLE,...) en propose, avec plus ou moins de fonctionnalités offertes.
> La registry dockerhub a mis en place un mécanisme de RateLimiting 
> (100 pull/6h en anonyme et 200 pull/6h en utilisateur identifié mais gratuit).
> L'effet produit est que de nombreux projets n'hébergent plus sur dockerhub et qu'il est souvent
> indispensable de mettre en place un registry mirror. 
>
## Registry Privée
Il n'est pas toujours souhaitable d'exposer ses images docker quand elles sont issues de développements privés. On optera alors pour avoir ce qu'on appelle une registry privée.
Pour accéder à cette registry privé, docker aura besoin de credential pour s'y connecter. 
Dans ce cas il conviendra de se logguer:
```
docker login URL-REGISTRY
```
ce qui aura pour effet de completer le fichier `~/.docker/config.json` 

## Registry auto hébergée
Plusieurs projets open-source ([Harbor](../Tools/Harbor.md), [JCR](../Tools/Artifactory.md),...) vous permettent de facilement avoir votre propre registry. 

Les avantages d'une registry auto hébergée sont multiples :
- vous pouvez mieux contrôler ce qui sera sur votre cluster. 
- vous pouvez fonctionner en mode "AirGap" ie sans internet.
- vous affranchir de téléchargements multiples de la même image depuis d'autres sites et préserver votre bande passante
- vous affranchir des rate limiting
- définir des politiques de scan d'images pour être informé de CVE via des outils annexes tel que [Trivy](../Tools/Trivy.md) ou Clair

## Registry mirror
Pour contrer le rate limiting imposé par dockerhub, on peut utiliser des registry mirror 

- Pour Docker :

Pour cela il suffira de les ajouter dans le fichier `/etc/docker/daemon.json`
```json
{
"registry-mirrors": ["https://<my-docker-mirror-host>"]
}
```
- Pour ContainerD :

Pour cela il suffira de les ajouter dans le fichier `/etc/containerd/config.toml` (à noter que ContainerD supporte des mirrors pour des repository autres que docker.io !)

```
[plugins."io.containerd.grpc.v1.cri".registry.mirrors]
  [plugins."io.containerd.grpc.v1.cri".registry.mirrors."docker.io"]
    endpoint = ["https://registry-1.docker.io"]
  [plugins."io.containerd.grpc.v1.cri".registry.mirrors."test.https-registry.io"]
    endpoint = ["https://HostIP1:Port1"]
  [plugins."io.containerd.grpc.v1.cri".registry.mirrors."test.http-registry.io"]
    endpoint = ["http://HostIP2:Port2"]
  # wildcard matching is supported but not required.
  [plugins."io.containerd.grpc.v1.cri".registry.mirrors."*"]
    endpoint = ["https://HostIP3:Port3"]
```

## Fonctionnalités avancées
> Attention à l'usage de certaines fonctionnalités sur les "RegistryDocker" qui peuvent être alléchantes. 

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/DockerCommand.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/DockerForceFaiblesse.html) 
