# DevSecOps Repo Baseline

Drop-in security/CI baseline for any repo you want to showcase. Copy the
contents of this folder into a new or existing repo, replace the
`{{PLACEHOLDER}}` values, and you get:

| File | What it does |
|---|---|
| `.github/workflows/codeql.yml` | Static analysis (SAST) on every push/PR + weekly scan |
| `.github/workflows/dependency-review.yml` | Blocks PRs that add vulnerable/incompatible dependencies |
| `.github/workflows/secret-scan.yml` | Gitleaks scan in CI (catches what pre-commit misses, e.g. fork PRs) |
| `.github/dependabot.yml` | Automated dependency update PRs |
| `.pre-commit-config.yaml` | Local hooks — secrets and lint issues caught before commit |
| `SECURITY.md` | Tells people how to report a vulnerability responsibly |
| `.github/CODEOWNERS` | Requires your review on every PR |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR checklist incl. a security section |
| `.github/ISSUE_TEMPLATE/` | Structured bug/feature templates |

## Setup checklist for a new repo

1. Copy this folder's contents into the target repo.
2. Replace placeholders:
   - `{{GITHUB_USERNAME}}` in `.github/CODEOWNERS`
   - `{{CONTACT_EMAIL}}` in `SECURITY.md`
3. Trim `codeql.yml`'s language matrix and `dependabot.yml`'s ecosystems
   to what the repo actually uses.
4. Install pre-commit locally once:
   ```bash
   pip install pre-commit
   pre-commit install
   ```
5. In the repo's GitHub settings, enable:
   - **Settings → Code security** → Dependabot alerts, Dependabot security
     updates, Secret scanning, Push protection, CodeQL
   - **Settings → Branches** → branch protection rule on `main`:
     require a PR before merging, require status checks to pass
     (CodeQL, Dependency Review, Gitleaks), require review from Code
     Owners
6. Commit and push — the workflows run automatically from here on.

## Why this set and not more

This is the practical 80% for a portfolio repo: it demonstrates you know
SAST, dependency scanning, secret scanning, and branch protection —
the same four controls most real DevSecOps pipelines start with — without
dragging in a SIEM, DAST scanner, or infra you'd need a live deployment
to justify. Add container scanning (Trivy) if/when a project actually
ships a Dockerfile.
