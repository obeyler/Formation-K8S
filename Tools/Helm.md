# Helm
Helm est un outil qui va servir à packager des logiciels sur kubernetes. 

## Le Helm Chart
Comme nous l'avons vu lors de cette formation, le nombre de fichiers yaml à produire pour installer un logiciel sur un cluster Kubernetes est conséquent.
Helm va permettre d'installer toutes ses ressources (Deploiement, Job, Role, ServiceAccount, Secret, configMap ...) en quelques lignes.
Il "templatise" ces ressources afin de permettre à chacun de les adapter au contexte à utiliser.
Le Chart est un fichier archive TGZ, qui contient tous les templates des objects K8S nécessaires, un fichier `value.yaml` qui va fournir des valeurs par défaut à utiliser pour peupler les variables des templates.
Chaque variable pouvant être modifié soit par l'usage d'un nouveau fichier yaml, soit par des affectations ponctuelles  ( --set key=valeur ) lors de l'usage de la cli helm.
Un chart peut lui-même s'appuyer sur d'autres chart (dépendance).

## Repository Helm
Un repository de Helm chart n'est en somme qu'un banal serveur web qui possède un fichier index.yaml.
Dans ce fichier, on retrouvera la liste des produits avec leur méta-donnée (numéro de version,auteurs,... et lieu où l'on peut télécharger le chart)

Exemple :

```yaml
apiVersion: v1
entries:
  airflow:
  - annotations:
      category: WorkFlow
    apiVersion: v2
    appVersion: 2.2.1
    created: "2021-10-30T11:01:25.771092252Z"
    dependencies:
    - name: common
      repository: https://charts.bitnami.com/bitnami
      tags:
      - bitnami-common
      version: 1.x.x
    - condition: postgresql.enabled
      name: postgresql
      repository: https://charts.bitnami.com/bitnami
      version: 10.x.x
    - condition: redis.enabled
      name: redis
      repository: https://charts.bitnami.com/bitnami
      version: 15.x.x
    description: Apache Airflow is a platform to programmatically author, schedule
      and monitor workflows.
    digest: eacc86fe88dd9bce16db8c52a8eb77841dd351b17655685899c3ef2f204641b2
    home: https://github.com/bitnami/charts/tree/master/bitnami/airflow
    icon: https://bitnami.com/assets/stacks/airflow/img/airflow-stack-220x234.png
    keywords:
    - apache
    - airflow
    - workflow
    - dag
    maintainers:
    - email: containers@bitnami.com
      name: Bitnami
    name: airflow
    sources:
    - https://github.com/bitnami/bitnami-docker-airflow
    - https://airflow.apache.org/
    urls:
    - https://charts.bitnami.com/bitnami/airflow-11.1.7.tgz
    version: 11.1.7
```
## A savoir

Pour affecter une variable qui servira pour les templates, 
- les `-set key=valeur` sont prioritaires, 
- puis le(s) fichiers `values.yaml` fournit en ligne 
- puis enfin les valeurs présentent dans le fichier `values.yaml` du chart.

## Commandes utiles

