# Network Standard

## Objetivo

Padronizar regras de rede da infraestrutura.

## Politica IPv4

A infraestrutura opera em IPv4.

IPv6 nao deve ser usado salvo autorizacao explicita.

## SSH

Padrao:

```text
ssh -p 22123 emerson@<ip>
```

Prioridade:

- autenticacao por chave;
- usuario nominal;
- grupo administrativo.

## Bastion e ProxyJump

Classificacao:

- Arquitetura
- Padrao
- Procedimento

Quando houver Bastion aprovado, a administracao remota deve preferir:

```text
Operator or Agent > Bastion > Target VM
```

Comando de referencia:

```text
ssh -J <usuario>@<bastion>:22123 -p 22123 <usuario>@<ip-alvo>
```

Regras:

- documentar Bastion no `NETWORK.md` da VM quando usado;
- manter usuario nominal no salto e no destino;
- nao usar Bastion para contornar firewall, DNS, proxy ou autorizacao;
- validar `ssh`, `scp` e `rsync` quando o caminho de acesso mudar.

## Exposicao

Ordem preferida:

```text
127.0.0.1
LAN
Proxy
Internet
```

Nao publicar servicos diretamente na Internet.

## Docker

Portas devem ser publicadas somente quando necessario.

Preferir bind em:

```text
127.0.0.1
```

Quando usar `0.0.0.0`, registrar justificativa em `NETWORK.md` da VM.

## NFS

Servidor NFS padrao:

```text
10.87.0.130
```

Mount local:

```text
/mnt/nfs
```

Persistencia:

```text
/mnt/nfs/docker/volumes
```

Artefatos:

```text
/mnt/nfs/artifacts
```

## Alteracoes proibidas sem autorizacao

- firewall;
- DNS;
- proxy;
- certificados;
- exports NFS;
- rotas estruturais.
