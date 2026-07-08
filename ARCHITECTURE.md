# Architecture

## Purpose

This document defines the macro architecture of the infrastructure standards.

The `infra-standards` repository is the official source for global technical
standards. It explains how VMs, storage, Docker, NFS, artifacts, backups,
documentation, automation, security, observability and migrations connect.

Global standards are stored in GitHub:

```text
https://github.com/emersonml/infra-standards
```

Local VM documentation is stored inside each VM:

```text
/opt/projects/HOST.md
/opt/projects/.docs/
```

Global standards define rules. Local VM docs describe implementation,
operational state and exceptions.

## Architecture Principles

- Keep global standards centralized in GitHub.
- Keep operational state documented inside each VM.
- Do not duplicate local VM state in the standards repository.
- Prefer explicit paths and predictable layouts.
- Use Docker Compose for service stacks.
- Store persistent container data on NFS.
- Store recovery artifacts on NFS.
- Keep definitive second-disk backup responsibility in OMV.
- Treat documentation as part of the operational system.
- Validate real environment before changing standards or local docs.

## Infrastructure Layers

Conceptual model:

```text
GitHub infra-standards > Human Operator > AI Agents > Infrastructure VMs > /opt/projects + /mnt/nfs + Docker + Artifacts
```

Layers:

- GitHub stores global standards and templates.
- Emerson is the human operator and final authority for infrastructure changes.
- Codex governs architecture, security, planning and privilege preparation.
- Claude executes approved engineering procedures inside the prepared
  permission boundary.
- Infrastructure VMs implement the standards according to their role.
- NFS stores persistent data and recovery artifacts.
- OMV is responsible for definitive second-disk backup.

## AI Agent Architecture

Classification:

- Arquitetura
- Governanca
- Padrao

The infrastructure supports assisted operation by AI agents.

Roles:

- `emerson`: human operator and owner of final authorization.
- `codex-infra`: Codex governance and privilege preparation identity.
- `claude-infra`: Claude engineering execution identity.

Rules:

- AI agents do not replace the human operator.
- AI agents must follow the same standards as a human operator.
- AI agents must read the relevant local context before execution.
- AI agents must stop and ask before repeated failed attempts on the same
  operation.
- AI agents must document permanent decisions in the correct standards layer.
- AI agents must write operational reports for relevant interventions.
- Codex prepares privileges and environment before delegating execution.
- Claude executes only within the permission boundary prepared by Codex.
- Agent privileges are assigned to platform roles, not directly to users.
- The Policy Broker is the official mechanism for materializing temporary
  privileges.

Detailed rules are defined in:

```text
standards/AI_AGENT_STANDARD.md
standards/PRIVILEGE_GOVERNANCE_STANDARD.md
```

## Access Architecture

Classification:

- Arquitetura
- Governanca
- Padrao

Standard administrative identities:

- `emerson`: nominal human administrator.
- `codex-infra`: nominal Codex automation user.
- `claude-infra`: nominal Claude automation user.

Standard groups:

- `_ssh`: users authorized for SSH login.
- `infra-collab`: collaborative ownership for projects, documentation and
  operational files.
- `infra-architect`: Codex role for architecture, preparation and governance.
- `infra-engineer`: Claude role for engineering execution.
- `infra-audit`: future read-only audit role.

Rules:

- SSH login and administrative privilege are separate controls.
- Membership in `_ssh` allows SSH access only.
- Membership in role groups identifies functional eligibility, not direct sudo.
- `infra-admin` is legacy and must not be used as the new authority model.
- Sudo privileges for agents must be brokered, scoped, documented and
  removable.
- Temporary sudo during bootstrap is allowed only for institutional bootstrap
  and must be mediated, removed or reduced after validation.
- Privileges for agents follow Least Privilege and Just-In-Time Privilege.
- Codex may receive temporary administrative authority only for approved
  preparation work.
- Claude must not alter sudoers, groups or its own privilege level.
- Collaboration must use `infra-collab`, not administrative authority.

## Application Access Architecture

Classification:

- Arquitetura
- Governanca
- Padrao
- Seguranca

