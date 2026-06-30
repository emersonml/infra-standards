# VM Lifecycle

## Planning

Purpose:

- Define why the VM exists before it is created.
- Reserve identity, network and operational role.
- Avoid creating infrastructure without a documented function.

Flow:

```text
Planning > VMID > Hostname > Function > Network
```

Required outputs:

- VMID selected.
- Hostname selected.
- Function documented.
- Network plan documented.
- Storage requirement identified.
- Access model documented.
- Human and agent administrative identities identified when applicable.

---

## Provisioning

Purpose:

- Create a VM from the approved template and bring it to baseline readiness.

Flow:

```text
Template > Clone > Hostname > Network > Identity > SSH > NFS > Docker > Documentation > Snapshot > Ready
```

Required outputs:

- Hostname configured.
- IPv4 configured.
- SSH access validated.
- Standard users and groups configured when applicable.
- NFS mounted when required.
- Docker installed when required.
- `/opt/projects/HOST.md` created.
- `/opt/projects/.docs/` created.
- `/opt/projects/reports/` created.
- Snapshot recommended or created according to change scope.
- Temporary bootstrap sudo removed, reduced or justified.

Bootstrap classification:

- Arquitetura
- Procedimento
- Padrao

---

## Service Deployment

Purpose:

- Deploy a service using the infrastructure standards.

Flow:

```text
Preparation > Diagnosis > Documentation > Deployment > Validation > Artifacts > Final Documentation
```

Required outputs:

- Compose files stored under `/opt/projects`.
- Persistent data mapped to `/mnt/nfs/docker/volumes`.
- Service validated.
- Recovery artifacts created when applicable.
- Local VM documentation updated.

---

## Production

Purpose:

- Keep the VM operating with validated services and current documentation.

Flow:

```text
Validation > Monitoring > Backup > Documentation
```

Required outputs:

- Service health validated.
- Monitoring scope documented.
- Recovery artifacts maintained.
- Operational documentation current.

---

## Maintenance

Purpose:

- Execute controlled changes without losing operational state.

Flow:

```text
Healthcheck > Update > Validation > Snapshot > Documentation
```

Required outputs:

- Pre-change healthcheck recorded.
- Update scope documented.
- Validation completed.
- Snapshot considered according to risk.
- Documentation updated.
- Operational report created when the change is relevant.
- Agent must ask before multiple failed retries on the same operation.

---

## Migration

Purpose:

- Move a service or role without mixing migration with unrelated upgrades.

Flow:

```text
Discovery > Documentation > Artifacts > Pre-flight > Migration > Validation > Cutover > Rollback > Final Documentation
```

Required outputs:

- Source documented.
- Destination validated.
- Recovery artifacts created.
- Snapshot considered or created according to risk.
- Rollback plan documented.
- Cutover validated.
- Final state documented.
- Operational report created.

Rollback classification:

- Procedimento
- Padrao
- Boas praticas

Rollback rules:

- rollback must be planned before execution;
- rollback must identify the exact state to restore;
- rollback must not depend only on memory or shell history;
- rollback must be validated when executed.

---

## Decommission

Purpose:

- Remove a VM or service without losing required records or recovery data.

Flow:

```text
Snapshot > Artifacts > Backup > Remove Monitoring > Remove Proxy > Remove DNS > Archive Documentation > Destroy VM
```

Required outputs:

- Final snapshot or equivalent rollback considered.
- Required artifacts archived.
- Monitoring removed.
- Proxy and DNS dependencies removed when applicable.
- Documentation archived.
- VM destroyed only after approval.

---

## Snapshot and Rollback Policy

Classification:

- Governanca
- Padrao
- Procedimento

Snapshots are required or recommended according to operational risk.

Snapshot is required before:

- destructive structural changes;
- migrations with production impact;
- storage layout changes;
- SSH or access hardening that can affect recoverability;
- rollback tests that may alter persistent state.

Snapshot is recommended before:

- major package updates;
- Docker runtime changes;
- service stack changes;
- bootstrap phases that change identity, SSH or sudo.

Rules:

- snapshot does not replace artifacts or backups;
- snapshot must have a clear restore point name;
- rollback path must be documented before execution;
- if snapshot is skipped, the reason must be documented.
