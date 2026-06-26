# Decision Record

Decision ID:

0007-migration-policy

Status:

Accepted

Date:

2026-06-26

Context:

Service migrations are high-risk when mixed with upgrades, undocumented
changes or missing rollback plans.

Decision:

Migrations must follow the standard migration flow and must not include
unrelated upgrades unless explicitly planned.

Consequences:

- Discovery and documentation happen before migration.
- Recovery artifacts are created before production changes.
- Pre-flight validation is required.
- Cutover and rollback are planned before execution.
- Final documentation records the resulting state.

Alternatives Considered:

- Direct cutover without artifact creation.
- Combining migration and version upgrades by default.
- Relying only on runtime volumes for rollback.

Implementation:

Standard flow:

```text
Discovery > Documentation > Artifacts > Pre-flight > Migration > Validation > Cutover > Rollback > Final Documentation
```

References:

- `ARCHITECTURE.md`
- `LIFECYCLE.md`