Application access is separate from operating system access.

Standard application Web administrator:

```text
root-admin
```

Rules:

- `root-admin` is the primary administrator for application Web panels.
- `root-admin` is not Linux `root`, database root, SSH access or hypervisor
  access.
- personal names should not be used for new primary Web panel administrators.
- integration tokens should use dedicated service identities when available.
- existing historical environments must not be rewritten without an approved
  operational remediation plan.

Reference:

```text
standards/APPLICATION_ACCESS_STANDARD.md
decision-records/0016-application-primary-admin-identity.md
```

## Application Configuration Architecture

Classification:

- Arquitetura
- Governanca
- Padrao
- Operacao

Applications should be initialized with explicit regional defaults instead of
relying on upstream image or application defaults.

Standard regional defaults for Brazilian operations:

```text
language = pt_BR
locale = pt_BR
phone region = BR
timezone = America/Maceio
```

Rules:

- new applications must set these values when supported;
- existing user preferences must not be changed without approval;
- client-specific exceptions must be documented.

Reference:

```text
standards/APPLICATION_CONFIGURATION_STANDARD.md
decision-records/0017-application-regional-defaults.md
```

## Privilege Governance Architecture

Classification:

- Arquitetura
- Governanca
- Padrao
- Seguranca

Official flow:

```text
Emerson authorizes
 |
 v
Policy Broker materializes approved temporary privilege
 |
 v
Codex prepares privilege boundary
 |
 v
Codex revokes temporary privilege
 |
 v
Claude executes
 |
 v
Policy Broker revokes task privileges
 |
 v
Codex validates and documents
```

Rules:

- Codex is a temporary administrator, not a permanent administrator.
- Codex prepares directories, ownership, groups, ACLs and execution policy when
  authorized.
- Codex does not edit `/etc/sudoers` or `/etc/sudoers.d` directly.
- The Policy Broker mediates temporary privilege grants and revocation.
- Claude performs service installation, compose execution, configuration and
  migration using prepared permissions.
- Claude never changes users, groups, sudoers or its own privilege level.
- Runtime automation for grants, revocation and validation belongs in
  `infra-runtime`.

Reference:

```text
standards/PRIVILEGE_GOVERNANCE_STANDARD.md
decision-records/0014-ai-agent-jit-privilege-model.md
decision-records/0015-role-based-agent-privilege-model.md
```

## Bastion and ProxyJump Model

Classification:

- Arquitetura
- Padrao
- Procedimento

Remote administration should prefer an approved Bastion path when direct access
is not required.

Conceptual flow:

```text
Operator or Agent > Bastion > Target VM
```

SSH client flow:

```text
ssh -J <bastion> <target>
```

Rules:

- Bastion is an access path, not a place to bypass governance.
- ProxyJump must preserve nominal user identity.
- Direct SSH to a VM is allowed only when the network model and authorization
  allow it.
- Bastion changes require documentation and rollback.

## Standard VM Layout

Every VM should follow this layout when applicable:

```text
/opt/projects
/opt/projects/HOST.md
/opt/projects/.docs
/opt/projects/reports
/mnt/nfs
/mnt/nfs/docker/volumes
/mnt/nfs/artifacts
```

Responsibilities:

- `/opt/projects` stores project configuration, compose files and live VM docs.
- `/opt/projects/HOST.md` is the main local context for the VM.
- `/opt/projects/.docs/` stores local operational documentation.
- `/opt/projects/reports/` stores operational reports.
- `/mnt/nfs/docker/volumes` stores persistent container data.
- `/mnt/nfs/artifacts` stores recovery artifacts.

## Storage Model

Official storage layout:

```text
/mnt/nfs > docker > volumes
/mnt/nfs > artifacts
```

Meaning:

```text
/mnt/nfs/docker/volumes = persistent container data
/mnt/nfs/artifacts = recovery artifacts, dumps, manifests, images, restore docs
```

Rules:

- Each VM has one NFS export.
- Each VM mounts its export at `/mnt/nfs`.
- Persistent Docker data must use `/mnt/nfs/docker/volumes`.
- Recovery artifacts must use `/mnt/nfs/artifacts`.
- A local directory must never silently replace a missing NFS mount.
- The real second-disk backup is the responsibility of OMV.
- VMs do not make definitive backups of themselves.

