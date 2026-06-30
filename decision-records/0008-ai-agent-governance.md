# Decision Record

Decision ID:

0008-ai-agent-governance

Status:

Accepted

Date:

2026-06-30

Classification:

- Arquitetura
- Governanca
- Padrao

Context:

AI agents are used to assist infrastructure diagnosis, documentation,
implementation and validation. Without a standard, agent authority, access and
reporting can become unclear.

Decision:

The infrastructure separates the human operator from AI agents. Emerson remains
the final authority. Standard agent users are `codex-infra` and `claude-infra`.

Consequences:

- Agent actions are attributable to nominal users.
- Agent privileges can be scoped and audited.
- Agents must follow the same documentation, evidence, rollback and report
  rules as human operation.
- Agents must ask before repeated failed attempts when risk increases.

Alternatives Considered:

- Use only the human user for all agent access.
- Treat agent execution as ungoverned automation.

Implementation:

Detailed rules are defined in:

```text
standards/AI_AGENT_STANDARD.md
```

References:

- `ARCHITECTURE.md`
- `standards/AI_AGENT_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
