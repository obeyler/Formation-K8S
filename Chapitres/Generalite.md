# Généralités 
## Quelques éléments de vocabulaire
Un cluster K8S est constitué de machines physiques et/ou de machines virtuelles que l'on va appeler `Node`.
Les Nodes peuvent être catalogués en deux groupes : les `master node` et les `workers`
Les `master nodes` vont héberger ce qui peut être considéré comme la partie moteur ou système de Kubernetes.
Les `workers` vont héberger les workflows applicatifs.

## Le point de vue REST
D'un certain point de vue, Kubernetes est comme une BDD accessible en REST. 
On peut d'ailleurs discuter avec l'APIServer avec des commandes CURL avec les verbes http usuels `GET`, `PUT`, `DELETE`,...

## Le point de vue Convergence vers la cible.
Kubernetes peut être également vu comme un système qui va chercher à atteindre/converger vers la cible que vous lui fixerez.
Lorsque vous lui demandez d'effectuer une action qu'il comprend (ex : lancement d'un pod), s'il ne peut la faire directement, il essayera régulièrement de répondre à votre demande.

>Note: Une action qu'il ne comprend pas sera tout simplement rejetée et oubliée. 

La cible de ce que vous demandez à un cluster Kubernetes est décrite dans des `objects`. Ces objets sont décrits en texte au format yaml ou en json.

## La structure des objets K8S
Tous les objets kubernetes ont un `kind` qui va donner le type d'objet.
Comme Kubernetes évolue, les spécifications des `objects` évoluent aussi d'où la présence d'une `apiVersion`. Cette `apiVersion` donne l'indication de la maturité de l'objet ainsi que la spécification de l'objet à utiliser.   
Ils ont toujours une section `metadata` qui permet de définir leur nom, leur namespace (s'ils appartiennent à un namespace), et d'autres informations comme les labels, et annotations 
On comprend mieux alors la structure globale des objects K8S. 
```yaml
apiVersion: versionDeLAPI
kind: TypeDObject
metadata:
  name: "nom"                                    #<= le nom de l'object
  namespace: "monnamespace"                      #<= si l'objet appartient à un namespace
  labels: []
  annotations: []
spec:
 ...
status:
 ...
```

> Kubernetes offre également l'opportunité d'étendre ses propres concepts et d'utiliser sa capacité à faire du CRUD pour de nouveaux `objects` (à partir de CRD (CustomDefinitionResource)).

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/TopologieK8S.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/KubeConfig.html)
