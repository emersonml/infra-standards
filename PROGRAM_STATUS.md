# PROGRAM STATUS

## Status Geral

Institucionalizacao publicada.

## Fase Atual

Institucionalizacao da Plataforma - IMP-001 concluido oficialmente.

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
IMP-001 encerrado
```

## Ultima Etapa Concluida

```text
IMP-001 - Etapa 9 - Validacao final de institucionalizacao
```

Observacao:

```text
Institucionalizacao publicada nos repositorios oficiais.
```

## Proxima Etapa

```text
A definir pelo Arquiteto-Chefe
```

Status:

```text
Sem etapa IMP-001 pendente
```

## Projetos Liberados

```text
nenhum projeto novo liberado por este status
```

Observacao:

- operacoes correntes podem continuar conforme autorizacao especifica;
- novas evolucoes estruturais exigem autorizacao propria.

## Projetos em Espera

```text
nenhum projeto adicional em espera por nomenclatura historica
```

## Decisoes Arquiteturais Vigentes

- Architecture Freeze ativo.
- Platform Architecture como indice mestre da plataforma.
- Platform Workspace para VMs de Engenharia.
- Operational Workspace para VMs de Servico.
- Personal Workspace fora da plataforma.
- Workspace como camada oficial da arquitetura.
- Principio do Privilegio Temporario.
- Least Privilege como principio oficial para agentes de IA.
- Just-In-Time Privilege como modelo oficial de elevacao temporaria.
- RBAC funcional para agentes de IA.
- Policy Broker como camada oficial de materializacao de privilegios
  temporarios.
- `infra-collab` como grupo de colaboracao sem autoridade administrativa.
- `infra-admin` como legado, nao como novo padrao de autoridade.
- Codex como controlador de governanca e preparacao de privilegios da
  plataforma.
- Claude como executor tecnico dentro de permissoes preparadas.
- `root-admin` como identidade administrativa primaria padrao para novos
  paineis Web de aplicacao.
- `pt_BR`, `BR` e `America/Maceio` como regionalizacao padrao para novas
  aplicacoes quando suportado.
- `ForwardAgent no`, `IdentityFile` explicito, `IdentitiesOnly yes` e
  `ProxyJump` como padrao de cliente SSH institucional.
- Relatorios locais como area transitoria.
- Promocao de relatorios oficiais para `infra-live-docs`.
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
- `APPLICATION_ACCESS_STANDARD.md`
- `APPLICATION_CONFIGURATION_STANDARD.md`
- `BACKUP_STANDARD.md`
- `DOCKER_STANDARD.md`
- `DOCUMENTATION_STANDARD.md`
- `GIT_STANDARD.md`
- `NETWORK_STANDARD.md`
- `NFS_STANDARD.md`
- `PRIVILEGE_GOVERNANCE_STANDARD.md`
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
- `0011-platform-architecture-master-index`
- `0012-platform-workspace-vs-operational-workspace`
- `0013-reports-and-live-docs-promotion-policy`
- `0014-ai-agent-jit-privilege-model`
- `0015-role-based-agent-privilege-model`
- `0016-application-primary-admin-identity`
- `0017-application-regional-defaults`
- `0018-ssh-client-forward-agent-policy`

ADRs pendentes previstas:

```text
nenhuma ADR estrutural minima pendente no escopo atual do IMP-001
```

## Riscos Conhecidos

- documentos historicos fora de `standards/` ainda podem citar `/opt/projects`
  como padrao unico;
- relatorios locais ainda existem como fila transitoria apos consolidacao;
- documentos reais da VM154 ainda precisam ser classificados e consolidados;
- relatorios historicos preservam mencoes ao nome anterior como evidencia.

## Pendencias

- classificar e consolidar documentos reais da VM154 quando a governanca
  definir destino;
- manter PROGRAM_STATUS atualizado em futuras atividades aprovadas.

## Ultima Atualizacao

Data:

```text
2026-07-06
```

Responsavel:

```text
Codex
```

Motivo:

```text
Registro dos padroes `root-admin`, regionalizacao de aplicacoes e politica de
cliente SSH com `ForwardAgent no`.
```
