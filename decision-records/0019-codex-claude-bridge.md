# Decision Record

Decision ID:

0019-codex-claude-bridge

Status:

Accepted

Date:

2026-07-14

Classification:

- Arquitetura
- Governanca
- Padrao
- Procedimento

Context:

The platform adopted the official agent chain:

```text
Emerson
 |
 v
Codex
 |
 v
Claude
```

Codex is responsible for architecture, planning, security, governance,
privilege preparation and review. Claude is responsible for engineering
execution inside the permission boundary prepared by Codex.

During operational work, direct ad hoc SSH calls from Codex to Claude were not
reliable enough for real tasks. Small prompts could succeed, while longer
review prompts could hang without a persisted result. This weakened the desired
workflow and pushed Emerson toward acting as a manual messenger between agents.

Decision:

Adopt the Codex-Claude Bridge as the official operational channel for
agent-to-agent coordination.

Standard runtime location on the Claude VM:

```text
/home/emerson/platform/bin/claude-bridge-run
/home/emerson/platform/bridge/claude/
  inbox/
  outbox/
  logs/
  archive/
```

The bridge supports two modes:

- `review`: read-only critical review, diagnostics, risk assessment and plan
  validation.
- `execute`: approved technical execution with write tools enabled only inside
  permissions previously prepared by Codex.

The bridge must persist prompts, responses, metadata and logs. It must use
timeouts and return clear exit status. It must not grant privileges and must not
replace the Policy Broker.

Consequences:

- Emerson is not required to copy prompts manually between Codex and Claude.
- Codex can coordinate Claude directly and review the resulting output.
- Claude responses become auditable through `outbox/` and `logs/`.
- Long-running or stuck Claude calls fail in a controlled way.
- Review and execution are separated.
- The bridge remains compatible with Least Privilege and Just-In-Time
  Privilege.

Implementation:

Update:

```text
standards/AI_AGENT_STANDARD.md
```

Operational state on VM154:

```text
/home/emerson/platform/bin/claude-bridge-run
/home/emerson/platform/bridge/claude/
```

Future executable consolidation should move or mirror the bridge implementation
into:

```text
infra-runtime/
```

Alternatives Considered:

- Continue using ad hoc SSH and stdin calls.
- Ask Emerson to manually copy prompts between agents.
- Give Claude broader autonomous authority.
- Make Claude execute without a persisted review channel.

Rejected alternatives reduce traceability, increase operational friction and
weaken the governance chain.

References:

- `decision-records/0014-ai-agent-jit-privilege-model.md`
- `decision-records/0015-role-based-agent-privilege-model.md`
- `standards/AI_AGENT_STANDARD.md`
- `standards/PRIVILEGE_GOVERNANCE_STANDARD.md`
