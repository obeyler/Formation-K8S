# Docker Registry
## Role
Une registry docker est un lieu où l'on va stocker et récupérer des images Docker.
La registry la plus connue est `dockerhub` (https://hub.docker.com) où l'on trouve la plupart des images des produits connus.
Ce n'est pas la seule ! Chaque grand fournisseur service Cloud (AWS/AZURE,GOOGLE,...) en propose.
## Registry Privée
Il n'est pas toujours souhaitable d'exposer ses images docker quand elles sont issues de développements privés. On optera alors pour avoir ce qu'on appelle une registry privée.
Pour accéder à cette registry privé, docker aura besoin de credential pour s'y connecter. 
## Registry auto hébergée
Plusieurs projets open-source ([Harbor](../Tools/Harbor.md), [JCR](../Tools/Arifactory.md),...) vous permettent de facilement avoir votre propre registry.

Les avantages d'une registry auto hébergée sont multiples :
- vous pouvez mieux controller ce qui sera sur votre cluster. 
- vous pouvez fonctionner en mode "AirGap" ie sans internet.
- vous affranchir des téléchargements multiples vers d'autres sites et préserver votre bande passante
- définir des politiques de scan d'images pour être informé de CVE via des outils annexes tel que [Trivy](../Tools/Trivy.md) ou Clair
