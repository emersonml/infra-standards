# VM Standard

## Objetivo

Padronizar a organizacao minima de VMs da infraestrutura.

## Estrutura obrigatoria

Toda VM deve possuir:

```text
/opt/projects
/opt/projects/HOST.md
/opt/projects/.docs
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

## Padroes de sistema

Recomendacoes:

- Debian 12 ou versao aprovada;
- timezone da infraestrutura;
- NTP ativo;
- QEMU Guest Agent ativo quando VM estiver em Proxmox;
- acesso SSH pela porta padrao `22123`;
- IPv4 como protocolo operacional.

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
