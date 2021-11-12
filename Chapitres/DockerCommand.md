# Docker Command
Voici une liste non exhaustive des principales commandes utiles sur docker. 

## Voir les containers 
```shell
docker ps (-a)
```
## Voir les images
```shell
docker images (-a)
```

## Construire une image 
```shell
docker build PATH
```

## Tagguer une image 

```shell
docker build -t username/image_name:tag_name .
```
## Re Tagguer une image 
```shell
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

## Lancer un container 
```shell
docker run (-d) (-p hostPort :containerPort ) (--name NAME ) IMGNAME /IMGID
```

## Voir les logs d'un container
```shell
docker logs ID /NAME (-f --tail NBLINE )
```


[Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/DockerRegistry.html)
