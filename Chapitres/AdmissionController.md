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

## GateKeeper
Un outil comme [GateKeeper](https://github.com/open-policy-agent/gatekeeper) adaptation dédiée kubernetes d'[OPA](https://www.openpolicyagent.org) utilise ce principe pour permettre de définir ce qui a le droit d'être fait ou pas sur votre cluster.
Par exemple, on peut déclarer des règles qui vont empêcher : 
- les objets Ingress de différents namespace de partager le même nom d'hôte.
- la récupération d'images docker hors de certaines registry.

Il existe des exemples de règles applicables [ici](https://github.com/open-policy-agent/gatekeeper-library).

[Retour](https://obeyler.github.io/Formation-K8S/Chapitres/Maj.html), [Menu](https://obeyler.github.io/Formation-K8S/), [Suite](https://obeyler.github.io/Formation-K8S/Tools/Helm.html)
