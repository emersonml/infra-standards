# Decision Record

Decision ID:

0013-reports-and-live-docs-promotion-policy

Status:

Accepted

Date:

2026-07-03

Classification:

- Governanca
- Documentacao
- Live Docs
- Relatorios

Context:

The institutionalization phase uses reports to record diagnostics, execution,
validation and decisions. Reports are necessary evidence, but they must not
become permanent architecture sources by accident.

The platform already recognizes `infra-live-docs` as the repository for live
documentation and consolidated official reports. Local `reports` directories are
transitional work areas.

Decision:

Local reports are transitional by default.

An official report becomes institutional only after explicit consolidation into
`infra-live-docs`.

The approved document lifecycle is:

```text
scratch
 |
 v
workspace
 |
 v
reports
 |
 v
infra-live-docs
```

Promotion to `infra-live-docs` must preserve traceability and must not rewrite
historical evidence unless a future approved activity explicitly requires a
context note.

Consequences:

- Local `reports` directories are not final sources of truth.
- `infra-live-docs` becomes the final repository for official live reports.
- GOV reports can inform ADRs, standards and live docs, but they do not replace
  them.
- Historical reports should remain auditable.
- Consolidation of reports must be planned and validated in a future IMP-001
  step.

Alternatives Considered:

- Treat every local report as official immediately.
- Store reports only beside the workspace where they were created.
- Convert every report directly into a standard.

Implementation:

This ADR defines the promotion policy. It does not move reports, create report
indexes or update `DOCUMENTATION_STANDARD.md`. Those actions belong to future
IMP-001 steps.

References:

- `PROGRAM_STATUS.md`
- `standards/DOCUMENTATION_STANDARD.md`
- `decision-records/0003-documentation-standard.md`
- `decision-records/0010-bootstrap-documentation-and-reports.md`
- `reports/REL-2026-07-03-gov-004-workspace-institucional.md`
- `reports/REL-2026-07-03-gov-005-platform-architecture.md`
- `reports/REL-2026-07-03-arch-review-001-revisao-arquitetura.md`
- `reports/REL-2026-07-03-imp-001-plano-institucionalizacao-plataforma.md`
