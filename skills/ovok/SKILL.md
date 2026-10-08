---
name: ovok
description: >
  Build and troubleshoot Ovok applications: verify FHIR R4 APIs, SDKs,
  authentication, AccessPolicies, settings, integrations, and Signals workflows.
  Use for developer-time work on Ovok, not Ovok's runtime `$ai` feature.
license: Proprietary
metadata:
  version: "1.1.0"
---

# Ovok App Builder

Build patient-facing, practitioner-facing, and operational applications on Ovok. Ovok is Actimi's FHIR-native backend; application teams own their user experience and product behavior.

## Verify the platform

1. Inspect the repository, framework, package manager, and existing patterns. Read `AGENTS.md`, `CLAUDE.md`, or other agent-specific repository instruction files that apply.
2. Find relevant current material from the [Ovok Docs router](https://docs.ovok.com/llms.txt), then fetch the page-specific `llms.txt` files for the workflow. Use current docs as the contract for platform-specific behavior.
3. When source or the docs repository is available, inspect the current implementation. If docs, source, installed SDK declarations, or environment behavior conflict, identify the mismatch and prefer evidence closest to deployed behavior. State what you could not verify.

## Load detail only when relevant

- Project features/settings and inheritance: [references/settings-and-features.md](references/settings-and-features.md).
- Workflow prerequisites, failures, writes, and side effects: [references/gotchas-checklist.md](references/gotchas-checklist.md).
- Ontoly graph use for codebase structure: [references/ontoly.md](references/ontoly.md).
- Signals, MDR, and medical-device claims: [references/regulatory.md](references/regulatory.md).
- Stable route scaffolding: [references/common-paths.md](references/common-paths.md); verify current contract before implementation.
- Worked practitioner blood-pressure flow: [references/example-walkthrough.md](references/example-walkthrough.md).

## Build and verify

- Separate Console administration (projects, settings, features, memberships, AccessPolicies, CMS, billing) from application code (authentication, APIs/SDKs, FHIR workflows, and UI). Give an admin the verified Console steps and implement the supported app flow.
- Use FHIR R4 only after confirming the deployed backend's `GET /fhir/R4/metadata` CapabilityStatement. Check the installed SDK's docs/declarations and the environment's advertised operations; package and backend releases may differ.
- Use sandbox and synthetic data by default. Keep PHI, credentials, and access tokens out of prompts, client code, fixtures, logs, and reports. Never target production for tests.
- Implement with explicit patient and project scope, documented authorization, and distinct loading, empty, and failure states. Exercise relevant unhappy paths—unauthorized access, missing configuration, invalid input, and partial or failed writes—before declaring the workflow complete; report cases you could not test.
- Do not invent Ovok routes, FHIR behavior, fields, SDK methods, Console controls, feature gates, or regulatory claims.

## Stop and ask

Get explicit human direction before changing production, deleting or loosening a `signals_silence` threshold, expanding AccessPolicy or `ProjectMembership` scope, or making a change that could expose PHI. Do not infer approval from a request to investigate or prepare a change.

## Report back using this fixed structure

- **Files changed:**
- **Docs cited:**
- **Console steps for an admin:**
- **Assumptions:**
- **Not verified:**