## Docker Model

Docker is used as the standard runtime for service stacks.

Rules:

- Use Docker Compose for stacks.
- Keep compose files under `/opt/projects`.
- Keep persistent data under `/mnt/nfs/docker/volumes`.
- Do not store databases in local VM disk without explicit authorization.
- Do not rebuild or upgrade images during migration unless the plan requires it.
- Preserve critical images by digest or by `docker save` when required.

Standard relationship:

```text
/opt/projects/<service>/docker-compose.yml > Docker Compose > /mnt/nfs/docker/volumes/<service>
```

## Artifacts Model

Recovery artifacts are controlled packages that allow rebuild or restore of a
service without depending only on the original VM.

Standard location:

```text
/mnt/nfs/artifacts
```

Recommended content:

- compose files;
- Dockerfile when relevant;
- redacted environment file;
- restricted original environment file when authorized;
- database dump;
- exported Docker images when critical;
- manifest;
- checksums;
- restore documentation.

Artifacts must be validated by:

- file count;
- size;
- checksum;
- restore listing when applicable;
- image tar inspection when applicable.

## Backup Model

Backup responsibilities are separated.

VM responsibility:

- produce recovery artifacts;
- validate artifacts;
- document restore procedures;
- store artifacts in `/mnt/nfs/artifacts`.

OMV responsibility:

- replicate NFS data to a second disk;
- provide definitive storage-level backup policy;
- protect NFS exports according to infrastructure policy.

Rule:

- VMs do not implement definitive second-disk backup of themselves.

## Documentation Model

Global standards:

```text
GitHub infra-standards
```

Local VM documentation:

```text
/opt/projects/HOST.md
/opt/projects/.docs/
```

Separation:

```text
Global standards define rules.
Local VM docs describe implementation and exceptions.
```

Local VM docs must describe:

- current operational state;
- local inventory;
- local network;
- local decisions;
- local changes;
- local exceptions;
- local TODO items.

Global standards must describe:

- reusable rules;
- templates;
- architecture principles;
- required validation flows;
- common roadmaps.

Operational reports:

```text
/opt/projects/reports/
```

Operational reports must describe:

- work performed;
- evidence collected;
- files changed;
- validation;
- rollback;
- pending risks.

Reports do not replace official documentation.

## Automation Model

Automation must follow standards and must not hide operational changes.

The runtime boundary is explicit:

- `infra-standards` stores architecture, governance, policies and templates.
- `infra-runtime` stores scripts, automations, executable bootstrap and
  operational utilities.
- permanent runtime decisions must be migrated into `infra-standards`;
- executable runtime components must not be copied into `infra-standards`.

Expected script categories:

- `backup.sh`;
- `restore.sh`;
- `update.sh`;
- `healthcheck.sh`;
- `maintenance.sh`.

Rules:

- Scripts must be documented before operational use.
- Scripts must log intent and result.
- Scripts must avoid destructive operations by default.
- Scripts must validate prerequisites.
- Scripts must update documentation when they change standards or state.

Automation must respect the intervention flow:

```text
Diagnostico > Evidencias > Plano > Execucao > Validacao > Documentacao > Relatorio
```

## Architecture Governance

Architecture evolution flow:

```text
Idea > RFC > Review > Approval > Decision Record > Standard > Template > VM > Service
```

Governance layers:

- Philosophy: defines permanent principles.
- Requests: propose infrastructure changes.
- Standards: define reusable infrastructure rules.
- Decision Records: register approved architecture decisions and their impact.
- Templates: provide consistent starting points for VM documentation.
- VM Documentation: records operational state and local exceptions.
- Operational Documentation: records execution evidence, validation and final state.

Rule:

```text
Philosophy > Architecture > Lifecycle > Requests > Decision Records > Standards > Templates > VM Documentation > Services
```

Responsibilities:

