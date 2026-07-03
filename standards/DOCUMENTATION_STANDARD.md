# Documentation Standard

## Objetivo

Definir o padrao de documentacao tecnica da infraestrutura.

## Regras gerais

- Usar somente Markdown.
- Usar somente caracteres ASCII.
- Escrever de forma tecnica, objetiva e auditavel.
- Nao duplicar documentacao sem necessidade.
- Registrar evidencias quando uma decisao depender do estado real.
- Atualizar a documentacao quando houver mudanca de padrao.

## Separacao de escopo

Estado oficial resumido da plataforma:

```text
infra-standards/PROGRAM_STATUS.md
```

Finalidade:

- informar fase atual;
- informar plano ativo;
- informar etapa atual;
- registrar riscos e pendencias vigentes;
- contextualizar rapidamente agentes humanos e IA.

Indice arquitetural da plataforma:

```text
infra-standards/PLATFORM_ARCHITECTURE.md
```

Finalidade:

- conectar os padroes existentes;
- orientar o fluxo de leitura;
- evitar duplicacao de conteudo tecnico dos standards.

Repositorio de padroes:

```text
infra-standards/
```

Finalidade:

- padroes globais;
- modelos;
- decisoes reutilizaveis;
- roadmap de padronizacao.

Workspace de VMs de Engenharia:

```text
/home/emerson/platform
```

Finalidade:

- concentrar repositorios institucionais;
- fornecer onboarding oficial por `README.md`;
- manter areas locais transitorias como `workspace`, `reports`, `inbox` e
  `scratch`.

Documentacao viva de VMs de Servico:

```text
/opt/projects/HOST.md
/opt/projects/.docs/
```

Finalidade:

- estado operacional real da VM;
- inventario local;
- historico local;
- decisoes locais;
- pendencias locais.

Relatorios operacionais:

```text
reports/
```

Finalidade:

- registrar execucao operacional;
- registrar evidencias e validacao;
- registrar arquivos alterados;
- registrar rollback e riscos;
- registrar resultado final.

Regra:

- relatorio operacional local e transitorio;
- relatorio operacional nao e fonte oficial de arquitetura;
- relatorio operacional nao substitui `HOST.md` ou `.docs/`;
- decisao permanente descoberta em relatorio deve ser promovida para ADR,
  Standard ou Template quando aprovada.
- relatorio oficial torna-se institucional somente depois de consolidado em
  `infra-live-docs`.

Classificacao:

- Governanca
- Padrao
- Procedimento

## Fluxo obrigatorio

Todo trabalho operacional deve seguir:

```text
Diagnostico > Evidencias > Plano > Execucao > Validacao > Documentacao > Relatorio
```

## Flow Notation

Todos os fluxos da documentacao devem utilizar o formato compacto:

```text
Step 1 > Step 2 > Step 3 > Step 4
```

Evitar diagramas verticais.

Priorizar leitura, diffs Git e facilidade de manutencao.

Exemplos:

```text
Template > Clone > Hostname > Network > SSH > NFS > Docker > Documentation > Snapshot > Ready
Discovery > Documentation > Artifacts > Pre-flight > Migration > Validation > Cutover > Rollback > Final Documentation
```

## Documentation Hierarchy

Hierarquia oficial:

```text
PROGRAM_STATUS > PLATFORM_ARCHITECTURE > PHILOSOPHY > ARCHITECTURE > LIFECYCLE > REQUESTS (RFC) > DECISION RECORDS (ADR) > STANDARDS > TEMPLATES > VM DOCUMENTATION > SERVICES
```

Responsabilidades:

- `PROGRAM_STATUS.md`: estado oficial resumido.
- `PLATFORM_ARCHITECTURE.md`: indice arquitetural da plataforma.
- `PHILOSOPHY.md`: principios permanentes.
- `ARCHITECTURE.md`: visao macro e governanca.
- `LIFECYCLE.md`: ciclo de vida padrao das VMs.
- `requests/`: propostas de mudanca.
- `decision-records/`: decisoes aprovadas.
- `standards/`: regras oficiais.
- `templates/`: modelos reutilizaveis.
- VM documentation: estado local e excecoes.
- Services: implementacao operacional.

Regra:

- Documentos de nivel inferior nunca podem contrariar documentos de nivel superior.
- Quando houver conflito, o documento inferior deve ser corrigido ou uma RFC deve propor mudanca no nivel superior.

## Documentos minimos por tipo de VM

VMs de Engenharia devem possuir Platform Workspace:

```text
/home/emerson/platform/README.md
/home/emerson/platform/infra-standards/
/home/emerson/platform/infra-runtime/
/home/emerson/platform/infra-live-docs/
/home/emerson/platform/workspace/
/home/emerson/platform/reports/
/home/emerson/platform/inbox/
/home/emerson/platform/scratch/
```

VMs de Servico devem possuir Operational Workspace:

```text
/opt/projects/HOST.md
/opt/projects/.docs/CHANGELOG.md
/opt/projects/.docs/DECISIONS.md
/opt/projects/.docs/INVENTORY.md
/opt/projects/.docs/NETWORK.md
/opt/projects/.docs/TODO.md
```

Diretorio minimo de relatorios:

```text
reports/
```

Regra:

- em Platform Workspace, `reports/` fica sob `/home/emerson/platform`;
- em Operational Workspace, `reports/` fica sob `/opt/projects`;
- ambos sao areas locais transitorias ate consolidacao oficial.

## Operational Report Policy

Reports must use:

```text
REL-YYYY-MM-DD-NNN-description.md
```

Reports must include:

- objective;
- files changed;
- new standards formalized;
- items migrated from runtime;
- commits created;
- push status;
- validation;
- recommended next step.

Template:

```text
templates/OPERATIONAL_REPORT_TEMPLATE.md
```

Official consolidation:

- local reports are transitional by default;
- official live reports belong in `infra-live-docs` after explicit
  consolidation;
- local reports must not replace ADRs, standards or live docs.

## Git Documentation Policy

Git history is part of the repository documentation.

Policy:

- Local commit records completed work.
- Remote push publishes official state.
- Commit messages must describe the documentation change.
- Push must be deliberate and validated.
- Pending local commits must be reported before push.

Required validation before push:

```text
git status
git log --oneline origin/main..HEAD
```

If worktree is not clean, do not push.

If local and remote diverge, stop and report.

## Formato de roadmap

Usar:

```text
[OK ] Item concluido
[TODO] Item pendente
```

## Boas praticas

- Preferir listas simples.
- Evitar tabelas quando uma lista for suficiente.
- Manter comandos em blocos separados.
- Nao registrar segredos.
- Nao registrar chaves privadas.
- Nao registrar senhas.
