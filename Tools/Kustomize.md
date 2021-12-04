# Kustomize

Kustomize [https://kustomize.io](https://kustomize.io) est un outil qui va vous permettre d'assembler et de contextualiser vos yaml.
Intégré au sein de la cli `kubectl`, il peut être utilisé en utilisant l'option `-k` au lieu de `-f` sur la commande apply.

exemple de fichier `kustomization.yaml`

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - service.yaml
  - deployment.yaml
```

On peut se referer à un autre fichier kustomization 

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
- ../../base

patchesStrategicMerge:
- custom-env.yaml
- replica-and-rollout-strategy.yaml
- database-secret.yaml

secretGenerator:
- literals:
  - db-password=12345
  name: sl-demo-app
  type: Opaque

images:
- name: foo/bar
  newName: foo/bar
  newTag: 3.4.5
```
On aura souvent usage d'une arborescence telle que celle-ci :
```
|__ base
│   |__ deployment.yaml
│   |__ service.yaml
│   |__ ...
│   |__ kustomization.yaml
│
│__ overlay
│   |__ dev
│   |    |__ kustomization.yaml
│   |__ prod
│        |__ kustomization.yaml
```

[Retour](https://obeyler.github.io/Formation-K8S/Tools/Helm.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Operateur.html)
