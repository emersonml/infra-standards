# NFS Standard

## Objetivo

Padronizar o uso de NFS para persistencia e artefatos da infraestrutura.

## Decisoes obrigatorias

- Cada VM possui apenas um export NFS.
- O export deve ser dedicado para a VM.
- A VM nao deve criar compartilhamentos no OMV sem autorizacao.
- O backup para segundo disco e responsabilidade do OMV.
- As VMs nao devem implementar copia para segundo disco local.

## Estrutura padrao

Toda VM que usa NFS deve montar em:

```text
/mnt/nfs
```

Estrutura:

```text
/mnt/nfs
 |
 +-- docker
 |   |
 |   +-- volumes
 |
 +-- artifacts
```

Volumes Docker:

```text
/mnt/nfs/docker/volumes
```

Artefatos de Recuperacao:

```text
/mnt/nfs/artifacts
```

## Validacoes obrigatorias

Antes de gravar dados:

```text
findmnt /mnt/nfs
df -h /mnt/nfs
stat -f -c %T /mnt/nfs
```

Antes de copiar artefatos:

- confirmar que `/mnt/nfs` e NFS real;
- validar leitura;
- validar escrita;
- nao usar diretorio local como substituto silencioso.

## Versao NFS

Estado atual:

- NFS v3 e usado onde NFS v4 ainda apresenta incompatibilidade.

Padrao futuro:

- migrar para NFS v4 apos identificar a causa da incompatibilidade.

## Regras de seguranca

- Nao alterar exports no OMV sem autorizacao.
- Nao alterar `/etc/fstab` sem backup e validacao.
- Nao mover persistencia sem snapshot, backup e rollback.
