# VM Standard

## Objetivo

Padronizar a organizacao minima de VMs da infraestrutura.

## Estrutura obrigatoria

Toda VM deve possuir:

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
/opt/projects/reports
```

Relatorios registram execucao e evidencia. Eles nao substituem `HOST.md`,
`.docs/` ou `infra-standards`.

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

Bootstrap minimo:

```text
Base VM > Identity > SSH > /opt/projects > Documentation > NFS > Docker > Snapshot > Report
```

Saidas obrigatorias:

- usuarios e grupos padrao avaliados;
- SSH validado;
- `/opt/projects` criado;
- documentacao viva criada;
- `/opt/projects/reports` criado;
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
