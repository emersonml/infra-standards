# WORKSPACE_STANDARD

## Status

Decisao arquitetural aprovada:

```text
GOV-004 - Definicao Oficial dos Workspaces da Plataforma
```

Esta revisao substitui a nomenclatura anterior:

```text
Workspace Institucional -> Platform Workspace
Workspace Operacional   -> Operational Workspace
```

Justificativa:

- a distincao entre os workspaces e funcional;
- `Platform Workspace` representa engenharia, arquitetura, governanca,
  documentacao e automacao da plataforma;
- `Operational Workspace` representa operacao de servicos em VMs especificas;
- o termo "institucional" permanece associado a documentos, repositorios e
  governanca, mas nao define sozinho o tipo de workspace.

## Definicao formal de Workspace

Workspace e o ponto de entrada oficial utilizado por agentes humanos ou
automatizados para compreender, operar ou evoluir um ambiente.

Um workspace deve fornecer contexto suficiente para:

- identificar a funcao da VM ou ambiente;
- localizar documentacao, repositorios e artefatos relevantes;
- separar material temporario de material consolidado;
- orientar a proxima acao operacional ou arquitetural;
- reduzir dependencia de conhecimento implicito.

Workspace nao e armazenamento permanente. Workspace representa contexto
operacional e ponto de orientacao.

## Tipos oficiais de Workspace

Existem tres conceitos oficiais:

1. Platform Workspace.
2. Operational Workspace.
3. Personal Workspace.

## Platform Workspace

Aplicacao:

- VMs de Engenharia;
- exemplos: VM152 e VM154.

Objetivo:

- desenvolvimento da plataforma;
- arquitetura;
- governanca;
- documentacao;
- automacao;
- manutencao dos repositorios institucionais.

Workspace oficial:

```text
/home/emerson/platform
```

Estrutura alvo:

```text
/home/emerson/platform/
 |
 +-- infra-standards/
 |
 +-- infra-runtime/
 |
 +-- infra-live-docs/
 |
 +-- workspace/
 |
 +-- reports/
 |
 +-- inbox/
 |
 +-- scratch/
 |
 +-- README.md
```

## Operational Workspace

Aplicacao:

- VMs de Servico;
- exemplos: VM230, VM240, VM241 e VM130.

Objetivo:

- operar o servico instalado naquela VM;
- manter arquivos operacionais da aplicacao;
- preservar documentacao viva local quando aplicavel;
- organizar compose, configuracoes, scripts e evidencias operacionais do
  servico.

Workspace oficial:

```text
/opt/projects
```

Exemplos:

```text
/opt/projects/nextcloud
/opt/projects/chatwoot
```

## Personal Workspace

Aplicacao:

- diretorios pessoais do usuario;
- areas locais que nao fazem parte da plataforma.

Exemplos:

```text
~/Downloads
~/Desktop
~/Documents
```

Regras:

- Personal Workspace nao faz parte da arquitetura da plataforma;
- nao deve armazenar documentacao institucional permanente;
- nao deve ser destino de relatorios oficiais;
- nao deve hospedar repositorios institucionais como padrao;
- pode conter arquivos pessoais, downloads, rascunhos nao classificados e
  material externo antes de triagem.

## README.md obrigatorio

Todo Workspace deve possuir obrigatoriamente um arquivo:

```text
README.md
```

Finalidade:

- servir como ponto oficial de onboarding para qualquer agente;
- identificar a funcao do ambiente;
- indicar o tipo de workspace;
- explicar a estrutura local;
- apontar documentos e repositorios relevantes;
- registrar regras resumidas de uso;
- indicar onde ficam material temporario, relatorios, entradas e descartes;
- reduzir ambiguidade antes de qualquer acao.

Regra:

- agentes devem iniciar a leitura pelo `README.md` do workspace quando ele
  existir;
- se o `README.md` nao existir, a ausencia deve ser registrada como lacuna de
  governanca antes de qualquer mudanca estrutural.

## Principios

### P1 - Todo agente inicia pelo Workspace

