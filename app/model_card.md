# Model Card — bank-transaction-triage-agent

- **App version:** 1.0.0  
- **Git SHA:** f8e206e  
- **Generated:** 2026-09-12T17:05:01.531663Z  
- **Policy version:** v1

## Intended use
Triage bank transactions into approve / review / block.

## Policy
- Block over: £10000
- Review risk levels: ['high']
- Sanctioned countries: ['XX', 'ZZ']

## Evaluation
- Golden set: 5/5 (100%)

## Limitations
- Deterministic policy demo — not a substitute for full AML screening.
- Thresholds are illustrative; calibrate to real risk appetite.

## Rollback
Shift Container Apps traffic to the previous known-good revision (see runbook).