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