O workspace e a primeira camada de contexto. Antes de operar ou evoluir um
ambiente, o agente deve identificar o workspace aplicavel e ler seu
`README.md`.

### P2 - Todo documento nasce temporario

Todo documento produzido durante uma atividade nasce como rascunho, evidencia
ou relatorio transitorio. Ele so se torna oficial apos classificacao e
consolidacao.

### P3 - Somente documentos consolidados tornam-se oficiais

Documentos oficiais devem estar no repositorio ou local aprovado para sua
classe. Para documentacao viva e relatorios oficiais, o destino institucional e
`infra-live-docs`.

### P4 - Platform Workspace e Operational Workspace possuem responsabilidades distintas

Platform Workspace concentra evolucao da plataforma. Operational Workspace
concentra operacao de servicos. Misturar essas responsabilidades aumenta risco
de duplicidade, perda de contexto e alteracoes fora de escopo.

### P5 - Workspace nao e armazenamento permanente

Workspace representa contexto operacional. Ele pode conter areas transitorias,
mas nao substitui repositorios oficiais nem armazenamento definitivo.

### P6 - Personal Workspace nao e fonte oficial

Diretorios pessoais podem receber arquivos e rascunhos, mas nao devem ser
usados como fonte permanente de documentacao institucional.

### P7 - Caminhos oficiais devem ser explicitos

Todo padrao deve declarar caminhos oficiais, excecoes conhecidas e destino de
consolidacao. Caminhos implicitos nao devem orientar automacao ou governanca.

## Camada oficial da arquitetura

Workspace passa a ser uma camada oficial da arquitetura da plataforma.

Arquitetura conceitual:

```text
Infraestrutura
 |
 +-- Workspace
      |
      +-- Servicos
```

Responsabilidades:

- Infraestrutura: fornece VM, rede, armazenamento, sistema operacional,
  identidade, permissoes e capacidade de execucao.
- Workspace: fornece contexto operacional, ponto de entrada, organizacao local,
  documentacao de onboarding e separacao entre material temporario e oficial.
- Servicos: executam a carga funcional, por exemplo Nextcloud, Chatwoot,
  observabilidade, bancos, filas e componentes auxiliares.

Regra arquitetural:

- agentes nao devem saltar diretamente de Infraestrutura para Servicos sem
  avaliar o Workspace aplicavel;
- o Workspace traduz o estado da infraestrutura em contexto operacional
  utilizavel.

## Ciclo de vida dos documentos

Fluxo conceitual:

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

Interpretacao:

- `scratch`: experimentos e material descartavel;
- `workspace`: rascunhos e trabalho temporario em andamento;
- `reports`: saidas documentais transitorias de atividades;
- `infra-live-docs`: consolidacao oficial de documentacao viva e relatorios.

Nem todo item precisa passar por todas as etapas. Material descartavel pode ser
eliminado apos triagem. Material oficial deve terminar em destino consolidado.

## Diferencas entre os workspaces

Platform Workspace:

- pertence as VMs de Engenharia;
- concentra repositorios institucionais;
- concentra governanca, arquitetura, automacao e documentacao da plataforma;
- usa `/home/emerson/platform` como raiz;
- contem areas auxiliares de trabalho, entrada, relatorios e descarte.

Operational Workspace:

- pertence as VMs de Servico;
- concentra operacao de um servico especifico;
- usa `/opt/projects` como raiz;
- nao deve hospedar a governanca central da plataforma;
- deve evitar misturar repositorios institucionais com operacao local.

Personal Workspace:

- pertence ao usuario;
- nao pertence a arquitetura da plataforma;
- nao deve ser destino oficial de documentacao;
- pode receber arquivos antes de triagem.

## Quando utilizar cada estrutura

Usar Platform Workspace quando a atividade envolver:

- evolucao de padroes;
- desenho arquitetural;
- decisao de governanca;
- consolidacao documental;
- automacao reutilizavel;
- manutencao dos repositorios `infra-standards`, `infra-runtime` e
  `infra-live-docs`;
