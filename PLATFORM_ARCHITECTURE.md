# PLATFORM_ARCHITECTURE

## Status

Documento mestre aprovado para orientar a leitura arquitetural da plataforma.

Este documento e um indice arquitetural. Ele conecta padroes existentes e nao
substitui os documentos de referencia.

## Capitulo 1 - Visao Geral da Plataforma

### Proposito

A plataforma existe para organizar a evolucao, operacao e governanca da
infraestrutura de forma auditavel, documentada e reutilizavel.

Ela conecta:

- padroes de infraestrutura;
- workspaces oficiais;
- artefatos de runtime;
- documentacao viva;
- governanca de decisoes;
- operacao de servicos.

### Objetivos

1. Reduzir ambiguidade operacional.
2. Separar engenharia da plataforma e operacao de servicos.
3. Manter decisoes rastreaveis.
4. Transformar conhecimento operacional em documentacao viva.
5. Evitar que padroes sejam criados sem governanca.
6. Permitir que agentes humanos e agentes de IA encontrem o contexto correto
   antes de agir.

### Principios gerais

- Documentacao antes da implementacao.
- Evidencia antes de conclusao.
- Padrao antes de automacao permanente.
- Runtime segue contratos e padroes.
- Relatorios oficiais pertencem a documentacao viva.
- Workspaces sao contexto operacional, nao armazenamento permanente.
- Mudancas estruturais exigem plano, validacao e rollback.

### Filosofia de documentacao viva

Documentacao viva registra o estado real observado, as decisoes aprovadas e as
evidencias relevantes para operacao futura.

Ela nao e apenas historico. Ela e parte da infraestrutura porque orienta:

- diagnostico;
- manutencao;
- migracao;
- revisao;
- onboarding;
- recuperacao.

### Arquitetura baseada em padroes

A plataforma evita decisoes isoladas. Uma regra duradoura deve nascer de
governanca, ser registrada, virar padrao quando aprovada e depois orientar
templates, runtime, operacao e documentacao viva.

Fluxo conceitual:

```text
Decisao
 |
 +-- Padrao
      |
      +-- Template
           |
           +-- Runtime
                |
                +-- Live Docs
```

## Capitulo 2 - Camadas da Plataforma

Representacao:

```text
Infraestrutura
 |
 v
Workspace
 |
 v
Servicos
 |
 v
Operacao
```

### Infraestrutura

Responsabilidade:

- fornecer VM, rede, armazenamento, sistema operacional, identidade,
  permissoes e capacidade de execucao;
- manter os limites tecnicos onde os servicos operam;
- preservar conectividade, persistencia, seguranca e recursos base.

### Workspace

Responsabilidade:

- ser o ponto de entrada oficial do ambiente;
- declarar contexto, finalidade e estrutura local;
- separar material temporario de material consolidado;
- orientar agentes antes de qualquer acao;
- conectar infraestrutura, padroes, runtime e documentacao viva.

### Servicos

Responsabilidade:

- executar a carga funcional da VM ou ambiente;
- materializar aplicacoes, bancos, filas, proxy local, monitoramento e demais
  componentes;
- seguir os padroes oficiais aplicaveis.

### Operacao

Responsabilidade:

- diagnosticar, validar, manter e evoluir servicos;
- registrar evidencias e relatorios;
- alimentar `infra-live-docs` com estado real;
- acionar governanca quando uma decisao local tiver impacto permanente.

## Capitulo 3 - Padroes oficiais existentes

Esta secao e um indice. Para detalhes, consultar o documento de referencia.

### Identity Standard

Objetivo:

- definir identidades humanas, identidades de agentes, grupos e modelo de
  acesso administrativo.

Responsabilidade:

- orientar usuarios, grupos, agentes, ownership, permissoes e revisoes de
  acesso.

Documento de referencia:

```text
/home/emerson/platform/infra-standards/ARCHITECTURE.md
/home/emerson/platform/infra-standards/standards/AI_AGENT_STANDARD.md
/home/emerson/platform/reports/REL-2026-07-02-002-gov-002-revisao-identidades.md
```

### Workspace Standard

Objetivo:

- definir Platform Workspace, Operational Workspace e Personal Workspace.

Responsabilidade:

- declarar pontos oficiais de entrada, regras de README, ciclo de documentos e
  separacao entre plataforma, operacao e areas pessoais.

Documento de referencia:

```text
/home/emerson/platform/infra-standards/WORKSPACE_STANDARD.md
```

### Security Standard

Objetivo:

- definir principios e limites de seguranca aplicaveis a infraestrutura.

Responsabilidade:

