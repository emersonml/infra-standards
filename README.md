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
  AI_AGENT_STANDARD.md
  DOCUMENTATION_STANDARD.md
  GIT_STANDARD.md
  DOCKER_STANDARD.md
  NFS_STANDARD.md
  PRIVILEGE_GOVERNANCE_STANDARD.md
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
  OPERATIONAL_REPORT_TEMPLATE.md
  TODO_TEMPLATE.md

decision-records/
  0001-one-nfs-export-per-vm.md
  0002-artifacts-standard.md
  0003-documentation-standard.md
  0004-docker-layout.md
  0005-centralized-proxy.md
  0006-ipv4-only.md
  0007-migration-policy.md
  0008-ai-agent-governance.md
  0009-administrative-access-model.md
  0010-bootstrap-documentation-and-reports.md
  0011-platform-architecture-master-index.md
  0012-platform-workspace-vs-operational-workspace.md
  0013-reports-and-live-docs-promotion-policy.md
  0014-ai-agent-jit-privilege-model.md

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

## Classificacao

Os documentos deste repositorio devem classificar regras e decisoes conforme a
funcao do conteudo.

Classificacoes oficiais:

- Arquitetura: estrutura permanente da infraestrutura.
- Governanca: autoridade, responsabilidade e fluxo de aprovacao.
- Template: modelo reutilizavel para documentacao local ou relatorio.
- Padrao: regra tecnica reutilizavel.
- Procedimento: sequencia operacional executavel por humano ou agente.
- Boas praticas: recomendacao que reduz risco, sem substituir uma regra.

## Git Policy

Git usage in this repository separates local work records from official remote
publication.

Policy:

- Local commit represents work record.
- Remote push represents official publication.
- Codex may create local commits during work.
- Codex must not push automatically after every commit.
- Push requires publication criteria or explicit Emerson request.

Local commit flow:

```text
Change > Validate > Commit > Continue
```

Remote push flow:

```text
Review > Clean Worktree > Count Local Commits > Show Pending Commits > Push > Confirm Remote
```

Detailed rules are defined in:

```text
standards/GIT_STANDARD.md
```

## Padrao de escrita

- Usar somente Markdown.
- Usar somente caracteres ASCII.
- Escrever documentacao tecnica objetiva.
- Evitar duplicacao de conteudo.
- Atualizar a documentacao sempre que um padrao mudar.

## Separacao entre standards e runtime

O repositorio `infra-standards` concentra decisoes permanentes:

- arquitetura;
- padroes;
- politicas;
- governanca;
- templates;
- ciclo de vida;
- decisoes aprovadas.

O repositorio `infra-runtime` deve permanecer responsavel por componentes
executaveis:

- scripts;
- automacoes;
- bootstrap executavel;
- utilitarios;
- componentes operacionais.

Regra:

- uma decisao permanente descoberta no runtime deve ser migrada para
  `infra-standards`;
- um comando, script ou automacao deve permanecer no `infra-runtime`;
- standards nao devem conter codigo executavel.

## Documentacao oficial e relatorios

Documentacao oficial:

- `infra-standards`;
- `/opt/projects/HOST.md`;
- `/opt/projects/.docs/`.

Relatorios operacionais:

- registram execucao, evidencias, validacao e resultado;
- nao substituem standards nem documentacao viva;
- devem ficar em `/opt/projects/reports/` quando aplicavel.

Formato de relatorio:

```text
REL-YYYY-MM-DD-NNN-descricao.md
```

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
- Acesso administrativo separa operador humano e agentes IA.
- Usuarios padrao: `emerson`, `codex-infra` e `claude-infra`.
- Grupos padrao: `infra-admin` para administracao e `_ssh` para login SSH.
- Acesso remoto deve preferir Bastion e ProxyJump quando houver segmentacao.
- Bootstrap institucional pode usar sudo temporario, removido ao final.
- Codex e controlador temporario de privilegios em janelas autorizadas.
- Claude executa apenas com permissoes previamente preparadas.
- Least Privilege e Just-In-Time Privilege sao principios oficiais.
- Relatorios operacionais ficam separados da documentacao oficial.
