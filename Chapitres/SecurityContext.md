# Security Context

## Role
Un securityContext s'applique sur un Pod et/ou sur ses containers.
Un contexte de sécurité définit les paramètres de contrôle des privilèges et des accès pour un pod ou un conteneur. Les paramètres du contexte de sécurité incluent, mais ne sont pas limités à :

Contrôle d'accès discrétionnaire : La permission d'accéder à un objet, comme un fichier, est basée sur l'ID utilisateur (UID) et l'ID groupe (GID).

Security Enhanced Linux (SELinux) : Des étiquettes de sécurité sont attribuées aux objets.

Exécution en tant que privilégiée ou non privilégiée.

Capacités Linux : Donne à un processus certains privilèges, mais pas tous les privilèges de l'utilisateur root.

AppArmor : Utilise des profils de programme pour restreindre les capacités des programmes individuels.

Seccomp : Filtre les appels système d'un processus.

AllowPrivilegeEscalation : Contrôle si un processus peut obtenir plus de privilèges que son processus parent. Ce bool contrôle directement si l'indicateur no_new_privs est activé sur le processus conteneur. AllowPrivilegeEscalation est toujours vrai lorsque le conteneur est : 1) exécuté en tant que Privileged OU 2) possède CAP_SYS_ADMIN.

readOnlyRootFilesystem : Monte le système de fichiers racine du conteneur en lecture seule.


## Structure
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:              <=============== Security Contexte au niveau du Pod
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  volumes:
  - name: sec-ctx-vol
    emptyDir: {}
  containers:
  - name: sec-ctx-demo
    image: busybox
    command: [ "sh", "-c", "sleep 1h" ]
    volumeMounts:
    - name: sec-ctx-vol
      mountPath: /data/demo
    securityContext:
      allowPrivilegeEscalation: false  <======== Security Contexte au niveau du Container
```

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/RBAC.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Chapitres/NetworkPolicy.html)
