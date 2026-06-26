# ROADMAP - Infrastructure Standards

## Padronizacao inicial

[OK ] Criar repositorio oficial de padroes da infraestrutura.
[OK ] Criar estrutura inicial de standards.
[OK ] Criar templates de documentacao viva das VMs.
[OK ] Registrar fluxo obrigatorio de intervencao.

## SSH

[TODO] Padronizar SSH usando `sshd_config.d`.
[TODO] Eliminar customizacoes diretas no `sshd_config` principal.
[TODO] Validar modelo SSH em VM de laboratorio.
[TODO] Atualizar VM Template com o padrao SSH aprovado.
[TODO] Publish SSH Standard.

## NFS

[TODO] Migrar NFS v3 para NFS v4 apos identificar a causa da incompatibilidade.
[TODO] Validar export unico por VM em todas as VMs.
[TODO] Validar estrutura `/mnt/nfs/docker/volumes` e `/mnt/nfs/artifacts`.

## Docker

[TODO] Padronizar estrutura Docker.
[TODO] Padronizar uso de Docker Compose.
[TODO] Inventariar imagens criticas e digests.
[TODO] Publish Docker Standard.

## Scripts

[TODO] Padronizar scripts de manutencao.
[TODO] Definir contratos para `backup.sh`, `restore.sh`, `update.sh`, `healthcheck.sh` e `maintenance.sh`.

## Documentacao

[TODO] Padronizar documentacao das VMs.
[TODO] Aplicar templates em novas VMs.
[TODO] Revisar VMs existentes contra o padrao.
[TODO] Consolidate Documentation standard.

## Artefatos de Recuperacao

[TODO] Padronizar politica de Artefatos de Recuperacao.
[TODO] Definir manifesto minimo.
[TODO] Definir validacao obrigatoria por checksums.
[TODO] Standardize Recovery Artifacts.

## Decision Records

[TODO] Complete Decision Records.

## Backup

[TODO] Publish Backup Standard.
[TODO] Define second-disk backup policy on OMV.

## Monitoring

[TODO] Publish Monitoring Standard.

## Templates

[TODO] Create Linux Template Standard.
