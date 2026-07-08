# Decision Record

Decision ID:

0018-ssh-client-forward-agent-policy

Status:

Accepted

Date:

2026-07-08

Classification:

- Arquitetura
- Seguranca
- Padrao
- Procedimento

Context:

The platform uses Bastion access and ProxyJump for administrative SSH flows.
Some client configurations enabled SSH agent forwarding or omitted explicit
identity files, increasing exposure if a Bastion session is compromised and
causing ambiguous authentication attempts.

The platform needs a client-side SSH standard that minimizes agent exposure,
keeps Bastion usage predictable and prevents `Too many authentication failures`
caused by trying unintended keys.

Decision:

Adopt `ForwardAgent no` as the default for all institutional SSH clients.

Require explicit `IdentityFile` and `IdentitiesOnly yes` for institutional
hosts. Prefer `ProxyJump` for Bastion access. Permit `ForwardAgent yes` only as
a documented exception.

Consequences:

- SSH agents are not exposed to Bastion hosts by default.
- Bastion remains a transport path instead of a place that can use local agent
  credentials.
- Authentication becomes predictable because each host declares the intended
  key.
- Local port forwarding remains available when the server permits TCP
  forwarding, because `LocalForward` is independent from agent forwarding.
- Existing configs that rely on forwarded agents must be reviewed and migrated.

Implementation:

Update:

```text
standards/SSH_STANDARD.md
```

Recommended client defaults:

```sshconfig
Host *
    ForwardAgent no
    ServerAliveInterval 30
    ServerAliveCountMax 3
    HashKnownHosts yes
    IdentitiesOnly yes
```

Recommended host entries:

```sshconfig
Host <vm>
    HostName <ip>
    User <nominal-user>
    Port 22123
    IdentityFile ~/.ssh/<approved-key>
    IdentitiesOnly yes
    ProxyJump bastion
    ForwardAgent no
```

Validation:

```text
ssh -G <host> | grep -Ei 'forwardagent|identityfile|proxyjump|user|hostname|port'
```

References:

- `standards/SSH_STANDARD.md`
- `decision-records/0009-administrative-access-model.md`
- `decision-records/0015-role-based-agent-privilege-model.md`
