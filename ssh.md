# SSH / Authentication Reference

## How SSH works
Private key — stays on your machine, proves your identity
Public key  — uploaded to GitHub, like a lock GitHub holds
Rule: whoever verifies you holds the public key

## Key locations on your machine
~/.ssh/id_ed25519      # private key — never share, never commit
~/.ssh/id_ed25519.pub  # public key — safe to share

## Generate SSH key (standard way)
ssh-keygen -t ed25519 -C "your@email.com"
# hit enter for all prompts

## ed25519
Modern encryption algorithm for SSH keys
Replaces older RSA — shorter, faster, more secure

## gh CLI (GitHub's helper tool)
gh auth login          # authenticate + auto generate SSH key
gh repo create NAME --public   # create new repo
gh repo clone NAME     # clone your repo

## One key per machine rule
~/.ssh/id_ed25519 works for ALL your GitHub repos
New machine → generate new key → upload to GitHub

## How authentication works when you push
1. Your Mac presents private key
2. GitHub checks if matching public key is on file
3. Match → you're in, no password needed

## HTTPS vs SSH
HTTPS — needs token or password each time
SSH   — silent, automatic, preferred for SRE

## Personal Access Token (PAT) — HTTPS fallback
github.com → Settings → Developer Settings → Tokens (classic)
Check "repo" scope → generate → use as password
Store in Mac keychain: git config --global credential.helper osxkeychain