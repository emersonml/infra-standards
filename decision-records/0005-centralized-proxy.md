# Decision Record

Decision ID:

0005-centralized-proxy

Status:

Accepted

Date:

2026-06-26

Context:

Services should not be published directly to the Internet by default.
Centralized proxying provides a controlled exposure point.

Decision:

External service exposure must prefer a centralized proxy path instead of
direct publication from individual VMs.

Consequences:

- Public exposure is easier to audit.
- Certificates and external routing remain centralized.
- VM-level services can prefer localhost, LAN or proxy bindings.
- Proxy changes require explicit authorization.

Alternatives Considered:

- Publish services directly from each VM.
- Manage certificates independently on each VM.

Implementation:

Preferred exposure order:

```text
127.0.0.1 > LAN > Proxy > Internet
```

References:

- `standards/NETWORK_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
