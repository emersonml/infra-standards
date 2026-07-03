# AI Agent Standard

## Objective

Define how AI agents participate in infrastructure work.

This standard separates the human operator from assisted execution by agents.

## Classification

- Arquitetura
- Governanca
- Padrao
- Procedimento
- Boas praticas

## Roles

Human operator:

- `emerson`

AI agent users:

- `codex-infra`
- `claude-infra`

Standard groups:

- `infra-admin`
- `_ssh`

## Authority Model

Rules:

- Emerson remains the final authority for infrastructure changes.
- AI agents execute diagnostics, documentation, approved procedures and
  validation.
- AI agents must not expand scope without authorization.
- AI agents must not alter production infrastructure without an approved plan.
- AI agents must not change scripts, bootstrap or automation when the task is
  documentation-only.

## Access Model

Rules:

- each AI agent uses a nominal user;
- `_ssh` controls SSH login;
- `infra-admin` identifies administrative authorization;
- sudo for agents must be documented;
- temporary sudo for bootstrap must be removed, reduced or justified after use;
- private keys must not be shared between human and agent users.

## Operating Flow

Required flow:

```text
Context > Diagnosis > Evidence > Plan > Authorization > Execution > Validation > Documentation > Report
```

Rules:

- read relevant context before commands;
- read `AGENTS.md` or local VM instructions when present;
- prefer standards and templates already present in `infra-standards`;
- update local VM documentation when operational state changes;
- update `infra-standards` only when a reusable rule or permanent decision
  changes.

## Repeated Attempt Policy

An agent must stop and ask before continuing when:

- the same operation failed more than once and the cause is unclear;
- a third attempt can alter state;
- the next attempt requires higher privilege;
- the next attempt can affect SSH, sudo, network, storage, Docker, data or
  production service availability.

## Runtime Boundary

Rules:

- permanent architecture decisions belong in `infra-standards`;
- scripts and executable automation belong in `infra-runtime`;
- bootstrap executable logic belongs in `infra-runtime`;
- operational utilities belong in `infra-runtime`;
- runtime findings that define permanent architecture must be promoted to ADR,
  Standard or Template in `infra-standards`.

## Report Requirement

For relevant interventions, agents must generate an operational report under:

```text
reports/
```

Rules:

- in Platform Workspace, reports are written under
  `/home/emerson/platform/reports/`;
- in Operational Workspace, reports are written under `/opt/projects/reports/`;
- local reports are transitional until explicit consolidation into
  `infra-live-docs`.

Report filename:

```text
REL-YYYY-MM-DD-NNN-description.md
```

Reports must include:

- objective;
- documents updated;
- standards formalized;
- runtime items migrated;
- commits created;
- push result;
- validation;
- recommended next step.