- orientar acesso, exposicao, validacao, recuperabilidade e protecao
  operacional.

Documento de referencia:

```text
/home/emerson/platform/infra-standards/standards/SECURITY_STANDARD.md
```

### Network Standard

Objetivo:

- definir padrao de rede, enderecamento e politica IPv4.

Responsabilidade:

- orientar conectividade, exposicao de servicos, SSH, LAN, proxy e limites de
  publicacao.

Documento de referencia:

```text
/home/emerson/platform/infra-standards/standards/NETWORK_STANDARD.md
```

### Runtime Standard

Objetivo:

- organizar artefatos reutilizaveis que executam ou suportam operacoes.

Responsabilidade:

- hospedar scripts, modelos, sudoers versionados, validadores e automacoes que
  seguem padroes aprovados.

Documento de referencia:

```text
/home/emerson/platform/infra-runtime/README.md
/home/emerson/platform/infra-runtime/sudoers/README.md
```

### Documentation Standard

Objetivo:

- definir como documentacao, relatorios, templates, ADRs e estado vivo devem
  ser organizados.

Responsabilidade:

- separar documentacao normativa, documentacao operacional e documentacao viva.

Documento de referencia:

```text
/home/emerson/platform/infra-standards/standards/DOCUMENTATION_STANDARD.md
/home/emerson/platform/infra-live-docs/README.md
```

### Git Governance

Objetivo:

- definir quando e por que usar Git em mudancas institucionais.

Responsabilidade:

- separar commit, push, revisao, promocao de documentos e preservacao de
  historico.

Documento de referencia:

```text
/home/emerson/platform/infra-standards/standards/GIT_STANDARD.md
/home/emerson/platform/reports/REL-2026-07-02-005-gov-002-proposta-git-governance.md
```

### Governance

Objetivo:

- definir como ideias, discussoes, decisoes, ADRs, padroes e implementacoes
  evoluem.

Responsabilidade:

- impedir que praticas permanentes nascam diretamente de execucao local;
- exigir rastreabilidade entre decisao, padrao, runtime e documentacao viva.

Documento de referencia:

```text
/home/emerson/platform/infra-standards/README.md
/home/emerson/platform/infra-standards/PHILOSOPHY.md
/home/emerson/platform/infra-standards/requests/README.md
```

## Capitulo 4 - Tipos de Workspace

### Platform Workspace

Usado por VMs de Engenharia.

Raiz oficial:

```text
/home/emerson/platform
```

Responsabilidade:

- evoluir a plataforma;
- hospedar repositorios institucionais;
- organizar arquitetura, governanca, automacao e documentacao.

### Operational Workspace

Usado por VMs de Servico.

Raiz oficial:

```text
/opt/projects
```

Responsabilidade:

- operar servicos instalados na VM;
- manter configuracoes, compose, evidencias e documentacao operacional local.

### Personal Workspace

Usado por diretorios pessoais do usuario.

Exemplos:

```text
~/Downloads
~/Desktop
~/Documents
```

Responsabilidade:

- receber arquivos pessoais, downloads e rascunhos nao classificados;
- nao armazenar documentacao institucional permanente.

### Relacao entre eles

```text
Platform Workspace
 |
 +-- define e evolui padroes
 |
 +-- orienta Operational Workspace

Operational Workspace
 |
 +-- executa servicos
 |
 +-- produz evidencias e relatorios

Personal Workspace
 |
 +-- recebe material pessoal ou nao classificado
 |
 +-- nao e fonte oficial
```

## Capitulo 5 - Fluxo de vida dos documentos

Representacao:

```text
scratch
   |
   v
workspace
   |
   v
reports
   |
   v
infra-live-docs
```

### scratch

Area descartavel para testes, experimentos e material sem valor permanente.

### workspace

Area temporaria de trabalho. Pode conter rascunhos, diagnosticos
intermediarios e preparacao de conteudo.

### reports

Area transitoria de saida documental. Recebe relatorios ate classificacao e
consolidacao.

### infra-live-docs

Destino oficial para documentacao viva, inventarios, estado real e relatorios
consolidados.

Regra:

- todo documento nasce temporario;
- somente documento consolidado se torna oficial;
- documento oficial nao deve permanecer indefinidamente em workspace local.

## Capitulo 6 - Fluxo de onboarding de agentes

Representacao:

```text
Novo Agente
 |
 v
README do Platform Workspace
 |
 v
PLATFORM_ARCHITECTURE
 |
 v
Standards
 |
 v
Runtime
 |
 v
Live Docs
 |
 v
Pronto para operar
```

### Objetivo

O fluxo garante que o agente:

