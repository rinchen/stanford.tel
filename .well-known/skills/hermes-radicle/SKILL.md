---
name: hermes-radicle
description: "Use this skill when working with Radicle repositories for peer-to-peer Git collaboration — `rad init`, `rad clone`, patches, issues, node ops, sync, remotes, and session repo detection."
license: MIT
compatibility: "Radicle installed and `rad auth` complete; Radicle node running (`rad node start`). Optional: `gh` CLI for GitHub mirroring."
triggers:
  - radicle
  - rad init
  - rad clone
  - rad patch
  - rad issue
  - rad node
  - rad sync
  - rad remote
---

# Radicle Skill

Use when working with Radicle repositories for p2p git collaboration.

## Requirements

- Radicle installed and configured: `rad auth`
- Radicle node running: `rad node start`
- Optional: `gh` CLI for GitHub mirroring workflows

## This Repository

- **GitHub:** https://github.com/rinchen/hermes-radicle
- **Radicle RID:** `rad:z3Y6EqELoo7SUYhk4WJdJS3zzJoVK`
- **Dual Push:** Configured via `git push rad main`

## Core Commands

### Init a New Radicle Repo

```bash
rad init
```

Creates a new Radicle repository ID and configures the `rad` remote. The rid is printed at the end.

**Verify:** `git remote -v` shows `rad` with a `rad://` URL.

### Clone an Existing Radicle Repo

```bash
rad clone rad:<rid>
cd <project>
git remote -v
# should show rad and rad-push remotes
```

### Push to GitHub + Radicle Together

If you want both remotes updated on push, add the GitHub remote as an additional push URL:

```bash
git remote set-url --add --push origin <github-url>
git remote set-url --add --push origin $(git remote get-url --push rad)
```

Then `git push` updates both.

### Create a Patch

From a branch with commits you want reviewed:

```bash
git checkout feature-branch
rad patch create --base main
```

This generates a reviewable patch against `main`.

### List / Update Issues

```bash
rad issue list
rad issue create --title "..." --description "..."
```

### Node Management

```bash
rad node start
rad node stop
rad node status
rad node info
```

### Sync / Fetch from Peers

```bash
rad sync
```

### Show Remote Configuration

```bash
git remote -v
```

## Session Repo Detection

Whenever the user opens a chat inside a Git repository, detect Radicle remotes:

```bash
git remote -v 2>/dev/null | grep '^rad\b'
```

If `rad` or `rad-push` remotes exist, announce the repo is Radicle-enabled and show the RID from `rad.rid` or the remote URL.

## Radicle Links Format

When sharing repo locations, use markdown with both GitHub and radicle URLs:

- GitHub: https://github.com/rinchen/hermes-radicle
- Radicle: `rad:z3Y6EqELoo7SUYhk4WJdJS3zzJoVK`

## Multi-Mirror Workflow (rad-enable-repo)

Use the helper script `~/repos/rad-enable-repo/rad-enable-repo.sh` to wire GitHub origin push alongside Radicle in one shot. Run it from inside an existing Git repo after `rad init`.

**Current Configuration:** This repo (hermes-radicle) is configured with dual push to both GitHub and Radicle via the `rad` remote. Use `git push rad main` to update both simultaneously.

## Rationale

Radicle provides a permissionless, p2p code collaboration layer. Use it for:
- Long-lived open-source projects
- Censorship-resistant mirrors
- Multi-node redundancy without a central forge

Keep GitHub for CI/PR workflow convenience; use Radicle for canonical distributed provenance.
