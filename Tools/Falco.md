# Falco

Falco ([https://falco.org](https://falco.org)) est installé sous forme de service linux ou sous forme de pod via un DaemonSet. 
Son but est de surveiller et détecter les comportements "anormaux" afin de prévenir l'administrateur.
Les comportements peuvent être liés à des actions utilisateurs (tentative de hack...) ou aux actions d'un container (virus, criptomining,...).

Il fournit un ensemble de règles qui peut être surchargées pour s'adapter à votre contexte.

Exemple de règle :
```yaml
- rule: run_shell_in_container
  desc: a shell was spawned by a non-shell program in a container. Container entrypoints are excluded.
  condition: container and proc.name = bash and spawned_process and proc.pname exists and not proc.pname in (bash, docker)
  output: "Shell spawned in a container other than entrypoint (user=%user.name container_id=%container.id container_name=%container.name shell=%proc.name parent=%proc.pname cmdline=%proc.cmdline)"
  priority: WARNING
```
Dans ce cas on vise à détecter qu'un shell a été lancé depuis un container.

d'autres exemples sont disponibles ici: [https://falco.org/docs/examples/](https://falco.org/docs/examples/)
