# Privilege Governance Standard

## Objetivo

Definir o modelo oficial de governanca de privilegios para operadores humanos
e agentes de IA.

Este padrao formaliza:

- principio de menor privilegio;
- privilegio Just-In-Time;
- Codex como controlador temporario de privilegios;
- Claude como executor tecnico sem autoelevacao;
- separacao entre preparacao, execucao, validacao e revogacao.

## Classificacao

- Arquitetura
- Governanca
- Padrao
- Procedimento
- Seguranca

## Cadeia de autoridade

Fluxo oficial:

```text
Emerson
 |
 v
Codex
 |
 v
Claude
```

Responsabilidades:

- Emerson: Arquiteto-Chefe, aprovacao final e autorizacao explicita.
- Codex: arquitetura, planejamento, seguranca, governanca e preparacao de
  privilegios.
- Claude: engenharia e execucao tecnica dentro das permissoes preparadas.

## Principio de menor privilegio

Regra:

```text
Todo agente deve possuir apenas as permissoes necessarias para a tarefa
aprovada.
```

Regras:

- privilegios permanentes nao devem existir por conveniencia;
- acesso SSH nao implica permissao administrativa;
- pertencer a `_ssh` permite login SSH, nao administracao;
- pertencer a `infra-admin` indica elegibilidade administrativa, nao
  necessariamente sudo amplo;
- sudo permanente para agentes exige justificativa documentada;
- permissoes temporarias devem ser revogadas ao fim da tarefa.

## Modelo Just-In-Time Privilege

Fluxo oficial:

```text
1. Emerson autoriza.
2. Codex recebe acesso administrativo temporario.
3. Codex prepara completamente o ambiente.
4. Codex remove seus proprios privilegios temporarios.
5. Codex documenta as permissoes concedidas ao Claude.
6. Claude executa a tarefa tecnica.
7. Codex revisa, valida seguranca e consolida documentacao.
8. O ambiente retorna ao estado minimo de privilegios.
```

## Responsabilidades do Codex

Codex atua como controlador de privilegios da plataforma.

Responsabilidades:

- projetar a solucao;
- revisar arquitetura;
- definir padroes;
- preparar o ambiente;
- criar diretorios aprovados;
- definir ownership;
- definir grupos;
- definir ACLs;
- criar politicas sudo quando necessario;
- preparar o ambiente para execucao;
- validar seguranca;
- revogar seus proprios privilegios temporarios;
- documentar permissoes concedidas;
- delegar execucao ao Claude.

Restricoes:

- Codex nao deve executar instalacoes de servicos quando isso puder ser
  delegado ao Claude;
- Codex nao deve manter sudo permanente por conveniencia;
- Codex nao deve ampliar escopo sem autorizacao de Emerson;
- Codex nao deve deixar privilegios temporarios sem revogacao ou justificativa.

## Responsabilidades do Claude

Claude atua como engenheiro de execucao.

Responsabilidades:

- instalar aplicacoes;
- subir containers;
- executar Docker Compose;
- configurar servicos;
- realizar migracoes aprovadas;
- executar procedimentos operacionais.

Restricoes:

- Claude deve usar somente permissoes previamente preparadas pelo Codex;
- Claude nunca deve ampliar seus proprios privilegios;
- Claude nunca deve alterar sudoers por iniciativa propria;
- Claude nunca deve adicionar usuarios a grupos administrativos;
- Claude deve parar se uma acao exigir permissao nao concedida;
- Claude deve registrar evidencias de execucao para revisao do Codex.

## Estado minimo permanente

Estado recomendado apos encerramento de uma tarefa:

```text
emerson       autoridade humana
codex-infra   SSH permitido, sem sudo permanente
claude-infra  SSH permitido, permissoes operacionais minimas
```

Regras:

- excecoes devem ser documentadas na VM e em relatorio operacional;
- permissoes residuais devem ser justificadas por necessidade operacional real;
- permissoes de conveniencia devem ser removidas.

## Preparacao de ambiente

Antes da execucao por Claude, Codex deve preparar:

