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
