# Backup Standard

## Objetivo

Padronizar backups, Artefatos de Recuperacao e responsabilidades.

## Decisao arquitetural

Backup para segundo disco:

- responsabilidade do OMV;
- nunca responsabilidade direta das VMs.

As VMs devem produzir e manter Artefatos de Recuperacao quando aplicavel.

## Artefatos de Recuperacao

Local padrao:

```text
/mnt/nfs/artifacts
```

Conteudo recomendado:

- `docker-compose.yml`;
- `Dockerfile`, quando existir;
- `.env` original com permissao restrita, se autorizado;
- `env.redacted` sem segredos;
- dump logico de banco;
- imagens Docker exportadas quando criticas;
- manifesto;
- procedimento de restore;
- checksums SHA256.

## Validacao obrigatoria

Para cada artefato:

- validar quantidade de arquivos;
- validar tamanho total;
- validar checksums;
- validar restore logico quando possivel;
- validar tarballs de imagem quando existirem.

## Regras

- Nao gravar banco persistente em disco local sem autorizacao.
- Nao usar diretorio local como substituto de NFS.
- Nao considerar backup valido sem validacao.
- Nao misturar backup com upgrade de versao.

## Restore

Todo artefato deve possuir instrucao de restore:

```text
restore/RESTORE.md
```

O restore deve declarar:

- origem dos dados;
- destino esperado;
- comandos;
- validacoes;
- rollback;
- riscos.
