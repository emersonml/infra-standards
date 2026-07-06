# Privilege Governance Standard

## Objetivo

Definir o modelo oficial de governanca de privilegios para operadores humanos
e agentes de IA.

Este padrao formaliza:

- principio de menor privilegio;
- privilegio Just-In-Time;
- permissoes associadas a funcoes da plataforma, nao a usuarios individuais;
- separacao entre colaboracao, arquitetura, engenharia, auditoria e autoridade;
- Policy Broker como camada oficial para materializar privilegios temporarios;
- Codex como controlador de governanca e preparacao;
- Claude como executor tecnico sem autoelevacao.

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
- colaboracao em arquivos nao implica autoridade administrativa;
- grupos de papel indicam elegibilidade funcional, nao sudo amplo;
- permissoes temporarias devem ser revogadas ao fim da tarefa.

## Modelo de papeis funcionais

Permissoes pertencem as funcoes da plataforma. Usuarios apenas ocupam papeis.

Grupos oficiais:

| Grupo | Finalidade | Autoridade administrativa |
| --- | --- | --- |
| `_ssh` | Login SSH | Nao |
| `infra-collab` | Ownership colaborativo, documentacao, projetos e arquivos operacionais | Nao |
| `infra-architect` | Elegibilidade do Codex para arquitetura, preparacao, ACLs, bootstrap e governanca | Nao diretamente |
| `infra-engineer` | Elegibilidade do Claude para execucao tecnica, Docker, Compose, instalacoes, migracoes e operacao | Nao diretamente |
| `infra-audit` | Leitura e revisao futura de arquitetura, seguranca, documentacao e permissoes | Nao |
| `infra-admin` | Legado historico | Nao usar como novo padrao de autoridade |

Regras:

- `infra-collab` substitui o uso semantico de `infra-admin` para colaboracao.
- `infra-admin` nao deve ser usado como grupo padrao de autoridade em novas
  VMs.
- ambientes historicos podem manter `infra-admin` temporariamente ate migracao
  documentada.
- nenhum grupo de papel concede sudo irrestrito por si so.
- privilegio administrativo so pode ser materializado por politica aprovada.

## Policy Broker

O Policy Broker e a camada oficial que materializa privilegios temporarios.

Responsabilidades:

- receber uma solicitacao aprovada por Emerson;
- validar usuario, papel, alvo, escopo, TTL e politica permitida;
- aplicar somente acoes previstas em manifestos oficiais;
- conceder ACLs, permissoes operacionais ou sudo limitado quando necessario;
- registrar auditoria antes, durante e depois;
- revogar privilegios temporarios;
- falhar fechado quando houver inconsistencia.

Regras:

- agentes nao editam diretamente `/etc/sudoers`;
- agentes nao editam diretamente `/etc/sudoers.d`;
- Codex usa o Policy Broker para preparar o ambiente;
- Claude usa apenas permissoes preparadas;
- Claude nunca amplia seus proprios privilegios;
- scripts soltos `grant-*` nao devem existir sem mediacao do Policy Broker;
- artefatos executaveis do broker pertencem ao `infra-runtime`;
- este documento define o contrato, nao a implementacao executavel.

Local futuro recomendado:

```text
/usr/local/sbin/infra-policy
/etc/infra-policy/policies.d/
/var/lib/infra-policy/
/var/log/infra-policy/
```

Artefatos versionados futuros:

```text
infra-runtime/
  infra-policy/
    bin/
      infra-policy
    policies/
      codex-prepare-directory.yml
      codex-prepare-docker-project.yml
      claude-run-compose.yml
      claude-service-maintenance.yml
    manifests/
      TASK_MANIFEST_TEMPLATE.yml
    validators/
      validate-policy
      validate-state
```

## Modelo Just-In-Time Privilege

Fluxo oficial:

```text
1. Emerson autoriza.
2. Codex solicita ao Policy Broker uma janela temporaria de preparacao.
3. O Policy Broker materializa somente as permissoes aprovadas.
4. Codex prepara ambiente, diretorios, ownership, grupos, ACLs e politicas.
5. Codex revoga seus proprios privilegios temporarios via Policy Broker.
6. Codex documenta as permissoes concedidas ao Claude.
7. Claude executa a tarefa tecnica dentro do escopo preparado.
8. O Policy Broker revoga permissoes temporarias do Claude.
9. Codex revisa, valida seguranca e consolida documentacao.
10. O ambiente retorna ao estado minimo de privilegios.
```

## Responsabilidades do Codex

Codex atua como controlador de governanca e preparacao.

Responsabilidades:

- projetar a solucao;
- revisar arquitetura;
- definir padroes;
- preparar o ambiente;
- definir ownership;
- definir grupos funcionais;
- definir ACLs;
- solicitar privilegios temporarios ao Policy Broker;
- validar seguranca;
- revogar seus proprios privilegios temporarios;
- documentar permissoes concedidas;
- delegar execucao ao Claude.

Restricoes:

- Codex nao deve executar instalacoes de servicos quando isso puder ser
  delegado ao Claude;
- Codex nao deve manter sudo permanente por conveniencia;
- Codex nao deve editar `/etc/sudoers` ou `/etc/sudoers.d` diretamente;
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
- Claude nunca deve alterar sudoers;
- Claude nunca deve adicionar usuarios a grupos administrativos;
- Claude deve parar se uma acao exigir permissao nao concedida;
- Claude deve registrar evidencias de execucao para revisao do Codex.

## Estado minimo permanente

Estado recomendado apos encerramento de uma tarefa:

```text
emerson       autoridade humana
codex-infra   _ssh, infra-collab, infra-architect; sem sudo permanente
claude-infra  _ssh, infra-collab, infra-engineer; permissoes operacionais minimas
```

Regras:

- excecoes devem ser documentadas na VM e em relatorio operacional;
- permissoes residuais devem ser justificadas por necessidade operacional real;
- permissoes de conveniencia devem ser removidas.

## Preparacao de ambiente

Antes da execucao por Claude, Codex deve preparar:

- diretorios;
- ownership;
- grupos funcionais;
- ACLs;
- paths de secrets sem conteudo exposto;
- permissoes temporarias solicitadas ao Policy Broker quando necessario;
- validacoes de seguranca;
- rollback de permissoes;
- manifesto de permissoes concedidas.

## Manifesto de privilegios

Toda tarefa que envolva privilegios temporarios deve possuir um manifesto.

Modelo conceitual:

```yaml
work_id: YYYY-MM-DD-descricao
target_vm: vmid-hostname
authorized_by: emerson
policy_broker: true
codex_role: infra-architect
claude_role: infra-engineer
ttl: 2h
directories:
  - path: /opt/projects/exemplo
    owner: claude-infra
    group: infra-collab
acl:
  - path: /mnt/nfs/docker/volumes/exemplo
    principal: claude-infra
    permissions: rwx
broker_actions:
  - codex-prepare-docker-project
  - claude-run-compose
validation:
  - id codex-infra
  - id claude-infra
  - stat paths
  - getfacl paths
```

O manifesto nao deve conter senhas, tokens ou chaves privadas.

## Estrategia de sudo

Sudo nao e a interface primaria de governanca dos agentes.

Regras:

- quando inevitavel, sudo para agentes deve ser limitado ao comando do Policy
  Broker ou a acoes explicitamente aprovadas;
- sudo amplo permanente para agentes e proibido por padrao;
- Codex nao escreve regras de sudo diretamente;
- Claude nao escreve regras de sudo diretamente;
- templates sudoers versionados podem existir em `infra-runtime`, mas devem
  ser instalados apenas por fluxo aprovado e validado;
- `visudo -cf` e obrigatorio para qualquer politica que materialize sudoers;
- regras transicionais baseadas em sudoers temporario devem migrar para o
  Policy Broker.

## Estrategia de ACL

ACL deve ser preferida a sudo sempre que resolver o problema com menor risco.

Regras:

- ACL deve ser limitada por path;
- ACL deve ser limitada por usuario ou grupo funcional;
- ACL temporaria deve possuir TTL ou revogacao planejada;
- ACL permanente exige justificativa operacional;
- ACL aplicada para Claude deve ser documentada antes da execucao;
- ACL residual deve ser validada por Codex ao final da tarefa.

## Estrategia SSH

SSH autentica identidade. SSH nao concede autoridade administrativa.

Regras:

- `_ssh` permite login;
- chaves privadas nao devem ser compartilhadas entre agentes;
- aliases SSH devem usar identidade explicita quando houver mais de uma chave;
- acesso via Bastion deve preservar usuario nominal;
- acesso SSH de agente nao implica sudo, Docker, ACL ou permissao de escrita.

## Inclusao futura de agentes

Novos agentes devem receber:

- usuario nominal proprio;
- chave SSH propria;
- grupo de papel funcional;
- permissao minima;
- politica de broker especifica;
- escopo documentado;
- auditoria de execucao.

Nenhum novo agente deve herdar permissoes de Codex ou Claude por conveniencia.

## Validacao obrigatoria

Antes de delegar ao Claude, Codex deve validar:

```text
id codex-infra
id claude-infra
stat <paths>
getfacl <paths>
docker access quando aplicavel
infra-policy audit quando disponivel
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
- papel funcional usado por Codex;
- papel funcional usado por Claude;
- acoes do Policy Broker;
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
- `decision-records/0015-role-based-agent-privilege-model.md`