- relatorios oficiais que devem ser preservados institucionalmente.

Usar Operational Workspace quando a atividade envolver:

- operacao de um servico em uma VM especifica;
- compose e configuracoes de servico;
- documentacao viva local da VM;
- diagnosticos e evidencias operacionais;
- scripts locais de manutencao do servico;
- arquivos necessarios para funcionamento ou suporte da aplicacao instalada.

Usar Personal Workspace somente para:

- arquivos pessoais;
- downloads;
- entrada nao classificada;
- rascunhos sem compromisso institucional;
- material que ainda precisa ser movido para um workspace oficial ou descartado.

## Estrutura oficial de /home/emerson/platform

### infra-standards

Repositorio institucional de padroes.

Finalidade:

- politicas;
- normas;
- templates;
- arquitetura de referencia;
- decisoes aprovadas;
- regras reutilizaveis entre VMs.

### infra-runtime

Repositorio institucional de artefatos executaveis ou reutilizaveis.

Finalidade:

- scripts;
- modelos operacionais;
- Policy Broker e politicas versionadas;
- sudoers versionados quando inevitaveis;
- automacoes;
- procedimentos tecnicos reutilizaveis.

### infra-live-docs

Repositorio institucional de documentacao viva.

Finalidade:

- inventarios;
- estado real observado;
- relatorios oficiais;
- historico operacional aprovado;
- registros consolidados por VM, servico ou componente.

### workspace

Area de trabalho temporaria.

Finalidade:

- rascunhos de execucao;
- preparacao de conteudo;
- diagnosticos intermediarios;
- arquivos ainda nao classificados.

Regra:

- nenhum documento oficial deve permanecer indefinidamente em `workspace`;
- conteudo aprovado deve migrar para o repositorio institucional correto.

### reports

Area local transitoria de relatorios.

Finalidade:

- receber relatorios gerados durante atividades;
- servir como fila local antes de consolidacao oficial;
- preservar evidencias ate que sejam movidas ou copiadas para
  `infra-live-docs`.

Regra:

- relatorios oficiais pertencem ao `infra-live-docs`;
- `reports` local nao e arquivo definitivo;
- relatorios em `reports` devem ser revisados e consolidados.

### inbox

Area de entrada.

Finalidade:

- receber arquivos externos;
- receber material pendente de classificacao;
- receber insumos antes de triagem.

Regra:

- nada em `inbox` deve ser considerado oficial sem triagem;
- conteudo deve ser classificado, movido para destino correto ou descartado.

### scratch

Area descartavel.

Finalidade:

- testes rapidos;
- arquivos temporarios;
- experimentos;
- material sem valor permanente.

Regra:

- `scratch` pode ser descartado;
- nao armazenar documentos oficiais;
- nao armazenar segredos;
- nao armazenar dados necessarios para operacao.

### README.md

Documento de entrada do workspace.

Finalidade:

- identificar a VM;
- declarar o tipo de workspace;
- explicar a estrutura local;
- indicar os repositorios institucionais;
- registrar regras resumidas de uso;
- apontar para este padrao.

## Regras oficiais

1. Platform Workspace usa `/home/emerson/platform`.
2. Operational Workspace usa `/opt/projects`.
3. Personal Workspace nao e armazenamento oficial da plataforma.
4. Todo Workspace deve possuir `README.md`.
5. Todo agente inicia pelo Workspace e por seu `README.md`.
6. Repositorios institucionais pertencem ao Platform Workspace.
7. Relatorios oficiais pertencem ao `infra-live-docs`.
8. `reports` local e transitorio.
9. `workspace` contem somente trabalho temporario ou nao consolidado.
10. `inbox` e area de entrada e triagem.
11. `scratch` pode ser descartado.
12. Nenhum documento oficial deve permanecer indefinidamente em `workspace`.
13. Nenhum documento oficial deve depender de `scratch`.
14. Excecoes permanentes devem ser documentadas.
15. Migracoes devem ser planejadas, validadas e reversiveis.

