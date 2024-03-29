# Ingress 
## Role
Le but d'un Ingress est de fournir des informations nécessaires à un ingress controller.
L'ingressController est un composant qui va effectuer un routage vers différents services à l'intérieur du cluster en fonction d'informations présentes dans la requête http/https. 
L'Ingress peut également donner l'indication à l'ingress controller quel certificat il doit presenter.
![schema](https://obeyler.github.io/Formation-K8S/images/ingress.svg)

L'ingressController fournira souvent bien plus qu'un routeur simple. Il permet d'exposer en HTTPS, fait du BasicAuth, de l'OIDC, rajoute des informations, etc. 
> Bien étudier chaque IngressController pour connaitre ses fonctions au dela du simple routage

>L'ingressController s'il n'est pas un composant du control plane ni obligatoire sur un cluster kubernetes, il est quand même (le plus souvent) fournit par votre fournisseur de cluster.

## Structure d'un Ingress
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /testpath
        pathType: Prefix
        backend:
          service:
            name: test
            port:
              number: 80
```

Le pathType peut être "Prefix", "Exact", "ImplementationSpecific"

## TLS
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-example-ingress
spec:
  tls:
  - hosts:
      - https-example.foo.com
    secretName: testsecret-tls
  rules:
  - host: https-example.foo.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
```

## Commandes utiles
Création d'un ingress nommé 'simple' qui redirigera les requetes du host foo.com/bar  vers le service svc1:8080 en utilisant un secret TLS "my-cert".
```
kubectl create ingress simple --rule="foo.com/bar=svc1:8080,tls=my-cert"
```

## FAQ
>Peut-on avoir plusieurs IngressController ?

Tout à fait ! Un même cluster peut héberger plusieurs ingress controller, dans ce cas il convient de préciser via une annotation
quel Ingress Class utiliser. 
```yaml
  annotations:
    kubernetes.io/ingress.class: nginx
```


> Quel sont les principaux ingress Controllers du marché

Il en existe beaucoup !
- nginx (https://github.com/nginxinc/kubernetes-ingress),
- traefik (https://doc.traefik.io/traefik/providers/kubernetes-ingress/), 
- istio (https://istio.io/latest/docs/tasks/traffic-management/ingress/kubernetes-ingress/),
- ambassador (https://github.com/datawire/ambassador)
- HaProxy (https://github.com/jcmoraisjr/haproxy-ingress)
- contour (https://github.com/heptio/contour)
- kong (https://github.com/Kong/kubernetes-ingress-controller)

Une liste plus exaustive des fournisseurs avec leurs differentes caractéristiques se trouve ici : https://docs.google.com/spreadsheets/d/191WWNpjJ2za6-nbG4ZoUMXMpUK8KlCIosvQB0f-oq3k/edit#gid=907731238

Attention, les annotations qui pilotent souvent les comportements des IngressController sont là plus part du temps propre à chaque fournisseur, et même au sein d'un même fournisseur, d'une version n à une version n+1 les annotations peuvent changer radicalement !

> Conseil bien choisir son ingress controller pour l'explorer est en connaitre ses subtilités et limites.
Certains permettrons de faire du TCP UDP en plus de l'HTTP/HTTPS. 

## Attention à certains IngressController 
Certains ingress controller utilisent d'autres moyens de se configurer en plus des ingress :

Traefik V2:
```yaml
apiVersion: traefik.containo.us/v1alpha1
kind: Middleware
metadata:
  name: my-auth
spec:
  basicAuth:
    # ...
    headerField: X-WebAuth-User
```

```yaml
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: ingressroute
spec:
# more fields...
  routes:
    middlewares:
      - name: my-auth
```
[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Service.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Persistence.html)

