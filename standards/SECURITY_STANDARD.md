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
- Agentes devem possuir apenas as permissoes necessarias para a tarefa
  aprovada.
- Nenhum privilegio permanente deve existir apenas por conveniencia.
- Privilegios temporarios devem ser revogados ao final da tarefa.

## Role-Based Just-In-Time Privilege

Classificacao:

- Arquitetura
- Governanca
- Padrao
- Seguranca

Regra:

```text
Emerson autoriza > Policy Broker materializa privilegios > Codex prepara >
Codex revoga privilegios temporarios > Claude executa > Codex valida
```

Regras:

- Codex pode receber acesso administrativo temporario apenas com autorizacao
  explicita de Emerson;
- Codex deve preparar diretorios, ownership, grupos, ACLs e politicas por meio
  do Policy Broker quando necessario;
- Codex deve remover seus proprios privilegios temporarios antes de delegar a
  execucao ao Claude;
- Claude deve executar somente com permissoes previamente preparadas;
- Claude nao deve alterar sudoers, grupos administrativos ou seus proprios
  privilegios;
- agentes nao devem editar `/etc/sudoers` ou `/etc/sudoers.d` diretamente;
- o estado final deve retornar ao minimo de privilegios necessario.

Documento de referencia:

```text
standards/PRIVILEGE_GOVERNANCE_STANDARD.md
```

## Controle de acesso administrativo

Classificacao:

- Governanca
- Padrao

Usuarios padrao:

- `emerson`: operador humano.
- `codex-infra`: agente Codex.
- `claude-infra`: agente Claude.

Grupos padrao:

- `_ssh`: login SSH.
- `infra-collab`: colaboracao em projetos, documentacao e arquivos
  operacionais.
- `infra-architect`: elegibilidade de arquitetura e preparacao.
- `infra-engineer`: elegibilidade de engenharia e execucao.
- `infra-audit`: leitura e revisao futura.

Regras:

- acesso humano e acesso de agentes devem ser separados;
- agentes devem usar usuarios nominais proprios;
- contas de agentes nao devem compartilhar chaves privadas com `emerson`;
- grupos de papel indicam elegibilidade, nao privilegio administrativo direto;
- `infra-admin` e legado e nao deve ser usado como novo padrao de autoridade;
- sudo permanente para agentes exige justificativa documentada;
- sudo temporario para bootstrap deve ser substituido por Policy Broker,
  removido ou reduzido ao final;
- alteracoes em SSH, sudo ou grupos exigem rollback documentado;
- Codex e o controlador de privilegios durante janelas Just-In-Time aprovadas;
- Claude e o executor tecnico e nao deve ampliar seus proprios privilegios.

## Politica de Bastion

Classificacao:

- Arquitetura
- Padrao

Bastion e um ponto controlado de entrada administrativa.

Regras:

- Bastion nao remove a necessidade de autorizacao;
- ProxyJump deve preservar identidade nominal;
- acesso direto fora do Bastion deve ser documentado quando permitido;
- mudancas no Bastion exigem validacao de SSH, SCP e RSYNC.

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

## Tentativas repetidas

Classificacao:

- Procedimento
- Boas praticas

Se uma operacao falhar repetidamente, o operador ou agente deve parar e pedir
orientacao antes de novas tentativas quando:

- a causa raiz nao estiver clara;
- a terceira tentativa puder alterar estado;
- a proxima tentativa exigir privilegio maior;
- a proxima tentativa puder afetar acesso, dados, rede ou producao.
