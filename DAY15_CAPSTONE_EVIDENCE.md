# Day 15 Capstone — Ship It Safely
**Learner:** Deepak Bhardwaj  
**Date:** 2026-09-13  
**Agent:** Bank Transaction Triage Agent  

## Definition of Done — Evidence

### ✅ 1. Containerised Agent (Slim, Non-Root)
- **Evidence:** `Dockerfile` design.
- Uses `python:3.12-slim` base image for a smaller attack surface.
- Runs as non-root user (`appuser`, UID 10001).
- Multi-stage build with pinned dependencies and built-in healthchecks.
- *[Insert Dockerfile screenshot here]*

### ✅ 2. Eval Gate Blocks Bad Deploys
- **Evidence:** Local execution of `eval_gate.py`.
- **GOOD policy (v1):** 5/5 passed (100%) -> Gate PASSES.
- **BAD policy (v2-bad):** 4/5 passed (80%) -> 20% regression -> Gate FAILS and blocks deploy.
- *[Insert Eval Gate PASS screenshot here]*
- *[Insert Eval Gate FAIL screenshot here]*

### ✅ 3. Model Card Auto-Generated (LLMOps)
- **Evidence:** Output of `scripts/model_card.py`.
- Automatically generates JSON and Markdown model cards on every release.
- Includes intended use, policy version, eval scores, and limitations for auditors (EU AI Act compliance).
- *[Insert model_card.json screenshot here]*

### ✅ 4. Blue/Green Validation (Local Simulation)
- **Evidence:** Ran two local instances simulating Blue (v1) and Green (v2-bad).
- **BLUE (Port 8001):** Correctly BLOCKED a £25,000 high-risk transaction.
- **GREEN (Port 8002):** Dangerously APPROVED the same transaction due to the regressed policy.
- Proves why validation on the Green revision at 0% traffic is critical before the traffic flip.
- *[Insert side-by-side Blue/Green curl output screenshot here]*

### ✅ 5. Rollback Runbook (02:00 Ready)
- **Evidence:** `deploy/04_rollback.sh`.
- A pinned, rehearsed runbook. Uses a single `az containerapp ingress traffic set` command to flip 100% traffic back to the last-good revision.
- No rebuild, no image pull — restores production in seconds.
- *[Insert 04_rollback.sh screenshot here]*

### ✅ 6. Scale Rules & Probes (KEDA)
- **Evidence:** `deploy/containerapp.yaml`.
- Configured for `activeRevisionsMode: Multiple` (required for blue/green).
- KEDA HTTP concurrency rule: 1 warm replica (min), up to 10 (max), scaling at 50 concurrent requests.
- Liveness, Readiness, and Startup probes configured.
- *[Insert containerapp.yaml screenshot here]*

### ✅ 7. Prompt Versioning Registry
- **Evidence:** `app/policies/registry.yaml`.
- Single source of truth for prompt versions.
- Every version tracks the author, commit SHA, and eval scores. Rollback is just changing the `active` pointer.
- *[Insert registry.yaml screenshot here]*

### ✅ 8. CI/CD Pipeline Design
- **Evidence:** `.github/workflows/deploy.yml`.
- Automated on push to `main`.
- Uses OIDC (Federated Identity) for passwordless Azure auth.
- Pipeline: Build -> Eval Gate -> Blue/Green Deploy.
- *[Insert deploy.yml screenshot here]*

## Retrospective (Start/Stop/Continue)
- **START:** Rehearse rollback in game-day drills.
- **STOP:** Editing prompts directly on main without a PR review.
- **CONTINUE:** Blue/green deployments and telemetry-first debugging for every release.