- diretorios;
- ownership;
- grupos;
- ACLs;
- secrets paths sem conteudo exposto;
- sudoers temporarios ou limitados, quando necessario;
- validacoes de seguranca;
- rollback de permissoes;
- manifest de permissoes concedidas.

## Manifesto de privilegios

Toda tarefa que envolva privilegios temporarios deve possuir um manifesto.

Modelo conceitual:

```yaml
work_id: YYYY-MM-DD-descricao
target_vm: vmid-hostname
authorized_by: emerson
codex_jit: true
claude_execution: true
directories:
  - path: /opt/projects/exemplo
    owner: claude-infra
    group: infra-admin
acl:
  - path: /mnt/nfs/docker/volumes/exemplo
    principal: claude-infra
    permissions: rwx
sudo_for_codex:
  temporary: true
  revoke_after_prepare: true
sudo_for_claude:
  required: false
validation:
  - sudo -n -l
  - stat paths
  - getfacl paths
```

O manifesto nao deve conter senhas, tokens ou chaves privadas.

## Automacao recomendada

Artefatos executaveis devem residir em `infra-runtime`, nao em
`infra-standards`.

Arquitetura proposta:

```text
infra-runtime/
  platform-jit-privilege/
    bin/
      jit-grant-codex
      jit-prepare-task
      jit-revoke-codex
      jit-audit
      jit-validate
    templates/
      sudoers-codex-jit.template
      sudoers-claude-task.template
      acl-policy.template
    plans/
      PLAN_TEMPLATE.yml
```

Nos hosts ou VMs, a automacao pode materializar:

```text
/usr/local/sbin/platform-jit-grant
/usr/local/sbin/platform-jit-revoke
/usr/local/sbin/platform-jit-audit
/etc/sudoers.d/90-platform-jit-codex
/etc/sudoers.d/91-platform-task-claude
/var/lib/platform-jit/
```

Requisitos da automacao:

- ser idempotente;
- validar pre-requisitos antes de alterar estado;
- executar `visudo -cf` antes de instalar sudoers;
- registrar arquivos criados;
- registrar permissoes antes e depois;
- possuir rollback;
- revogar privilegios temporarios ao final;
- nao expor secrets em logs;
- falhar fechado quando houver inconsistencia.

## Sudoers temporario

Quando necessario, sudo para Codex deve ser temporario e limitado.

Regras:

- arquivo temporario deve ficar em `/etc/sudoers.d/`;
- nome deve indicar JIT e tarefa;
- permissao deve ser `0440`;
- validacao obrigatoria com `visudo -cf`;
- revogacao deve ser parte do plano;
- ausencia de revogacao e falha operacional.

Claude nao deve receber permissao para:

- editar `/etc/sudoers`;
- editar `/etc/sudoers.d`;
- modificar grupos administrativos;
- conceder privilegio a si mesmo;
- alterar SSH sem plano aprovado.

## Validacao obrigatoria

Antes de delegar ao Claude, Codex deve validar:

```text
id codex-infra
id claude-infra
sudo -n -l
stat <paths>
getfacl <paths>
docker access quando aplicavel
visudo -cf <sudoers temporario>
```

Apos a execucao, Codex deve validar:

- privilegios temporarios revogados;
- sudo residual justificado ou ausente;
- ownership e ACLs finais;
- servico operacional;
- documentacao atualizada;
- relatorio criado quando aplicavel.

## Documentacao obrigatoria

Toda tarefa com privilegio temporario deve registrar:

- autorizacao de Emerson;
- objetivo;
- escopo;
- privilegios concedidos ao Codex;
- privilegios concedidos ao Claude;
- arquivos sudoers criados;
- ACLs aplicadas;
- diretorios e ownership;
- momento de revogacao;
- validacoes executadas;
- pendencias.

Documentos de destino:

- relatorio operacional em `infra-live-docs` ou workspace local;
- documentacao viva da VM quando estado real mudar;
- `infra-standards` quando uma regra reutilizavel mudar;
- `infra-runtime` quando automacao executavel for criada.

## Referencias

- `standards/AI_AGENT_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
- `standards/SSH_STANDARD.md`
- `standards/VM_STANDARD.md`
- `decision-records/0014-ai-agent-jit-privilege-model.md`
