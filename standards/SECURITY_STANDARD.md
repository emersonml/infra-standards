# Security Standard

## Objetivo

Definir requisitos minimos de seguranca operacional.

## Regras gerais

- Nao alterar firewall sem autorizacao.
- Nao alterar DNS sem autorizacao.
- Nao alterar reverse proxy sem autorizacao.
- Nao alterar certificados sem autorizacao.
- Nao alterar Proxmox sem autorizacao.
- Nao publicar servicos diretamente na Internet.
- Nao armazenar segredos em documentacao.
- Nao commitar chaves privadas.
- Nao commitar dumps de banco sem saneamento e autorizacao.

## Principio de menor privilegio

- Acesso administrativo deve ser nominal.
- Login direto como root deve ser bloqueado por SSH.
- Grupos administrativos devem ser documentados.
- Permissoes de arquivos devem ser restritivas por padrao.

## Segredos

Proibido registrar:

- senhas;
- tokens;
- chaves privadas;
- cookies;
- dumps com dados sensiveis;
- arquivos `.env` com segredos em texto claro.

Permitido registrar:

- nomes de variaveis;
- caminhos;
- valores saneados;
- checksums;
- digests de imagens.

## Exposicao de servicos

Preferencia:

```text
127.0.0.1
LAN
Proxy
Internet
```

Publicacoes em `0.0.0.0` exigem justificativa.

## Auditoria

Toda alteracao relevante deve registrar:

- motivo;
- evidencia;
- arquivos alterados;
- impacto;
- rollback;
- validacao.
