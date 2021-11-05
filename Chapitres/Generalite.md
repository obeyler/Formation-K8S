# Généralité 
## Le point de vue REST
D'un certain point de vue, Kubernetes est comme une BDD accessible en REST. 
On peut d'ailleurs discuter avec l'APIServer avec des commandes CURL avec les verbes http usuels `GET`, `PUT`, `DELETE`,...

## La structure des objets K8S
Tous les objets kubernetes ont un `kind` qui va donner le type d'objet
Comme Kubernetes évolue les specifications des objects évoluent aussi d'où la présence d'une `apiVersion`elle donne l'indication de la maturité de l'objet ainsi que la spécification de l'objet à utiliser.   
Ils ont toujours une section `metadata` qui permet de définir leur nom, leur namespace (s'ils appartiennent à un namespace), et d'autres informtions comme les labels, et annotations 
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
 ....
```

> Kubernetes offre également l'opportunité d'étendre ses concepts et utiliser sa capacité à faire du CRUD pour de nouveaux objects.
à partir de CRD (CustomDefinitionResource)

## Format générique des Commandes 

Voici les commandes de base que l'on peut avoir via `kubectl` (liste non exhaustive) :
Pour récupérer la liste des objets d'un certain type qui n'appartiennent pas spécifiquement à un namespace ou au namespace `default`:

```shell
kubectl get `TypeDObjet` 
```

Pour récupérer la liste des objets d'un certain type qui appartiennent à un namespace :
```shell
kubectl get `TypeDObjet` -n `monnamespace`
```

Pour récupérer la liste des objets d'un certain type quelque soit leur namespace :
```shell
kubectl get `TypeDObjet` --A
ou 
kubectl get `TypeDObjet` --all-namespaces
```
Pour récupérer un objet d'un certain type qui appartiennent à un namespace :
```shell
kubectl get `TypeDObjet` -n `monnamespace` object
```
Pour récuperer le yaml d'un objet :
```shell
kubectl get `TypeDObjet` -n `monnamespace` object -o yaml
```
Pour éditer le yaml d'un objet :
```shell
kubectl edit `TypeDObjet` -n `monnamespace` object -o yaml
```

Pour détruire un objet d'un certain type qui appartiennent à un namespace :

```shell
kubectl delete `TypeDObjet` -n `monnamespace` object
```
On peut lister les types d'objets supportés par un cluster :
```
kubectl api-resources

NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
endpoints                         ep           v1                                     true         Endpoints
events                            ev           v1                                     true         Event
limitranges                       limits       v1                                     true         LimitRange
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1                                     false        Node
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVolume
pods                              po           v1                                     true         Pod
podtemplates                                   v1                                     true         PodTemplate
replicationcontrollers            rc           v1                                     true         ReplicationController
resourcequotas                    quota        v1                                     true         ResourceQuota
secrets                                        v1                                     true         Secret
serviceaccounts                   sa           v1                                     true         ServiceAccount
services                          svc          v1                                     true         Service
mutatingwebhookconfigurations                  admissionregistration.k8s.io/v1        false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io/v1        false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io/v1                false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io/v1              false        APIService
controllerrevisions                            apps/v1                                true         ControllerRevision
daemonsets                        ds           apps/v1                                true         DaemonSet
deployments                       deploy       apps/v1                                true         Deployment
replicasets                       rs           apps/v1                                true         ReplicaSet
statefulsets                      sts          apps/v1                                true         StatefulSet
tokenreviews                                   authentication.k8s.io/v1               false        TokenReview
localsubjectaccessreviews                      authorization.k8s.io/v1                true         LocalSubjectAccessReview
selfsubjectaccessreviews                       authorization.k8s.io/v1                false        SelfSubjectAccessReview
selfsubjectrulesreviews                        authorization.k8s.io/v1                false        SelfSubjectRulesReview
subjectaccessreviews                           authorization.k8s.io/v1                false        SubjectAccessReview
horizontalpodautoscalers          hpa          autoscaling/v1                         true         HorizontalPodAutoscaler
cronjobs                          cj           batch/v1                               true         CronJob
jobs                                           batch/v1                               true         Job
certificatesigningrequests        csr          certificates.k8s.io/v1                 false        CertificateSigningRequest
leases                                         coordination.k8s.io/v1                 true         Lease
endpointslices                                 discovery.k8s.io/v1                    true         EndpointSlice
events                            ev           events.k8s.io/v1                       true         Event
flowschemas                                    flowcontrol.apiserver.k8s.io/v1beta1   false        FlowSchema
prioritylevelconfigurations                    flowcontrol.apiserver.k8s.io/v1beta1   false        PriorityLevelConfiguration
ingressclasses                                 networking.k8s.io/v1                   false        IngressClass
ingresses                         ing          networking.k8s.io/v1                   true         Ingress
networkpolicies                   netpol       networking.k8s.io/v1                   true         NetworkPolicy
runtimeclasses                                 node.k8s.io/v1                         false        RuntimeClass
poddisruptionbudgets              pdb          policy/v1                              true         PodDisruptionBudget
podsecuritypolicies               psp          policy/v1beta1                         false        PodSecurityPolicy
clusterrolebindings                            rbac.authorization.k8s.io/v1           false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io/v1           false        ClusterRole
rolebindings                                   rbac.authorization.k8s.io/v1           true         RoleBinding
roles                                          rbac.authorization.k8s.io/v1           true         Role
priorityclasses                   pc           scheduling.k8s.io/v1                   false        PriorityClass
csidrivers                                     storage.k8s.io/v1                      false        CSIDriver
csinodes                                       storage.k8s.io/v1                      false        CSINode
csistoragecapacities                           storage.k8s.io/v1beta1                 true         CSIStorageCapacity
storageclasses                    sc           storage.k8s.io/v1                      false        StorageClass
volumeattachments                              storage.k8s.io/v1                      false        VolumeAttachment
```
On peut lister les apiversions supportés par un cluster
```
kubectl api-versions

admissionregistration.k8s.io/v1
apiextensions.k8s.io/v1
apiregistration.k8s.io/v1
apps/v1
authentication.k8s.io/v1
authorization.k8s.io/v1
autoscaling/v1
autoscaling/v2beta1
autoscaling/v2beta2
batch/v1
batch/v1beta1
certificates.k8s.io/v1
coordination.k8s.io/v1
discovery.k8s.io/v1
discovery.k8s.io/v1beta1
events.k8s.io/v1
events.k8s.io/v1beta1
flowcontrol.apiserver.k8s.io/v1beta1
networking.k8s.io/v1
node.k8s.io/v1
node.k8s.io/v1beta1
policy/v1
policy/v1beta1
rbac.authorization.k8s.io/v1
scheduling.k8s.io/v1
storage.k8s.io/v1
storage.k8s.io/v1beta1
v1
```