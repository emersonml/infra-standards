# Application Access Standard

## Objetivo

Padronizar identidades administrativas de aplicacoes com painel Web.

Este padrao separa administracao de aplicacao de administracao Linux, SSH,
banco de dados, hypervisor e tokens de integracao.

## Identidade administrativa primaria

Classificacao:

- Governanca
- Padrao
- Seguranca

Usuario padrao para novos paineis Web:

```text
root-admin
```

Significado:

- `root-admin` e a identidade primaria de administracao da aplicacao.
- `root-admin` nao representa o usuario `root` do Linux.
- `root-admin` nao representa root de banco de dados.
- `root-admin` nao representa conta de SSH.
- `root-admin` nao deve ser usado para integracoes quando a aplicacao permitir
  usuario ou token dedicado.

## Regras

- novas aplicacoes com painel Web devem usar `root-admin` como usuario
  administrativo inicial quando a aplicacao permitir definir esse usuario;
- o usuario `admin` generico deve ser evitado em novas implantacoes;
- usuarios com nome pessoal, como `emerson-admin`, devem ser evitados em novas
  implantacoes;
- contas administrativas de aplicacao devem possuir senha forte e unica;
- senhas, tokens e segredos nao devem ser registrados em documentacao ou Git;
- integracoes entre sistemas devem usar contas ou tokens dedicados sempre que
  disponivel;
- excecoes devem ser registradas na documentacao viva da VM e no relatorio
  operacional da implantacao.

## Separacao de identidades

Identidades oficiais possuem escopos distintos:

```text
emerson       = operador humano Linux/SSH/CLI
codex-infra   = agente Codex
claude-infra  = agente Claude
root-admin    = administrador primario de painel Web de aplicacao
```

Regras:

- `root-admin` nunca deve ser criado como usuario Linux por padrao;
- `root-admin` nunca deve substituir usuarios nominais de operadores ou
  agentes;
- `root-admin` nao deve receber acesso SSH;
- `root-admin` nao deve ser usado como owner de arquivos no sistema
  operacional;
- a documentacao deve informar que se trata de uma conta da aplicacao, nao do
  sistema operacional.

## Handover para clientes

Quando uma aplicacao for entregue a um cliente:

- a senha de `root-admin` deve ser rotacionada no momento da entrega;
- quando o cliente exigir identidade propria, criar um administrador nominal do
  cliente e registrar a decisao;
- apos validacao do acesso do cliente, avaliar manter, desabilitar ou remover
  `root-admin` conforme contrato e modelo operacional aprovado;
- a transferencia nao deve expor segredos em relatorios, commits ou mensagens.

## Estado historico

Ambientes existentes podem conter usuarios administrativos anteriores.

Regra:

- nao reescrever evidencias historicas;
- nao alterar usuario administrativo de aplicacao sem plano, backup e
  aprovacao;
- quando tecnicamente seguro, migrar novas implantacoes para o padrao
  `root-admin`;
- quando a aplicacao nao permitir renomear usuario, criar novo administrador,
  validar acesso e remover ou desabilitar o usuario antigo apenas com
  aprovacao.

## Validacoes minimas

Depois de criar ou migrar o administrador principal de aplicacao:

- validar login Web;
- validar que `root-admin` possui permissao administrativa na aplicacao;
- validar que nenhum segredo foi documentado;
- validar que integracoes nao reutilizam `root-admin` quando houver conta ou
  token dedicado;
- registrar a identidade administrativa saneada na documentacao viva.

## Referencias

```text
decision-records/0016-application-primary-admin-identity.md
standards/SECURITY_STANDARD.md
```
