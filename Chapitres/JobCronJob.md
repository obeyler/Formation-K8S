# Job 
## Role
Un Job crée un ou plusieurs Pods et continuera à réessayer l'exécution des Pods
jusqu'à ce qu'un nombre spécifié d'entre eux se terminent avec succès.

## Structure
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: lance

spec:
  activeDeadlineSeconds: 3600
  backoffLimit: 2
  completions: 1
  parallelism: 1
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: init
          image: dwdraju/alpine-curl-jq:latest
          imagePullPolicy: Always
          command:
            - "sh"
            - "-c" 
            - "curl http://myservice.namespace.svc"
         
      dnsPolicy: ClusterFirst
```


# CronJob
## Role
Les CronJobs sont destinés à exécuter des actions planifiées régulières telles que des sauvegardes, la génération de rapports, etc. 
Chacune de ces tâches doit être configurée pour se répéter indéfiniment (par exemple : une fois par jour / semaine / mois) ; 
vous pouvez définir le moment dans cet intervalle où la tâche doit commencer.


## Structure
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

Le schedule se programme ainsi :
```
minute (0 - 59)
  │ ┌───────────── Heure (0 - 23)
  │ │ ┌───────────── Jour dans le mois (1 - 31)
  │ │ │ ┌───────────── Mois (1 - 12)
  │ │ │ │ ┌───────────── Jour de la semaine (0 - 6) 
  │ │ │ │ │
  │ │ │ │ │
  * * * * *
```

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/StatefulSet.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/Service.html)
