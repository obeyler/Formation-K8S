# Admission Controller
Un contrôleur d'admission est un morceau de code qui intercepte les demandes adressées au serveur API de Kubernetes avant la persistance de l'objet, mais après l'authentification et l'autorisation de la demande.
Ils ne peuvent être configurés que par l'administrateur du cluster. 
Dans les admissions controllers possibles, il y en a deux spéciaux : MutatingAdmissionWebhook et ValidatingAdmissionWebhook.
Ils exécutent les webhooks de contrôle d'admission mutant et validant (respectivement) qui sont configurés dans l'API.
Les contrôleurs d'admission peuvent être "validants", "mutants" ou les deux. Les contrôleurs mutants peuvent modifier les objets qu'ils admettent ; les contrôleurs validants ne le peuvent pas.
Les contrôleurs d'admission limitent les demandes de création, de suppression, de modification ou de connexion (proxy). 
Ils ne prennent pas en charge les demandes de lecture.
Le processus de contrôle d'admission se déroule en deux phases.
Dans la première phase, les contrôleurs d'admission mutants sont exécutés.
Dans la deuxième phase, les contrôleurs d'admission de validation sont exécutés. N
Certains contrôleurs sont à la fois mutant et validant.

![schema](https://obeyler.github.io/Formation-K8S/images/AdmissionControl.svg)

## Structure
```yaml
apiVersion: admissionregistration.k8s.io/v1beta1
kind: ValidatingWebhookConfiguration
metadata:
  name: <NOM_DE_LA_CONFIGURATION>
webhooks:
- name: <NOM_DU_WEBHOOK>
  rules:
  - apiGroups: 
    - apps
    apiVersions:
    - v1                 #<== liste des versions impactées par ce webhook
    operations:
    - CREATE             #<== liste des verbes HTTP qui déclencheront l'appel vers le Webhook
    resources:
    - deployments        #<== liste des resources qui sont concernées
  clientConfig:
    url: "https://my-webhook.example.com:9443/my-webhook-path"    <== soit une url
    service:                                                      <= soit un service
      namespace: <NAMESPACE_DU_SERVICE>
      name: <NOM_DU-SERVICE>
      path: /<Chemin> 
      port: <Port>
    caBundle: <pem encoded ca cert du serveur webhook>
```

## GateKeeper
Un outil comme [GateKeeper](https://github.com/open-policy-agent/gatekeeper) adaptation dédiée kubernetes d'[OPA](https://www.openpolicyagent.org) utilise ce principe pour permettre de définir ce qui a le droit d'être fait ou pas sur votre cluster.
Par exemple, on peut déclarer des règles qui vont empêcher : 
- les objets Ingress de différents namespace de partager le même nom d'hôte.
- la récupération d'images docker hors de certaines registry.

Il existe des exemples de règles applicables [ici](https://github.com/open-policy-agent/gatekeeper-library).

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Maj.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Tools/Helm.html)
