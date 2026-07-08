# Decision Record

Decision ID:

0016-application-primary-admin-identity

Status:

Accepted

Date:

2026-07-07

Classification:

- Arquitetura
- Governanca
- Padrao
- Seguranca

Context:

Applications with Web panels require an initial administrative identity. The
previous local preference used a personal name in the primary Web administrator
account. This creates portability and handover issues when a service is created
for another organization or later transferred to a client.

The platform needs a non-personal, reusable and documented convention for
application-level administration. The convention must not be confused with
Linux `root`, database root users, SSH identities or integration tokens.

Decision:

Adopt `root-admin` as the standard primary Web panel administrator identity for
new applications when the application allows choosing the initial administrator.

Meaning:

- `root-admin` is an application-level primary administrator.
- `root-admin` is not a Linux, SSH, database or hypervisor root account.
- `root-admin` must not be used as a shared operating system user.
- `root-admin` must not be reused for integration tokens when a dedicated
  service account is available.

Consequences:

- New Web panels avoid personal naming tied to Emerson or any individual.
- Client handover becomes cleaner because the primary administrator identity is
  role-based.
- Documentation must distinguish application administration from operating
  system administration.
- Existing environments that already use another primary Web administrator
  should be treated as historical state and remediated only through an approved
  operational change.
- Credentials for `root-admin` remain secrets and must not be committed or
  documented in plaintext.

Implementation:

Create and maintain the official standard:

```text
standards/APPLICATION_ACCESS_STANDARD.md
```

Update references in:

```text
README.md
PROGRAM_STATUS.md
PLATFORM_ARCHITECTURE.md
ARCHITECTURE.md
standards/SECURITY_STANDARD.md
```

Alternatives Considered:

- Keep `emerson-admin`.
- Use generic `admin`.
- Use one account name per client.
- Use application-specific account names only.

Rejected alternatives either preserve personal coupling, increase attack
surface through a generic name, or reduce standardization across services.

References:

- `standards/APPLICATION_ACCESS_STANDARD.md`
- `standards/SECURITY_STANDARD.md`
- `decision-records/0009-administrative-access-model.md`
