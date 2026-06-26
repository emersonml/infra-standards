# Decision Record

Decision ID:

0004-docker-layout

Status:

Accepted

Date:

2026-06-26

Context:

Docker stacks need predictable locations for compose files and persistent data.

Decision:

Docker project files live under `/opt/projects`. Persistent Docker data lives
under `/mnt/nfs/docker/volumes`.

Consequences:

- Compose files are easy to find and audit.
- Persistent data is separated from local VM disk.
- NFS validation becomes part of service readiness.

Alternatives Considered:

- Store compose and data together inside `/srv`.
- Store persistent data in `/var/lib/docker`.
- Use unmanaged `docker run` commands.

Implementation:

Project configuration:

```text
/opt/projects/<service>
```

Persistent data:

```text
/mnt/nfs/docker/volumes
```

References:

- `standards/DOCKER_STANDARD.md`
- `standards/NFS_STANDARD.md`
