# Documentation Standard

## Objetivo

Definir o padrao de documentacao tecnica da infraestrutura.

## Regras gerais

- Usar somente Markdown.
- Usar somente caracteres ASCII.
- Escrever de forma tecnica, objetiva e auditavel.
- Nao duplicar documentacao sem necessidade.
- Registrar evidencias quando uma decisao depender do estado real.
- Atualizar a documentacao quando houver mudanca de padrao.

## Separacao de escopo

Repositorio de padroes:

```text
infra-standards/
```

Finalidade:

- padroes globais;
- modelos;
- decisoes reutilizaveis;
- roadmap de padronizacao.

Documentacao viva da VM:

```text
/opt/projects/HOST.md
/opt/projects/.docs/
```

Finalidade:

- estado operacional real da VM;
- inventario local;
- historico local;
- decisoes locais;
- pendencias locais.

## Fluxo obrigatorio

Todo trabalho operacional deve seguir:

```text
Diagnostico
Evidencias
Plano
Execucao
Validacao
Documentacao
Relatorio
```

## Documentos minimos por VM

Toda VM deve possuir:

```text
/opt/projects/HOST.md
/opt/projects/.docs/CHANGELOG.md
/opt/projects/.docs/DECISIONS.md
/opt/projects/.docs/INVENTORY.md
/opt/projects/.docs/NETWORK.md
/opt/projects/.docs/TODO.md
```

## Formato de roadmap

Usar:

```text
[OK ] Item concluido
[TODO] Item pendente
```

## Boas praticas

- Preferir listas simples.
- Evitar tabelas quando uma lista for suficiente.
- Manter comandos em blocos separados.
- Nao registrar segredos.
- Nao registrar chaves privadas.
- Nao registrar senhas.
