# Infrastructure Philosophy

## Purpose

Define the permanent principles of the infrastructure.

This document does not define technologies. It defines how the infrastructure
must be reasoned about, changed, documented and governed.

## Core Principles

- Documentation before implementation.
- Evidence before change.
- Root cause before correction.
- Rollback before execution.
- Infrastructure as documentation.
- One responsibility per VM.
- One NFS export per VM.
- Global standards.
- Local implementation.
- Standards evolve through governance.
- Every architectural decision must be documented.
- Commit local work before changing context.
- Publish remote changes only with clear criteria.
- Human authority remains separate from AI agent execution.
- Official documentation remains separate from operational reports.
- Codex prepares and governs privileges; Claude executes within prepared
  boundaries.
- Least Privilege and Just-In-Time Privilege are default security principles.

## Engineering Principles

- Prefer simple and explicit designs.
- Prefer repeatable procedures over manual memory.
- Prefer known operational state over assumptions.
- Separate migration from upgrade.
- Separate standards from local implementation.
- Separate standards from executable runtime.
- Keep blast radius small.
- Validate before and after change.
- Treat exceptions as documented debt.

## Documentation Principles

- Documentation is part of the infrastructure.
- README.md is the repository entry point.
- PHILOSOPHY.md defines permanent principles.
- ARCHITECTURE.md defines the macro architecture.
- LIFECYCLE.md defines the VM lifecycle.
- RFCs propose changes.
- ADRs record approved decisions.
- Standards define official rules.
- Templates define starting points.
- VM documentation records local state and exceptions.
- Operational reports record execution evidence and outcome.
- ASCII-only documentation.

## Security Principles

- Security must preserve recoverability.
- Security with operational validation.
- Hardening must be tested in controlled scope.
- Hardening must not silently break SSH, SCP, RSYNC, SFTP, automation, backup or recovery.
- SSH login and administrative privilege must be controlled separately.
- Bastion and ProxyJump must preserve nominal identity and auditability.
- Secrets must not be stored in documentation.
- Private keys must not be committed.
- Public exposure must be intentional and documented.
- No AI agent should keep permanent administrative privilege for convenience.
- Temporary privilege must be authorized, documented, validated and revoked.

## Automation Principles

- Automation first.
- Automation must be documented before operational use.
- Automation must validate prerequisites.
- Automation must expose intent and result.
- Automation must not hide destructive actions.
- Automation must follow the same evidence and rollback rules as manual work.
- AI agents must stop and ask before repeated failed attempts on the same task.
- Privilege automation must be idempotent, auditable and reversible.

## Operational Principles

- Snapshots before migration.
- Backups and recovery artifacts before risky changes.
- Production state must be preserved unless a plan says otherwise.
- Local VM documentation must reflect operational reality.
- If documentation and environment disagree, the environment is evidence and documentation must be corrected.
- Changes must end with validation and a report.
- Local commits preserve work history.
- Remote push publishes official repository state.
- Snapshots protect rollback for structural or risky changes.
- Temporary sudo is allowed for bootstrap only when documented and removed or reduced after use.
- Codex may receive temporary administrative privilege to prepare an approved
  task, but must reduce its own privilege surface before delegating execution.

## Decision Principles

- No infrastructure evolution should become a Standard directly.
- Ideas must become RFCs before approval.
- Approved RFCs can become Decision Records.
- A Standard must have a corresponding approved Decision Record.
- Not every RFC is approved.
- Not every approved decision creates a Standard.
- Decisions must document context, consequences and implementation.
- Publication must be intentional.
- Emerson controls explicit publication approval.
- Emerson remains the final authority for AI-assisted infrastructure changes.
