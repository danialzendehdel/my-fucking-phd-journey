---
title: "How to Give Someone Access to a Single GitHub Repo"
date: 2026-03-16
description: "Grant read or read/write access to one specific GitHub repository without touching anything else — using SSH deploy keys."
tags: ["github", "ssh", "devops", "security"]
---

# How to Give Someone Access to a Single GitHub Repo

Need to let a machine, agent, or collaborator pull (or push) to **one specific repo** without granting access to your entire GitHub account? Deploy keys are the answer.

A deploy key is an SSH key scoped to a single repository. Even if the private key is compromised, the attacker can only access that one repo — nothing else on your account.

---

## Step 1: Generate a Dedicated SSH Key

Run this on the machine that needs access:

```bash
ssh-keygen -t ed25519 -C "deploy-key-description" -f ~/.ssh/github_deploy_REPONAME -N ""
```

- `-t ed25519` — modern, secure key type
- `-N ""` — no passphrase (needed for automated access)
- `-f` — saves to a specific file so it does not conflict with your main key

---

## Step 2: Add the Public Key to GitHub

1. Go to your repo on GitHub
2. Navigate to **Settings → Deploy keys → Add deploy key**
3. Paste the contents of `~/.ssh/github_deploy_REPONAME.pub`
4. Give it a name (e.g., `my-server`, `ci-bot`)
5. Check **"Allow write access"** only if the machine needs to push

---

## Step 3: Configure SSH to Use This Key

Add an entry to `~/.ssh/config`:

```
Host github-REPONAME
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_deploy_REPONAME
    IdentitiesOnly yes
```

This creates a custom SSH alias so the deploy key is used only for this repo.

---

## Step 4: Clone or Update the Remote URL

New clone:
```bash
git clone git@github-REPONAME:yourusername/your-repo.git
```

Existing repo — update the remote:
```bash
git remote set-url origin git@github-REPONAME:yourusername/your-repo.git
```

---

## Step 5: Test the Connection

```bash
ssh -T git@github-REPONAME
```

You should see:
```
Hi yourusername/your-repo! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Why Deploy Keys Over Personal Access Tokens?

| Method | Scope | Auth | Best For |
|--------|-------|------|----------|
| Deploy Key | Single repo | SSH | Servers, agents, CI |
| PAT | All repos | HTTPS | Scripts, APIs |

Deploy keys are safer for automated access — narrow scope, easy to revoke per-repo.

---

## Quick Summary

```bash
# 1. Generate key
ssh-keygen -t ed25519 -C "deploy" -f ~/.ssh/github_deploy_REPO -N ""

# 2. Add ~/.ssh/github_deploy_REPO.pub to GitHub repo Settings > Deploy keys

# 3. Add to ~/.ssh/config
Host github-REPO
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_deploy_REPO
    IdentitiesOnly yes

# 4. Clone
git clone git@github-REPO:USERNAME/REPO.git
```

Done. One key, one repo, zero blast radius.
