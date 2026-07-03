# PROGRAM STATUS

## Status Geral

Institucionalizacao.

## Fase Atual

Institucionalizacao da Plataforma.

## Architecture Freeze

ATIVO.

Desde:

```text
2026-07-03
```

## Plano Ativo

```text
IMP-001
```

## Etapa Atual

```text
Aguardando autorizacao para Etapa 2 - ADRs estruturais minimas
```

## Ultima Etapa Concluida

```text
Sincronizacao institucional dos repositorios oficiais
```

Observacao:

```text
infra-standards, infra-runtime e infra-live-docs sincronizados com GitHub.
```

## Proxima Etapa

```text
IMP-001 - Etapa 2 - ADRs estruturais minimas
```

Status:

```text
Aguardando autorizacao explicita
```

## Projetos Liberados

```text
nenhum projeto novo liberado por este status
```

Observacao:

- operacoes correntes podem continuar conforme autorizacao especifica;
- novas evolucoes estruturais aguardam institucionalizacao.

## Projetos em Espera

- migracao da VM154 para Platform Workspace;
- atualizacao dos prompts-base;
- incorporacao definitiva de TechBase ao Projeto Colegio Intensivo;
- promocao documental de `PLATFORM_ARCHITECTURE.md`;
- promocao documental de `WORKSPACE_STANDARD.md`.

## Decisoes Arquiteturais Vigentes

- Architecture Freeze ativo.
- Platform Workspace para VMs de Engenharia.
- Operational Workspace para VMs de Servico.
- Personal Workspace fora da plataforma.
- Workspace como camada oficial da arquitetura.
- Principio do Privilegio Temporario.
- Relatorios locais como area transitoria.
- Documentacao viva em `infra-live-docs`.
- Git sem push sem autorizacao.

## Repositorios Oficiais

- `infra-standards`
- `infra-runtime`
- `infra-live-docs`

Status:

```text
sincronizados com GitHub em 2026-07-03
```

## Workspaces Oficiais

Platform Workspace:

```text
/home/emerson/platform
```

Operational Workspace:

```text
/opt/projects
```

## Standards Oficiais

- `AI_AGENT_STANDARD.md`
- `BACKUP_STANDARD.md`
- `DOCKER_STANDARD.md`
- `DOCUMENTATION_STANDARD.md`
- `GIT_STANDARD.md`
- `NETWORK_STANDARD.md`
- `NFS_STANDARD.md`
- `SECURITY_STANDARD.md`
- `SSH_STANDARD.md`
- `VM_STANDARD.md`

## ADRs

ADRs oficiais aprovadas:

- `0001-one-nfs-export-per-vm`
- `0002-artifacts-standard`
- `0003-documentation-standard`
- `0004-docker-layout`
- `0005-centralized-proxy`
- `0006-ipv4-only`
- `0007-migration-policy`
- `0008-ai-agent-governance`
- `0009-administrative-access-model`
- `0010-bootstrap-documentation-and-reports`

ADRs pendentes previstas:

- Platform Architecture as Master Index.
- Platform Workspace vs Operational Workspace.
- Reports and Live Docs Promotion Policy, se nao for absorvida por
  Documentation Standard.

## Riscos Conhecidos

- documentos recentes ainda estao em `/opt/projects`;
- standards antigos ainda citam `/opt/projects` sem distinguir VMs de
  Engenharia e VMs de Servico;
- relatorios locais ainda carregam decisoes que precisam de promocao.

## Pendencias

- autorizar IMP-001 Etapa 2;
- criar ADRs estruturais minimas;
- promover `PLATFORM_ARCHITECTURE.md`;
- promover `WORKSPACE_STANDARD.md`;
- reconciliar standards existentes;
- consolidar relatorios oficiais no `infra-live-docs`;
- planejar e executar migracao da VM154;
- atualizar prompts-base;
- incorporar TechBase ao Projeto Colegio Intensivo.

## Ultima Atualizacao

Data:

```text
2026-07-03
```

Responsavel:

```text
Codex
```

Motivo:

```text
Atualizacao apos sincronizacao institucional dos repositorios oficiais.
```
