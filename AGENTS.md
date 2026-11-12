# ToggleMaster GitOps Rules

- This repository is the deployment source of truth; it contains no service source or Terraform.
- Never commit passwords, tokens, private keys, kubeconfigs, Vault contents, or rendered Secrets.
- Image tags must use immutable sha-<12 hex> values after publication is enabled.
- Keep one overlay per service so promotions remain independent.
- Shared namespace and platform resources must have one owner.
- Automated sync is enabled in the manifests. The root Application is the bootstrap gate: never set bootstrap_gitops=true or sync the root without explicit authorization and a passing validate_structure.py --ready.
- Never run kubectl apply, Argo sync, Helm, or cluster mutations without explicit authorization.
- Never add Co-Authored-By lines to commit messages.
- Phase 4 monitoring stack (OTel Collector, Prometheus, Loki, Grafana) lives under observability/ as Argo CD-managed Helm releases with pinned chart versions.
- APM, incident management and ChatOps credentials are referenced by secret identifiers only.
- New Kustomize roots must be added to scripts/validate_structure.py and the render list in .github/workflows/validate.yml in the same change.
