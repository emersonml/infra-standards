# Decision Record

Decision ID:

0002-artifacts-standard

Status:

Accepted

Date:

2026-06-26

Context:

Critical services require recovery material that can be validated and used
outside the original VM.

Decision:

Recovery artifacts must be stored under:

```text
/mnt/nfs/artifacts
```

Consequences:

- Recovery data is separated from runtime container data.
- Artifacts can include dumps, manifests, checksums, images and restore docs.
- Artifacts must be validated before being considered usable.

Alternatives Considered:

- Store artifacts only inside `/opt/projects`.
- Store artifacts inside Docker volumes.
- Store artifacts on local VM disk.

Implementation:

Artifact packages should include manifest, checksums and restore documentation.

References:

- `standards/BACKUP_STANDARD.md`
- `ARCHITECTURE.md`
