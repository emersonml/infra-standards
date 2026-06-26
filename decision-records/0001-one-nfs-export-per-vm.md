# Decision Record

Decision ID:

0001-one-nfs-export-per-vm

Status:

Accepted

Date:

2026-06-26

Context:

VMs require persistent storage on NFS. Multiple exports per VM increase
operational complexity and make validation harder.

Decision:

Each VM must have one dedicated NFS export.

Consequences:

- Mount validation becomes simpler.
- VM storage layout is predictable.
- Export ownership and permissions are easier to audit.
- Additional service paths must live under the VM export.

Alternatives Considered:

- Multiple exports per service.
- Local persistent disks per VM.

Implementation:

Each VM mounts its export at:

```text
/mnt/nfs
```

References:

- `standards/NFS_STANDARD.md`
- `ARCHITECTURE.md`
