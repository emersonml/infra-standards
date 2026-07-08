# Decision Record

Decision ID:

0017-application-regional-defaults

Status:

Accepted

Date:

2026-07-07

Classification:

- Arquitetura
- Governanca
- Padrao
- Operacao

Context:

New application deployments may inherit default language, locale, region and
timezone from container images or upstream application defaults. This can cause
Brazilian users to be created with English profiles or incorrect regional
settings.

The platform needs a standard default for applications operated in Brazil,
while still allowing client-specific exceptions.

Decision:

Adopt Brazilian regional defaults for new applications unless the client or
service explicitly requires another locale.

Default values:

```text
language: pt_BR
locale: pt_BR
phone region: BR
timezone: America/Maceio
```

Consequences:

- new users are expected to inherit Portuguese/Brazilian defaults;
- application bootstrap must include regional configuration when supported;
- existing users may keep their own preferences and should be changed only when
  explicitly approved;
- exceptions must be documented in live docs and reports.

Implementation:

Create and maintain the official standard:

```text
standards/APPLICATION_CONFIGURATION_STANDARD.md
```

For Nextcloud, the expected settings are:

```text
default_language = pt_BR
default_locale = pt_BR
default_phone_region = BR
default_timezone = America/Maceio
```

Alternatives Considered:

- rely on upstream defaults;
- configure locale manually per user;
- configure only the operating system timezone.

These alternatives increase inconsistency and create avoidable work during user
onboarding.

References:

- `standards/APPLICATION_CONFIGURATION_STANDARD.md`
- `standards/DOCKER_STANDARD.md`
