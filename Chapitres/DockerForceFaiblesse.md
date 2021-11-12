# Docker Forces et Faiblesses
Comme la formation n'est pas dédiée à Docker (ni au machine virtuelle), mais à Kubernetes.
Voyons rapidement ce qui peut vous pousser malgré les forces de docker à utiliser kubernetes. 

## Forces
### La vitesse de lancement  

Par rapport à une machine virtuelle, le lancement d'un container Docker est ultra rapide, là où une machine virtuelle va prendre quelques minutes, il ne faudra que quelques secondes à Docker pour instancier votre container.
Cela est du au fait que Docker va se reposer sur l'hôte pour une bonne partie de son execution.

### La réutilisabilité 
La bibliothèque d'images de container disponible sur Internet (via Dockerhub par exemple) vous trouverez facilement votre bonheur.

### Le DockerFile
Le Docker file vous apporte la connaissance du contenu de votre container et son évolutivité.
En parcourant votre DockerFile, vous savez ce que contient votre container. 
Au passage n'utilisez jamais d'image dont le dockerfile n'est pas connu.
En faisant juste évoluer l'image de base de votre DockerFile, vous pouvez reconstruire votre container en bénéficiant des corrections de la communauté.

### Dev/Test/PreProd/Prod
Comme on livre des images, il y a moins d'écart entre les différents environnements. 

### La composition
En décrivant au sein d'un même fichier une infrastructure complete, un fichier DockerCompose va vous permettre de l'assemblage de containers.   

### Faiblesses
Quand on commence à avoir une grosse application qui nécessite beaucoup de containers, on va se retrouver à l'étroit sur une seule machine. 
On commence alors à utiliser plusieurs machines pour déployer nos containers.
Qui dit répartir les containers sur N machines, dit communication entre machines. 
Pour faire communiquer les containers d'une machine vers ceux d'une autre machine, on va devoir mettre en place du port mapping. C'est-à-dire associer un port d'un container à celui de la machine hôte.
La gestion de ses mapping de ports qui peut paraitre simple au début, va poser des problèmes sur le long terme.
Docker n'offre pas nativement de ServiceDiscovery.
Le placement des containers sur les machines, va se faire à la main, c'est fastidieux. 
Qui mettre ensemble ? Qui séparer ? Est ce la même répartition si on change de type de machine ? 
Quand on décide de réorganiser le placement sur les machines (Il faut refaire les ports mapping !),
ça devient un véritable calvaire. 



[Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/PrincipauxComposants.html)
