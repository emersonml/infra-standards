# SSH Standard

## Objetivo

Definir o padrao SSH da infraestrutura para novas VMs e para a futura VM Template.

## Direcao arquitetural

Classificacao:

- Arquitetura
- Padrao
- Procedimento

Padrao futuro:

```text
/etc/ssh/sshd_config > Include /etc/ssh/sshd_config.d/*.conf > 99-emerson.conf
```

Decisao:

- Padronizar SSH usando `sshd_config.d`.
- Eliminar customizacoes diretas no `sshd_config` principal.
- Nao retornar ao padrao Debian como fonte final.

## Recomendacoes para novas VMs

```text
Port 22123
ListenAddress <IPv4 da VM>
PermitRootLogin no
AllowUsers emerson codex-infra claude-infra
AllowGroups _ssh
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitEmptyPasswords no
StrictModes yes
IgnoreRhosts yes
HostbasedAuthentication no
PermitUserEnvironment no
MaxAuthTries 3
LoginGraceTime 30
MaxSessions 2
ClientAliveInterval 300
ClientAliveCountMax 2
TCPKeepAlive no
PrintMotd no
AuthorizedKeysFile .ssh/authorized_keys
Subsystem sftp /usr/lib/openssh/sftp-server
```

## Criptografia recomendada

```text
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com
```

## Restricoes recomendadas

```text
AllowTcpForwarding no
AllowAgentForwarding no
X11Forwarding no
PermitTunnel no
GatewayPorts no
```

Excecoes devem usar `Match` com justificativa documentada.

## Usuarios e grupos

Classificacao:

- Governanca
- Padrao

Usuarios padrao:

- `emerson`: operador humano.
- `codex-infra`: agente Codex.
- `claude-infra`: agente Claude.

Grupos padrao:

- `_ssh`: autorizado a autenticar via SSH.
- `infra-admin`: autorizado para administracao conforme politica de sudo.

Regras:

- `_ssh` controla login SSH.
- `infra-admin` controla elegibilidade administrativa.
- um usuario pode ter SSH sem sudo;
- um agente nao deve receber privilegio maior que o necessario.
- Codex pode receber privilegio administrativo temporario apenas em janela
  Just-In-Time aprovada.
- Claude nao deve alterar sudoers, grupos administrativos ou seus proprios
  privilegios.
- privilegios temporarios devem ser revogados ao final da tarefa.

Modelo de privilegio detalhado:

```text
standards/PRIVILEGE_GOVERNANCE_STANDARD.md
```

## Bastion e ProxyJump

Classificacao:

- Arquitetura
- Padrao
- Procedimento

Quando houver Bastion aprovado, o acesso deve usar ProxyJump.

Formato:

```text
ssh -J <usuario>@<bastion>:22123 -p 22123 <usuario>@<ip-alvo>
```

Regras:

- preservar usuario nominal no Bastion e no destino;
- evitar chaves compartilhadas entre humanos e agentes;
- documentar excecoes de acesso direto no `NETWORK.md` da VM;
- validar `ssh`, `scp` e `rsync` pelo caminho aprovado.

## Configuracoes que nao devem ser usadas globalmente

```text
ForceCommand /bin/bash
GatewayPorts yes
PermitTunnel yes
X11Forwarding yes
AllowAgentForwarding yes
```

## Validacao obrigatoria

Antes de aplicar:

```text
sudo sshd -t
```

Apos aplicar em projeto autorizado:

```text
sudo systemctl reload ssh.service
ssh -p 22123 emerson@<ip> 'true'
scp -P 22123 arquivo_teste emerson@<ip>:/tmp/
rsync -av -e 'ssh -p 22123' arquivo_teste emerson@<ip>:/tmp/
```

## Rollback

Antes de qualquer mudanca:

```text
sudo cp -a /etc/ssh/sshd_config /etc/ssh/sshd_config.bak-YYYYMMDD
sudo cp -a /etc/ssh/sshd_config.d /etc/ssh/sshd_config.d.bak-YYYYMMDD
```

Rollback:

```text
sudo cp -a /etc/ssh/sshd_config.bak-YYYYMMDD /etc/ssh/sshd_config
sudo rm -f /etc/ssh/sshd_config.d/99-emerson.conf
sudo sshd -t
sudo systemctl reload ssh.service
```