## Impactos conhecidos

Documentos e padroes que precisarao de revisao futura:

- relatorios que apontam `/opt/projects` como padrao unico da VM152;
- relatorio GOV-003 de auditoria de repositorios da VM152;
- documentos que recomendam criar `HOST.md` ou `.docs` diretamente em
  `/opt/projects` para VMs de Engenharia;
- documentacao de `infra-standards` que trate layout base de VMs;
- documentacao de `infra-live-docs` sobre local oficial de relatorios;
- instrucoes de agentes que assumam `/opt/projects` como workspace universal;
- planos de migracao ou auditoria baseados no padrao antigo;
- templates de bootstrap que diferenciem pouco VMs de Engenharia e VMs de
  Servico;
- documentos que ainda usem os nomes `Workspace Institucional` e
  `Workspace Operacional`.

## Recomendacao arquitetural

Recomenda-se criar futuramente uma ADR especifica:

```text
ADR - Platform Workspace vs Operational Workspace
```

Objetivo da ADR:

- registrar o contexto da decisao;
- explicar por que a distincao e funcional;
- declarar alternativas consideradas;
- registrar consequencias para VMs de Engenharia e VMs de Servico;
- formalizar criterios de migracao e excecao.

Esta ADR nao deve ser criada nesta atividade.

## Plano de migracao nao executado

### 1. Preparacao

1. Confirmar lista de VMs de Engenharia.
2. Confirmar lista de VMs de Servico.
3. Levantar repositorios existentes.
4. Levantar arquivos locais em `/home/emerson`.
5. Levantar arquivos locais em `/opt/projects`.
6. Identificar documentos oficiais, transitorios e descartaveis.
7. Definir janela de migracao.
8. Comunicar impacto.

### 2. Backup

1. Gerar snapshot da VM quando aplicavel.
2. Criar backup dos diretorios envolvidos.
3. Registrar checksums dos arquivos criticos.
4. Registrar estado Git de cada repositorio.
5. Registrar permissoes e ownership.

### 3. Validacoes pre-migracao

1. Validar espaco em disco.
2. Validar permissoes do usuario `emerson`.
3. Validar que nao ha processos usando caminhos antigos de forma critica.
4. Validar remotes, branches e status dos repositorios.
5. Validar que nao ha arquivos oficiais sem classificacao.
6. Validar existencia ou lacuna de `README.md` nos workspaces envolvidos.

### 4. Movimentacao dos repositorios

1. Criar `/home/emerson/platform`.
2. Criar estrutura alvo.
3. Criar `README.md` do Platform Workspace.
4. Mover `infra-standards` para `/home/emerson/platform/infra-standards`.
5. Mover `infra-runtime` para `/home/emerson/platform/infra-runtime`.
6. Mover `infra-live-docs` para `/home/emerson/platform/infra-live-docs`.
7. Preservar ownership, permissoes e conteudo.

### 5. Atualizacao dos caminhos

1. Atualizar documentacao local.
2. Atualizar referencias em relatorios consolidados quando aprovado.
3. Atualizar instrucoes de agentes.
4. Atualizar scripts que dependam de caminhos absolutos.
5. Atualizar READMEs de workspace.
6. Substituir nomenclatura antiga por Platform Workspace e Operational
   Workspace.

### 6. Validacoes pos-migracao

1. Validar existencia da estrutura alvo.
2. Validar `git status` dos repositorios.
3. Validar remotes e branches.
4. Validar leitura dos documentos oficiais.
5. Validar que relatorios oficiais estao no destino correto.
6. Validar que nao restaram duplicatas institucionais nao documentadas.
7. Validar `README.md` do workspace.

### 7. Rollback

1. Parar novas escritas durante rollback.
2. Restaurar diretorios a partir do backup ou snapshot.
3. Restaurar ownership e permissoes.
4. Restaurar caminhos antigos temporariamente.
5. Validar repositorios.
6. Registrar causa do rollback.
7. Reabrir plano de migracao corrigido.
