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

Repositorio de padroes:

```text
infra-standards/
```

Finalidade:

- padroes globais;
- modelos;
- decisoes reutilizaveis;
- roadmap de padronizacao.

Documentacao viva da VM:

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
PHILOSOPHY > ARCHITECTURE > LIFECYCLE > REQUESTS (RFC) > DECISION RECORDS (ADR) > STANDARDS > TEMPLATES > VM DOCUMENTATION > SERVICES
```

Responsabilidades:

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

## Documentos minimos por VM

Toda VM deve possuir:

```text
/opt/projects/HOST.md
/opt/projects/.docs/CHANGELOG.md
/opt/projects/.docs/DECISIONS.md
/opt/projects/.docs/INVENTORY.md
/opt/projects/.docs/NETWORK.md
/opt/projects/.docs/TODO.md
```

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
