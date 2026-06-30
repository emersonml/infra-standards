# Decision Record

Decision ID:

0009-administrative-access-model

Status:

Accepted

Date:

2026-06-30

Classification:

- Arquitetura
- Governanca
- Padrao
- Procedimento

Context:

Infrastructure access now requires a clear separation between SSH login,
administrative authorization, Bastion access and agent identities.

Decision:

Use standard users `emerson`, `codex-infra` and `claude-infra`. Use `_ssh` for
SSH login authorization and `infra-admin` for administrative authorization.
Prefer Bastion and ProxyJump when segmentation requires an access path.

Consequences:

- SSH access and sudo eligibility are controlled separately.
- Bastion use becomes documented and auditable.
- Agent and human identities remain distinct.
- SSH, SCP and RSYNC validation must cover the approved access path.

Alternatives Considered:

- Control administrative access only through `AllowUsers`.
- Use shared user accounts for agents.
- Treat Bastion as an informal operator preference.

Implementation:

Relevant standards:

```text
standards/SSH_STANDARD.md
standards/SECURITY_STANDARD.md
standards/NETWORK_STANDARD.md
```

References:

- `ARCHITECTURE.md`
- `standards/SSH_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
- `standards/NETWORK_STANDARD.md`
