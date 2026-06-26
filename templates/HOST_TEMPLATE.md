# HOST - <VM_NAME>

Ultima atualizacao: YYYY-MM-DD

## Identificacao

- VMID: <vmid>
- Hostname: <hostname>
- Funcao: <funcao>
- Sistema: <sistema>

## Rede

- IPv4: <ip>/<cidr>
- Gateway: <gateway>
- DNS: <dns>
- SSH: `ssh -p 22123 emerson@<ip>`

## Recursos

- CPU: <cpu>
- Memoria: <memoria>
- Disco raiz: <disco>
- NFS: <estado>

## Diretorios padrao

```text
/opt/projects
/opt/projects/.docs
/mnt/nfs
/mnt/nfs/docker/volumes
/mnt/nfs/artifacts
```

## Servicos

- <servico>: <estado>

## Regras locais

- Ler este arquivo antes de qualquer alteracao.
- Ler todos os arquivos em `/opt/projects/.docs/`.
- Atualizar documentacao apos mudancas.

## Documentacao complementar

```text
/opt/projects/.docs/CHANGELOG.md
/opt/projects/.docs/DECISIONS.md
/opt/projects/.docs/INVENTORY.md
/opt/projects/.docs/NETWORK.md
/opt/projects/.docs/TODO.md
```
