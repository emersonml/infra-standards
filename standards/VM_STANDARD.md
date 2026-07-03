# VM Standard

## Objetivo

Padronizar a organizacao minima de VMs da infraestrutura.

## Tipos de workspace

VMs de Engenharia usam Platform Workspace:

```text
/home/emerson/platform
```

VMs de Servico usam Operational Workspace:

```text
/opt/projects
```

Diretorios pessoais como `~/Downloads`, `~/Desktop` e `~/Documents` nao fazem
parte da plataforma.

## Estrutura obrigatoria para VMs de Engenharia

VMs de Engenharia devem possuir:

```text
/home/emerson/platform
/home/emerson/platform/infra-standards
/home/emerson/platform/infra-runtime
/home/emerson/platform/infra-live-docs
/home/emerson/platform/workspace
/home/emerson/platform/reports
/home/emerson/platform/inbox
/home/emerson/platform/scratch
/home/emerson/platform/README.md
```

## Estrutura obrigatoria para VMs de Servico

VMs de Servico devem possuir:

```text
/opt/projects
/opt/projects/HOST.md
/opt/projects/.docs
/opt/projects/reports
```

Documentos minimos:

```text
/opt/projects/.docs/CHANGELOG.md
/opt/projects/.docs/DECISIONS.md
/opt/projects/.docs/INVENTORY.md
/opt/projects/.docs/NETWORK.md
/opt/projects/.docs/TODO.md
```

## Responsabilidade da documentacao viva

Cada VM deve documentar:

- funcao;
- IP;
- hostname;
- recursos;
- servicos;
- volumes;
- redes;
- estado operacional;
- pendencias;
- decisoes locais.

Relatorios operacionais devem ficar em:

```text
reports
```

Regra:

- em VMs de Engenharia, `reports` fica em `/home/emerson/platform/reports`;
- em VMs de Servico, `reports` fica em `/opt/projects/reports`;
- relatorios locais registram execucao e evidencia;
- relatorios locais sao transitorios ate consolidacao oficial em
  `infra-live-docs`;
- relatorios nao substituem `HOST.md`, `.docs/`, `infra-standards` ou
  `infra-live-docs`.

## Padroes de sistema

Recomendacoes:

- Debian 12 ou versao aprovada;
- timezone da infraestrutura;
- NTP ativo;
- QEMU Guest Agent ativo quando VM estiver em Proxmox;
- acesso SSH pela porta padrao `22123`;
- IPv4 como protocolo operacional.

## Identidade administrativa

Classificacao:

- Governanca
- Padrao

Usuarios padrao:

- `emerson`;
- `codex-infra`;
- `claude-infra`.

Grupos padrao:

- `infra-admin`;
- `_ssh`.

Regras:

- `emerson` representa o operador humano;
- `codex-infra` e `claude-infra` representam agentes IA;
- `_ssh` controla login SSH;
- `infra-admin` controla administracao;
- sudo temporario de bootstrap deve ser removido ou reduzido depois da
  validacao.

## Bootstrap institucional

Classificacao:

- Procedimento
- Padrao

Bootstrap minimo de VM de Servico:

```text
Base VM > Identity > SSH > /opt/projects > Documentation > NFS > Docker > Snapshot > Report
```

Bootstrap minimo de VM de Engenharia:

```text
Base VM > Identity > SSH > /home/emerson/platform > Repositories > README > PROGRAM_STATUS > Report
```

Saidas obrigatorias:

- usuarios e grupos padrao avaliados;
- SSH validado;
- workspace oficial criado conforme tipo da VM;
- documentacao viva criada;
- diretorio local de relatorios criado;
- NFS validado quando aplicavel;
- Docker validado quando aplicavel;
- snapshot recomendado ou criado conforme risco;
- relatorio operacional criado.

## Mudancas estruturais

Antes de qualquer mudanca estrutural:

- recomendar snapshot;
- registrar diagnostico;
- registrar plano;
- definir rollback;
- validar apos execucao;
- atualizar documentacao viva.

## Limite entre VM e repositorio

Repositorio:

- padroes gerais.

VM:

- estado real e operacional.