- `PHILOSOPHY.md` defines permanent principles.
- `ARCHITECTURE.md` defines macro structure and governance.
- `LIFECYCLE.md` defines VM lifecycle.
- `requests/` contains RFCs and proposed changes.
- `decision-records/` explains why approved decisions exist.
- `standards/` defines how infrastructure should be built.
- `templates/` defines how local docs should start.
- `/opt/projects/HOST.md` and `/opt/projects/.docs/` describe each VM.
- Change reports document what was done and validated.

Governance rules:

- Not every RFC will be approved.
- Not every RFC generates a Standard.
- Every approved Standard must have a corresponding Decision Record.
- No infrastructure evolution should become a Standard directly.

Git publication rules:

- Local commits are work records.
- Remote push is official publication.
- Codex may create local commits during work.
- Codex must not push automatically after each commit.
- Emerson can explicitly request push.
- Before push, Codex must report pending local commits.
- Push must not happen with a dirty worktree.

Git publication flow:

```text
Local Commit > Validate Worktree > Count Pending Commits > Review Pending Log > Push > Confirm Remote
```

## Security Model

Hardening must follow this flow:

```text
Identify risk > Document risk > Define mitigation > Evaluate operational impact > Test in controlled scope > Validate affected functions > Document final state
```

Hardening must not break these functions without documented justification:

```text
SSH
SCP
RSYNC
SFTP
Automation
Backup
Recovery
```

Security rules:

- Do not store secrets in documentation.
- Do not commit private keys.
- Do not expose services directly to the Internet by default.
- Do not alter firewall, DNS, proxy or certificates without authorization.
- Prefer key-based SSH authentication.
- Keep operational access recoverable during hardening.
- Separate human operator access from AI agent access.
- Keep Bastion and ProxyJump paths documented when used.
- Use temporary sudo only during approved bootstrap or recovery windows.

## Bootstrap Model

Classification:

- Arquitetura
- Procedimento
- Padrao

Institutional bootstrap is the initial process that brings a VM to the standard
baseline.

Bootstrap flow:

```text
Base VM > Identity > SSH > Groups > /opt/projects > Documentation > NFS > Docker > Snapshot > Ready
```

Rules:

- bootstrap may use temporary sudo when required;
- temporary sudo must be documented before use;
- temporary sudo must be removed, reduced or explicitly justified after
  bootstrap;
- bootstrap must create `/opt/projects`, `/opt/projects/HOST.md`,
  `/opt/projects/.docs/` and `/opt/projects/reports/`;
- bootstrap must end with validation and a report.

## Observability Model

Observability is a platform function, not a local service detail.

Expected scope:

- hosts;
- VMs;
- Docker;
- NFS;
- network equipment;
- critical applications;
- alerts;
- dashboards;
- capacity planning.

Rules:

- Observability services should follow the same Docker, NFS, artifact and
  documentation standards.
- Monitoring state and exceptions must be documented in the VM that hosts the
  platform.
- Global observability standards should remain in this repository.

## Migration Model

Standard migration flow:

```text
Discovery > Documentation > Artifacts > Pre-flight > Migration > Validation > Cutover > Rollback plan > Final documentation
```

Rules:

- Do not mix migration with upgrade unless explicitly planned.
- Produce artifacts before changing production state.
- Validate destination before cutover.
- Keep rollback plan documented.
- Update local VM documentation after migration.
- Update global standards only when a reusable rule changes.

## Exception Model

Exceptions are allowed only when documented.

Each exception must record:

- what differs from the standard;
- why it differs;
- operational impact;
- risk;
- rollback or future correction path;
- responsible VM or service.

Exceptions belong in local VM documentation when they describe implementation.

Exceptions belong in `infra-standards` only when they change or clarify a global
rule.

## Roadmap

[OK ] Establish infra-standards repository
[TODO] Consolidate SSH standard
[TODO] Consolidate Docker standard
[TODO] Consolidate NFS standard
[TODO] Consolidate Backup standard
[TODO] Consolidate Documentation standard
[TODO] Standardize VM template
[TODO] Standardize observability platform
[TODO] Review NFSv3 to NFSv4 migration
[TODO] Define second-disk backup policy on OMV
