# Docker Standard

## Objetivo

Padronizar o uso de Docker nas VMs da infraestrutura.

## Padrao de diretorios para VMs de Servico

Codigo e configuracao:

```text
/opt/projects
```

Regra:

- `/opt/projects` e o Operational Workspace das VMs de Servico;
- stacks Docker permanentes devem manter compose e configuracao operacional
  nesse workspace;
- Platform Workspace de VMs de Engenharia nao e destino padrao para stacks de
  servico.

Persistencia Docker:

```text
/mnt/nfs/docker/volumes
```

Artefatos:

```text
/mnt/nfs/artifacts
```

## Regras obrigatorias

- Usar Docker Compose para stacks.
- Nao usar `docker run` para componentes permanentes de stack.
- Manter arquivos Compose de VMs de Servico em `/opt/projects`.
- Manter dados persistentes em `/mnt/nfs/docker/volumes`.
- Nao gravar bancos de dados em disco local da VM sem autorizacao explicita.
- Nao atualizar imagens durante migracao, salvo plano especifico.
- Nao reconstruir imagem critica quando o requisito for preservar versao.

## Validacoes minimas

Antes de aplicar uma stack:

```text
docker compose config
docker compose ps
```

Apos aplicar:

```text
docker compose ps
docker compose logs
healthcheck da aplicacao
```

## Volumes

Volumes devem usar paths explicitos sob:

```text
/mnt/nfs/docker/volumes
```

Recomendacao:

- mapear UID/GID antes do primeiro start;
- validar ownership apos criacao;
- documentar volumes em `INVENTORY.md` da VM.

## Imagens

Regras:

- registrar imagem, tag e digest quando a imagem for critica;
- preservar imagens customizadas por `docker save` quando nao forem reproduziveis;
- evitar tag `latest` em novos projetos;
- se `latest` existir por legado, documentar o digest real.

## Proibido sem plano

- `docker compose down` em producao;
- `docker image prune`;
- `docker volume rm`;
- alterar paths persistentes;
- trocar tags de imagem em janela de migracao.
