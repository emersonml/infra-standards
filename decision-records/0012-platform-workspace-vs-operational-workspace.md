# Decision Record

Decision ID:

0012-platform-workspace-vs-operational-workspace

Status:

Accepted

Date:

2026-07-03

Classification:

- Arquitetura
- Governanca
- Workspace
- Onboarding

Context:

Earlier bootstrap decisions used `/opt/projects` as a common workspace pattern.
GOV-004 refined this into a functional distinction between engineering
environments and service environments.

The distinction is functional, not institutional:

- engineering VMs evolve the platform;
- service VMs operate installed services;
- personal directories are outside the platform.

Decision:

The platform recognizes three workspace concepts:

1. Platform Workspace.
2. Operational Workspace.
3. Personal Workspace.

Platform Workspace applies to engineering VMs, including VM152 and VM154.
Its official root is:

```text
/home/emerson/platform
```

Operational Workspace applies to service VMs, including VM230, VM240, VM241 and
VM130. Its official root remains:

```text
/opt/projects
```

Personal Workspace applies to user personal directories, such as:

```text
~/Downloads
~/Desktop
~/Documents
```

Personal Workspace is not a platform source and must not store permanent
institutional documentation.

Consequences:

- Engineering VMs use `/home/emerson/platform`.
- Service VMs continue using `/opt/projects`.
- ADR `0010-bootstrap-documentation-and-reports` remains valid for operational
  bootstrap, but must be interpreted through the Platform Workspace versus
  Operational Workspace distinction.
- Standards that mention `/opt/projects` must be reconciled in future steps
  without breaking service VM conventions.
- Every workspace must have a `README.md` as the official onboarding entry
  point.

Alternatives Considered:

- Keep `/opt/projects` as a universal workspace root.
- Rename the model as institutional versus operational.
- Treat personal directories as informal workspace roots.

Implementation:

Promotion of `WORKSPACE_STANDARD.md` must happen in a future IMP-001 step.
This ADR authorizes the workspace taxonomy and root paths, not a migration by
itself.

References:

- `PROGRAM_STATUS.md`
- `decision-records/0010-bootstrap-documentation-and-reports.md`
- `standards/VM_STANDARD.md`
- `standards/DOCUMENTATION_STANDARD.md`
- `reports/REL-2026-07-03-gov-004-workspace-institucional.md`
- `reports/REL-2026-07-03-arch-review-001-revisao-arquitetura.md`
- `reports/REL-2026-07-03-imp-001-plano-institucionalizacao-plataforma.md`
