# Decision Record

Decision ID:

0015-role-based-agent-privilege-model

Status:

Accepted

Date:

2026-07-06

Classification:

- Arquitetura
- Governanca
- Padrao
- Seguranca

Context:

The platform already adopted Least Privilege and Just-In-Time Privilege for AI
agents in ADR 0014. The first implementation model still allowed ambiguity
around broad administrative groups and temporary sudoers management.

The architecture now requires clearer separation between:

- collaboration;
- architecture;
- engineering;
- execution;
- audit;
- administrative authority.

Permissions must belong to platform functions, not directly to individual
users. AI agents should occupy roles and receive only task-scoped privileges
through an approved mechanism.

Decision:

Adopt a role-based privilege model for AI agents, mediated by a Policy Broker.

Official role groups:

- `_ssh`: SSH login only.
- `infra-collab`: collaborative ownership for documentation, projects and
  operational files.
- `infra-architect`: Codex eligibility for architecture, preparation,
  bootstrap, ACLs and governance.
- `infra-engineer`: Claude eligibility for engineering execution, Docker,
  Compose, installations, migrations and operation.
- `infra-audit`: future read-only validation role.

The `infra-admin` group is deprecated as a new standard for administrative
authority. Historical environments may keep it temporarily, but new VMs must
not use `infra-admin` as the default expression of authority. Collaboration
must use `infra-collab`.

The Policy Broker becomes the official layer that materializes temporary
privileges. Codex must not edit `/etc/sudoers` or `/etc/sudoers.d` directly.
Claude must never alter sudoers, users, groups or its own privileges.

Consequences:

- collaboration and authority are separated.
- role membership indicates eligibility, not direct privilege.
- sudo is no longer the primary governance interface for agents.
- temporary privilege grants become brokered, auditable and revocable.
- future executable automation belongs in `infra-runtime`.
- existing VMs that use `infra-admin` for collaboration should be migrated
  gradually with documentation.

Implementation:

Update the official standards:

```text
standards/PRIVILEGE_GOVERNANCE_STANDARD.md
standards/AI_AGENT_STANDARD.md
standards/SECURITY_STANDARD.md
standards/SSH_STANDARD.md
standards/VM_STANDARD.md
```

Future executable implementation:

```text
infra-runtime/infra-policy/
```

The executable Policy Broker should support:

- approved policy manifests;
- task-scoped grants;
- TTL or explicit revocation;
- audit logs;
- fail-closed validation;
- sudoers generation only through approved templates when unavoidable;
- ACL-first permission grants when sufficient.

Alternatives Considered:

- Keep `infra-admin` as the main administrative group.
- Create many independent `grant-*` scripts without a broker.
- Let Codex generate sudoers directly during each task.
- Give permanent sudo to Codex or Claude.
- Use one shared technical account for all agents.

Rejected alternatives increase ambiguity, weaken auditability and make it
harder to enforce least privilege.

References:

- `decision-records/0008-ai-agent-governance.md`
- `decision-records/0009-administrative-access-model.md`
- `decision-records/0014-ai-agent-jit-privilege-model.md`
- `standards/PRIVILEGE_GOVERNANCE_STANDARD.md`
- `standards/AI_AGENT_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
