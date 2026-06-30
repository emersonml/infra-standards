# Decision Record

Decision ID:

0010-bootstrap-documentation-and-reports

Status:

Accepted

Date:

2026-06-30

Classification:

- Arquitetura
- Governanca
- Template
- Padrao
- Procedimento

Context:

New VMs and operational interventions require consistent bootstrap,
documentation, reports, snapshots and rollback expectations. Operational reports
must not replace official standards or local live documentation.

Decision:

Institutional bootstrap must create the standard `/opt/projects` structure,
including live documentation and report directories. Temporary sudo is allowed
only for bootstrap when documented and must be removed, reduced or justified
after validation. Operational reports must live separately from official
documentation.

Consequences:

- VM bootstrap has a reusable baseline.
- `/opt/projects/reports/` becomes the standard report location.
- Reports document execution without becoming the source of architecture.
- Snapshot and rollback expectations are part of the VM lifecycle.

Alternatives Considered:

- Store reports inside `.docs`.
- Treat bootstrap sudo as permanent by default.
- Keep bootstrap rules only inside runtime scripts.

Implementation:

Relevant documents:

```text
LIFECYCLE.md
standards/VM_STANDARD.md
standards/DOCUMENTATION_STANDARD.md
templates/OPERATIONAL_REPORT_TEMPLATE.md
```

References:

- `ARCHITECTURE.md`
- `LIFECYCLE.md`
- `standards/VM_STANDARD.md`
- `standards/DOCUMENTATION_STANDARD.md`
