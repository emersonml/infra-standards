# Infrastructure Standards

Repositorio oficial dos padroes tecnicos da infraestrutura.

Este repositorio documenta normas, modelos e decisoes reutilizaveis em todas as
VMs e projetos. Ele nao substitui a documentacao viva de cada VM.

## Escopo

Este repositorio deve conter:

- padroes tecnicos da infraestrutura;
- modelos de documentacao;
- requisitos minimos para novas VMs;
- fluxo operacional obrigatorio;
- roadmaps de padronizacao.

Este repositorio nao deve conter:

- estado operacional detalhado de uma VM especifica;
- senhas;
- chaves privadas;
- dumps de banco;
- inventarios sensiveis nao saneados;
- artefatos binarios de recuperacao.

## Separacao de responsabilidades

Padroes globais:

```text
infra-standards > standards
infra-standards > templates
infra-standards > decision-records
infra-standards > roadmap
```

Documentacao viva de cada VM:

```text
/opt/projects/HOST.md
/opt/projects/.docs/
```

Regra:

- O repositorio GitHub documenta padroes da infraestrutura.
- Cada VM documenta apenas seu estado operacional real.
- Se houver divergencia entre documentacao e ambiente real, prevalece o ambiente real e a documentacao deve ser atualizada.

## Fluxo obrigatorio para intervencoes

Toda intervencao deve seguir:

```text
Diagnostico > Evidencias > Plano > Execucao > Validacao > Documentacao > Relatorio
```

Nenhuma alteracao estrutural deve ser executada por hipotese.

## Repository Structure

```text
README.md
PHILOSOPHY.md
ARCHITECTURE.md
LIFECYCLE.md

standards/
  DOCUMENTATION_STANDARD.md
  DOCKER_STANDARD.md
  NFS_STANDARD.md
  SSH_STANDARD.md
  VM_STANDARD.md
  SECURITY_STANDARD.md
  BACKUP_STANDARD.md
  NETWORK_STANDARD.md

templates/
  HOST_TEMPLATE.md
  CHANGELOG_TEMPLATE.md
  DECISIONS_TEMPLATE.md
  INVENTORY_TEMPLATE.md
  NETWORK_TEMPLATE.md
  TODO_TEMPLATE.md

decision-records/
  0001-one-nfs-export-per-vm.md
  0002-artifacts-standard.md
  0003-documentation-standard.md
  0004-docker-layout.md
  0005-centralized-proxy.md
  0006-ipv4-only.md
  0007-migration-policy.md

requests/
  README.md
  RFC_TEMPLATE.md

roadmap/
  ROADMAP.md
```

Meaning:

- `README.md`: repository entry point.
- `PHILOSOPHY.md`: permanent infrastructure principles.
- `ARCHITECTURE.md`: macro architecture and governance.
- `LIFECYCLE.md`: standard VM lifecycle.
- `requests/`: proposed infrastructure changes.
- `decision-records/`: approved architectural decisions.
- `standards/`: reusable technical standards.
- `templates/`: local VM documentation templates.
- `roadmap/`: standardization roadmap.

## Repository Governance

The repository governance model defines how an idea becomes infrastructure
policy.

Documents and responsibilities:

- `README.md`: entry point and repository map.
- `PHILOSOPHY.md`: permanent infrastructure principles.
- `ARCHITECTURE.md`: macro architecture and governance model.
- `LIFECYCLE.md`: standard VM lifecycle.
- `decision-records/`: approved architectural decisions.
- `requests/`: RFCs and proposed changes.
- `standards/`: official reusable rules.
- `templates/`: reusable documentation starting points.
- `roadmap/`: planned standardization work.

Evolution flow:

```text
Idea > RFC > Review > Approval > Decision Record > Standard > Template > VM > Service
```

Rules:

- RFC represents a proposal.
- ADR represents an approved decision.
- Standard represents an official rule.
- No infrastructure evolution should become a Standard directly.
- Every approved Standard must have a corresponding Decision Record.

## Padrao de escrita

- Usar somente Markdown.
- Usar somente caracteres ASCII.
- Escrever documentacao tecnica objetiva.
- Evitar duplicacao de conteudo.
- Atualizar a documentacao sempre que um padrao mudar.

## Arquitetura

A visao macro da infraestrutura esta documentada em:

```text
ARCHITECTURE.md
```

Esse documento explica como GitHub, VM152 Codex, VMs, storage, Docker, NFS,
artefatos, backups, documentacao, automacao, seguranca, observabilidade e
migracoes se conectam.

## Decisoes arquiteturais registradas

- A infraestrutura mantem padroes proprios.
- Cada VM possui apenas um export NFS.
- Volumes Docker ficam em `/mnt/nfs/docker/volumes`.
- Artefatos de Recuperacao ficam em `/mnt/nfs/artifacts`.
- Backup para segundo disco e responsabilidade do OMV, nao das VMs.
- SSH deve ser padronizado futuramente por `sshd_config.d`.
- O arquivo principal `sshd_config` nao deve concentrar customizacoes futuras.
