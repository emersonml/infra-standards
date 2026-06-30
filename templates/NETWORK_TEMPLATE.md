# NETWORK - <VM_NAME>

Documentacao de rede da VM.

## Identidade

- Hostname:
- FQDN:
- Dominio/search:

## Interface principal

- Interface:
- IPv4:
- Gateway:
- DNS:

## SSH

```text
ssh -p 22123 emerson@<ip>
```

## Bastion / ProxyJump

- Bastion: <nao_aplicavel|hostname_ou_ip>
- ProxyJump:

```text
ssh -J <usuario>@<bastion>:22123 -p 22123 <usuario>@<ip-alvo>
```

## NFS

- Servidor:
- Export:
- Mount:
- Versao:

## Docker

- Redes:
- Portas publicadas:

## Politicas

- Operar em IPv4.
- Nao alterar firewall sem autorizacao.
- Nao alterar DNS sem autorizacao.
- Nao alterar proxy sem autorizacao.
- Nao publicar servicos diretamente na Internet.
