# HOST - <VM_NAME>

Ultima atualizacao: YYYY-MM-DD

## Identificacao

- VMID: <vmid>
- Hostname: <hostname>
- Funcao: <funcao>
- Sistema: <sistema>

## Rede

- IPv4: <ip>/<cidr>
- Gateway: <gateway>
- DNS: <dns>
- SSH: `ssh -p 22123 emerson@<ip>`
- Bastion/ProxyJump: <nao_aplicavel_ou_comando>

## Acesso administrativo

- Operador humano: emerson
- Agente Codex: codex-infra
- Agente Claude: claude-infra
- Grupo administrativo: infra-admin
- Grupo SSH: _ssh
- Sudo temporario de bootstrap: <nao_aplicavel|removido|justificado>

## Recursos

- CPU: <cpu>
- Memoria: <memoria>
- Disco raiz: <disco>
- NFS: <estado>

## Diretorios padrao

```text
/opt/projects
/opt/projects/.docs
/opt/projects/reports
/mnt/nfs
/mnt/nfs/docker/volumes
/mnt/nfs/artifacts
```

## Perfil shell

Aliases obrigatorios para VMs de Servico:

```bash
alias ll='ls -la'
alias ppp='cd /opt/projects'
alias vvv='cd /mnt/nfs/docker/volumes'
```

- Estado dos aliases no `.bashrc` do usuario `emerson`: <validado|pendente>

## Servicos

- <servico>: <estado>

## Regras locais

- Ler este arquivo antes de qualquer alteracao.
- Ler todos os arquivos em `/opt/projects/.docs/`.
- Atualizar documentacao apos mudancas.
- Gerar relatorio em `/opt/projects/reports/` para intervencoes relevantes.
- Parar e pedir orientacao antes de multiplas tentativas sem causa raiz clara.

## Documentacao complementar

```text
/opt/projects/.docs/CHANGELOG.md
/opt/projects/.docs/DECISIONS.md
/opt/projects/.docs/INVENTORY.md
/opt/projects/.docs/NETWORK.md
/opt/projects/.docs/TODO.md
```
