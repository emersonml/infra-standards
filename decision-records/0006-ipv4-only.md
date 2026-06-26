# Decision Record

Decision ID:

0006-ipv4-only

Status:

Accepted

Date:

2026-06-26

Context:

The infrastructure currently operates on IPv4. IPv6 is not part of the approved
operational model.

Decision:

The infrastructure operates as IPv4-only unless IPv6 is explicitly authorized.

Consequences:

- Network documentation focuses on IPv4.
- Service publication and validation must use IPv4.
- IPv6 link-local presence does not imply operational IPv6 support.

Alternatives Considered:

- Dual-stack operation by default.
- Per-service IPv6 enablement without global standard.

Implementation:

All network standards and VM docs must document IPv4 as the operational
protocol unless an approved exception exists.

References:

- `standards/NETWORK_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
