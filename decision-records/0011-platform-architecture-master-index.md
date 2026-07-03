# Decision Record

Decision ID:

0011-platform-architecture-master-index

Status:

Accepted

Date:

2026-07-03

Classification:

- Arquitetura
- Governanca
- Documentacao
- Onboarding

Context:

The platform evolved from isolated standards into an institutional architecture.
Human architects and AI agents need a first architectural map before reading
specific standards, runtime artifacts or live documentation.

The existing `ARCHITECTURE.md` describes the infrastructure architecture. The
new `PLATFORM_ARCHITECTURE.md` was approved by GOV-005 as a master index that
connects existing documents without duplicating their technical content.

Decision:

`PLATFORM_ARCHITECTURE.md` is the official master index for platform reading and
onboarding.

It must:

- present the platform at a high level;
- connect standards, runtime and live docs by reference;
- document the platform layers;
- guide architects and agents to the correct source documents;
- remain an index, not a detailed technical standard.

It must not:

- replace `ARCHITECTURE.md`;
- duplicate standards;
- duplicate runtime procedures;
- duplicate live documentation;
- create new operational rules by itself.

Consequences:

- New sessions should read the Workspace README, then `PROGRAM_STATUS.md`, then
  `PLATFORM_ARCHITECTURE.md`.
- `PLATFORM_ARCHITECTURE.md` can be promoted as an institutional document.
- Specific rules remain in standards.
- Real observed state remains in `infra-live-docs`.
- Operational artifacts remain in `infra-runtime`.

Alternatives Considered:

- Continue using only `ARCHITECTURE.md` as the general entry point.
- Merge `PLATFORM_ARCHITECTURE.md` into a standard.
- Keep platform architecture only in GOV reports.

Implementation:

Promotion of `PLATFORM_ARCHITECTURE.md` must happen in a future IMP-001 step.
This ADR authorizes the document role, not the promotion itself.

References:

- `PROGRAM_STATUS.md`
- `ARCHITECTURE.md`
- `reports/REL-2026-07-03-gov-005-platform-architecture.md`
- `reports/REL-2026-07-03-arch-review-001-revisao-arquitetura.md`
- `reports/REL-2026-07-03-imp-001-plano-institucionalizacao-plataforma.md`
