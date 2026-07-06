# Decision Record

Decision ID:

0014-ai-agent-jit-privilege-model

Status:

Accepted

Superseded or refined by:

- `0015-role-based-agent-privilege-model`

Date:

2026-07-06

Classification:

- Arquitetura
- Governanca
- Padrao
- Seguranca

Context:

The platform uses AI agents to support architecture, documentation, execution
and operational validation. Earlier decisions separated human and agent
identities, but the platform now requires a stronger privilege governance model.

The approved operating model separates authority, preparation and execution:

```text
Emerson
 |
 v
Codex
 |
 v
Claude
```

Emerson remains the final authority. Codex becomes the privilege controller for
the platform. Claude remains the technical execution agent.

Decision:

Adopt Least Privilege and Just-In-Time Privilege as official platform
principles for AI agents.

Codex receives administrative privilege only when explicitly authorized by
Emerson and only for preparation work. Codex prepares directories, ownership,
groups, ACLs, sudoers and execution policy. Codex then revokes its own temporary
privileges and documents the permissions delegated to Claude.

Claude executes engineering tasks only with the permissions prepared by Codex.
Claude must not expand its own privileges, modify sudoers, add itself to
administrative groups or bypass the approved privilege model.

Consequences:

- Codex is no longer only a documentation or architecture agent.
- Codex becomes the platform privilege controller during approved preparation
  windows.
- Codex must not remain a permanent administrator for convenience.
- Claude remains the engineering executor and must operate inside the prepared
  permission boundary.
- Privilege grants become auditable, temporary and task-specific.
- Future VMs must be prepared to support this model.
- Automation for granting, validating and revoking temporary privileges belongs
  in `infra-runtime`.

Implementation:

Create and maintain:

```text
standards/PRIVILEGE_GOVERNANCE_STANDARD.md
```

Update related standards:

```text
standards/AI_AGENT_STANDARD.md
standards/SECURITY_STANDARD.md
standards/SSH_STANDARD.md
standards/VM_STANDARD.md
```

Future executable automation should be implemented in:

```text
infra-runtime/platform-jit-privilege/
```

Alternatives Considered:

- Give permanent sudo to Codex and Claude.
- Allow Claude to adjust its own privileges during execution.
- Keep privilege preparation as manual, undocumented work by Emerson.
- Use only broad `infra-admin` membership without task-specific controls.

Rejected alternatives increase blast radius, reduce auditability and weaken the
separation between governance and execution.

References:

- `decision-records/0008-ai-agent-governance.md`
- `decision-records/0009-administrative-access-model.md`
- `standards/AI_AGENT_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
- `standards/PRIVILEGE_GOVERNANCE_STANDARD.md`
