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
GitHub infra-standards > VM152 Codex > Infrastructure VMs > /opt/projects + /mnt/nfs + Docker + Artifacts
```

Layers:

- GitHub stores global standards and templates.
- VM152 Codex reads standards, updates documentation and proposes changes.
- Infrastructure VMs implement the standards according to their role.
- NFS stores persistent data and recovery artifacts.
- OMV is responsible for definitive second-disk backup.

## Standard VM Layout

Every VM should follow this layout when applicable:

```text
/opt/projects
/opt/projects/HOST.md
/opt/projects/.docs
/mnt/nfs
/mnt/nfs/docker/volumes
/mnt/nfs/artifacts
```

Responsibilities:

- `/opt/projects` stores project configuration, compose files and live VM docs.
- `/opt/projects/HOST.md` is the main local context for the VM.
- `/opt/projects/.docs/` stores local operational documentation.
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

## Automation Model

Automation must follow standards and must not hide operational changes.

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
