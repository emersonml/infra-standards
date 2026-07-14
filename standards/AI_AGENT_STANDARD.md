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

- `_ssh`
- `infra-collab`
- `infra-architect`
- `infra-engineer`
- `infra-audit`

## Official Responsibility Chain

Official flow:

```text
Emerson
 |
 v
Codex
 |
 v
Claude
```

Responsibilities:

- Emerson remains the Chief Architect and final authority.
- Codex is responsible for architecture, planning, security, governance and
  privilege preparation.
- Claude is responsible for engineering execution inside the permission boundary
  prepared by Codex.

## Authority Model

Rules:

- Emerson remains the final authority for infrastructure changes.
- AI agents execute diagnostics, documentation, approved procedures and
  validation.
- AI agents must not expand scope without authorization.
- AI agents must not alter production infrastructure without an approved plan.
- AI agents must not change scripts, bootstrap or automation when the task is
  documentation-only.
- Codex controls privilege preparation during approved Just-In-Time windows.
- Codex must use the Policy Broker for temporary privilege materialization.
- Claude must not expand its own privileges or alter sudoers by initiative.

## Access Model

Rules:

- each AI agent uses a nominal user;
- `_ssh` controls SSH login;
- `infra-collab` controls collaborative ownership and operational files;
- `infra-architect` identifies Codex eligibility for preparation work;
- `infra-engineer` identifies Claude eligibility for execution work;
- `infra-audit` is reserved for future read-only review;
- `infra-admin` is legacy and must not be used as the new authority model;
- sudo for agents must be brokered, scoped and documented;
- temporary sudo for bootstrap must be replaced by Policy Broker flows,
  removed, reduced or justified after use;
- private keys must not be shared between human and agent users;
- permanent sudo for agents must not exist for convenience;
- privilege grants for agents must follow the Least Privilege and Just-In-Time
  Privilege model.

Detailed privilege governance is defined in:

```text
standards/PRIVILEGE_GOVERNANCE_STANDARD.md
```

## Codex Privilege Controller Role

Codex is the platform privilege controller for approved preparation work.

Responsibilities:

- prepare directories, ownership, groups, ACLs and brokered privilege policy;
- validate security before delegating execution;
- document permissions granted to Claude;
- revoke its own temporary privileges after preparation;
- avoid installing or operating services when the work can be delegated to
  Claude.

Codex must not remain a permanent administrator for convenience.
Codex must not edit `/etc/sudoers` or `/etc/sudoers.d` directly.

## Claude Execution Role

Claude is the engineering execution agent.

Responsibilities:

- install applications;
- run Docker Compose;
- configure services;
- execute migrations and operational procedures approved by Emerson.

Rules:

- Claude must use only permissions prepared by Codex;
- Claude must stop when a task requires privileges not granted;
- Claude must not alter sudoers, groups or its own administrative authority;
- Claude must not call privilege broker actions that grant privileges to itself
  unless that action was explicitly prepared and authorized by Codex.

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
  changes;
- when privileges are required, run the additional flow:
  `Authorization > Policy Broker > Codex preparation > Codex revocation >
  Claude execution > Broker revocation > Codex review`.

## Codex-Claude Bridge

The official operational channel for Codex to coordinate Claude is the
Codex-Claude Bridge.

Purpose:

- avoid using Emerson as a manual messenger between agents;
- make prompts, responses, logs and execution status auditable;
- prevent long-running Claude calls from leaving Codex without a result;
- separate review and execution modes;
- preserve the official chain `Emerson > Codex > Claude`.

Standard location on the Claude VM:

```text
/home/emerson/platform/bin/claude-bridge-run
/home/emerson/platform/bridge/claude/
  inbox/
  outbox/
  logs/
  archive/
```

Modes:

| Mode | Purpose | Write access | Privilege escalation |
| --- | --- | --- | --- |
| `review` | Critical analysis, plan review and risk assessment | No | No |
| `execute` | Approved technical execution inside prepared permissions | Yes, task-scoped | No sudo or self-escalation |

Rules:

- Codex must send Claude prompts through the bridge when agent-to-agent
  coordination is required.
- Claude must write relevant execution output, diagnostics and reports to the
  bridge outbox, logs or the platform reports directory.
- `review` mode must be used before execution when the task has operational,
  security, data or availability risk.
- `execute` mode must only be used after Emerson authorization and after Codex
  prepares the required permissions.
- The bridge must use timeouts and persist outputs under `outbox/` and `logs/`.
- Claude must not use the bridge to grant privileges to itself.
- The bridge is an orchestration mechanism, not a privilege mechanism.

Report location for Platform Workspace tasks:

```text
/home/emerson/platform/reports/
```

The Codex-Claude Bridge is a transitional operational implementation until the
full Policy Broker automation is implemented in `infra-runtime`.

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
