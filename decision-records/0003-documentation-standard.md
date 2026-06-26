# Decision Record

Decision ID:

0003-documentation-standard

Status:

Accepted

Date:

2026-06-26

Context:

Global standards and local VM state have different purposes. Mixing them creates
stale documentation and unclear ownership.

Decision:

Global standards live in GitHub. Local VM documentation lives inside each VM.

Consequences:

- `infra-standards` defines reusable rules.
- Each VM documents its own operational state and exceptions.
- Divergence must be resolved by validating the real environment and updating
  the correct documentation layer.

Alternatives Considered:

- Store all VM state in the GitHub repository.
- Store standards only inside individual VMs.

Implementation:

Global standards:

```text
GitHub infra-standards
```

Local VM documentation:

```text
/opt/projects/HOST.md
/opt/projects/.docs/
```

References:

- `standards/DOCUMENTATION_STANDARD.md`
- `ARCHITECTURE.md`