- entenda o ambiente antes de agir;
- leia o mapa geral antes dos detalhes;
- conheca os padroes aplicaveis;
- identifique artefatos de runtime aprovados;
- consulte estado real em documentacao viva;
- evite executar por hipotese.

## Capitulo 7 - Governanca

Decisoes evoluem por etapas.

Representacao:

```text
Discussao
 |
 v
GOV
 |
 v
ADR
 |
 v
Standard
 |
 v
Implementacao
 |
 v
Live Docs
```

### Discussao

Explora problema, contexto, alternativas e riscos.

### GOV

Formaliza uma decisao ou direcao arquitetural ainda em consolidacao.

### ADR

Registra decisao aprovada, contexto, alternativas e consequencias.

### Standard

Transforma decisao em regra reutilizavel.

### Implementacao

Aplica a regra em workspace, runtime, servico, template ou operacao.

### Live Docs

Registra estado real, evidencias, inventario e historico operacional.

## Capitulo 8 - Roadmap Arquitetural

### Arquitetura concluida

- Separacao conceitual entre padroes, runtime e documentacao viva.
- Repositorios institucionais identificados:
  - `infra-standards`;
  - `infra-runtime`;
  - `infra-live-docs`.
- Workspace definido como camada oficial.
- Platform Workspace, Operational Workspace e Personal Workspace definidos.
- Documentacao viva reconhecida como destino oficial de estado real.
- Principio do Privilegio Temporario registrado no `infra-runtime`.

### Arquitetura em evolucao

- Promocao do Workspace Standard para `infra-standards`.
- Criacao de ADR para Platform Workspace vs Operational Workspace.
- Consolidacao de Git Governance.
- Consolidacao de Identity Governance e Identity Standard.
- Ajuste das referencias antigas que ainda apontam `/opt/projects` como
  padrao unico.
- Consolidacao dos relatorios oficiais no `infra-live-docs`.

### Proximos grandes marcos

1. Promover `WORKSPACE_STANDARD.md` por processo de governanca.
2. Criar ADR do Workspace Standard.
3. Criar ou consolidar `PLATFORM_ARCHITECTURE.md` no repositorio apropriado.
4. Definir plano de migracao para `/home/emerson/platform`.
5. Consolidar governancas P0:
   - Change Governance;
   - Git Governance;
   - Documentation Governance;
   - Identity Governance.
6. Criar indice oficial de relatorios no `infra-live-docs`.

## Capitulo 9 - Dependencias entre os documentos

Diagrama conceitual:

```text
PHILOSOPHY
 |
 v
PLATFORM_ARCHITECTURE
 |
 +-- WORKSPACE_STANDARD
 |
 +-- ARCHITECTURE
 |    |
 |    +-- VM_STANDARD
 |    +-- NETWORK_STANDARD
 |    +-- SECURITY_STANDARD
 |
 +-- DOCUMENTATION_STANDARD
 |    |
 |    +-- infra-live-docs
 |    +-- reports
 |
 +-- GOVERNANCE
 |    |
 |    +-- ADR
 |    +-- Standards
 |    +-- Git Governance
 |
 +-- RUNTIME
      |
      +-- infra-runtime
      +-- scripts
      +-- sudoers
```

Leitura:

- `PHILOSOPHY` orienta principios.
- `PLATFORM_ARCHITECTURE` conecta as camadas.
- Standards definem regras reutilizaveis.
- Governance define como regras nascem e mudam.
- Runtime materializa artefatos reutilizaveis.
- `infra-live-docs` registra estado real e relatorios consolidados.

## Capitulo 10 - Recomendacoes

Oportunidades futuras:

1. Promover este documento ao repositorio apropriado apos revisao humana.
2. Criar ADR para `Platform Workspace vs Operational Workspace`.
3. Criar ADR para `PLATFORM_ARCHITECTURE` como indice mestre.
4. Consolidar Identity Standard em documento proprio.
5. Consolidar Git Governance em documento proprio.
6. Criar indice oficial de relatorios em `infra-live-docs`.
7. Revisar documentos que ainda tratam `/opt/projects` como padrao unico.
8. Definir criterio para quando um relatorio local deve migrar para
   `infra-live-docs`.
9. Criar checklist de onboarding para agentes.
10. Criar mapa de maturidade dos padroes:
    - proposto;
    - aprovado;
    - promovido;
    - implementado;
    - validado.

## Observacao final

Este deve ser o primeiro documento lido por um arquiteto humano ou por um
agente de IA depois do `README.md` do workspace.

Ele e um mapa geral da plataforma. Detalhes tecnicos pertencem aos padroes,
ADRs, runtime e documentacao viva correspondentes.

