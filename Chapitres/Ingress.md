# Ingress 
## Role
Le but d'un Ingress est de fournir des informations nécessaires à un ingress controller.
L'ingressController va effectuer un routage vers différents services à l'intérieur du cluster en fonction d'informations présentes dans la requête http/https. 
L'Ingress peut également donner l'indication à l'ingress controller quel certificat il doit presenter.
![schema](https://obeyler.github.io/Formation-K8S/images/ingress.svg)


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
>Peut on avoir plusieurs IngressController ?

Tout à fait un même cluster peut héberger plusieurs ingress controller, dans ce cas il convient de préciser via une annotation
quel Ingress Class utiliser
```yaml
  annotations:
    kubernetes.io/ingress.class: nginx
```


> Quel sont les principaux ingress Controllers du marché

- nginx (https://github.com/nginxinc/kubernetes-ingress),
- traefik (https://github.com/Kong/kubernetes-ingress-controller), 
- istio (https://istio.io/latest/docs/tasks/traffic-management/ingress/kubernetes-ingress/),
- ambassador (https://github.com/datawire/ambassador)
- HaProxy (https://github.com/jcmoraisjr/haproxy-ingress)
- contour (https://github.com/heptio/contour)
- kong (https://github.com/Kong/kubernetes-ingress-controller)

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Service.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Persistence.html)

