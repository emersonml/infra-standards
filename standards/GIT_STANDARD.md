# Git Standard

## Objective

Define how Git must be used in the `infra-standards` repository.

This repository separates local work history from official publication.

## Core Policy

Local commit represents work record.

Remote push represents official publication.

Codex may create local commits during work.

Codex must not push automatically after every commit.

## Local Commit

Local commits should be created when:

- a small step is complete;
- a relevant document is created;
- a relevant document is updated;
- work is about to move to another subject;
- a long session is about to end.

Local commit flow:

```text
Change > Validate > Commit > Continue
```

Rules:

- Validate ASCII-only documentation before commit.
- Validate Git diff before commit.
- Keep commits focused.
- Use objective commit messages.
- Do not mix unrelated topics in one commit.

## Remote Push

Remote push should happen only when:

- Emerson explicitly requests it;
- it is the end of the operational day;
- there are 10 local commits pending without push;
- there is a critical or very important change;
- an important project phase is closed.

Remote push flow:

```text
Review > Clean Worktree > Count Local Commits > Show Pending Commits > Push > Confirm Remote
```

## Required Validation Before Push

Before any push, run:

```bash
git status
git log --oneline origin/main..HEAD
```

Rules:

- Always report how many local commits exist before push.
- If worktree is not clean, do not push.
- If local and remote diverge, stop and report.
- If there is a conflict, do not resolve automatically without authorization.
- Never push automatically without publication criteria.

## Codex Responsibility

Codex must:

- create local commits when useful as work records;
- avoid automatic push after every commit;
- check worktree status before push;
- show pending local commits before push;
- report divergence or conflict instead of forcing publication;
- ask or wait for Emerson when publication criteria are not met.

## Emerson Responsibility

Emerson can:

- explicitly request push;
- approve publication timing;
- define when a change is critical;
- decide how to handle divergence or conflicts;
- request squashing or restructuring before publication.

## Publication Meaning

After push, the remote repository represents official published state.

Before push, local commits are working history and are not official publication.

## Prohibited Actions

- Do not push with dirty worktree.
- Do not push after every commit by habit.
- Do not force push without explicit authorization.
- Do not resolve conflicts automatically without authorization.
- Do not publish secrets or private keys.
