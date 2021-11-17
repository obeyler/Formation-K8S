# NetworkPolicy
## Role
Si les RBAC vous assure un contrôle de qui peut voir ou agir sur les objects Kubernetes, il reste que tous les pods peuvent de base se parler.
Rien n'empêcherait un container malicieux d'appeler un service n'appartenant pas à son namespace. 
C'est le rôle des `NetworkPolicy` de définir ce qui peut `entrer` ou `sortir` d'un pod.

> Attention : Les NetworkPolicy ne sont pas forcément supportés par tous les CNI fonctionnant sur Kubernetes.

## Structure
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```


[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/SecurityContext.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Maj.html)
