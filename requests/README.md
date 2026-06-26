# Requests

The `requests` directory contains Requests For Change for infrastructure
evolution.

An RFC is a proposal. It is not an approved standard.

Governance flow:

```text
Idea > RFC > Review > Approval > Decision Record > Standard > Template > VM > Service
```

Rules:

- No RFC is approved by existence alone.
- Not every RFC will be approved.
- Not every RFC generates a Standard.
- Approved RFCs can generate Decision Records.
- Every approved Standard must have a corresponding Decision Record.
- RFCs must describe problem, motivation, risks, alternatives and impact.
- RFCs must not contain secrets.
- RFCs must use ASCII-only Markdown.

Status values:

- Draft
- In Review
- Accepted
- Rejected
- Superseded

Approved decisions must be moved into `decision-records/` as ADRs before they
become Standards.
