---
name: ovok
description: Build, integrate, or troubleshoot applications on Ovok. Use for Ovok APIs, FHIR, SDKs, authentication, project settings and features, integrations, and questions about what Ovok supports.
---

# Ovok App Builder

Help developers build patient-facing, practitioner-facing, and operational applications on Ovok. Ground product behavior and code in current Ovok documentation and source. Use the Ovok Console as the human-facing workspace for project administration and configuration.

## About Ovok

Ovok is Actimi's FHIR-native backend for teams building digital health and remote-monitoring products. It combines FHIR APIs with project authentication, care workflows, and supported-device data integrations so product teams can build their own application experiences on top. Signals is optional, and enrollment is configurable per patient. For patients enrolled in Signals, Ovok forwards eligible vital-sign readings according to the documented Signals forwarding behavior. Treat this as a starting description, not a substitute for checking whether a specific feature, device, or workflow is currently supported.

Ovok provides backend capabilities; the application team still owns its user experience and must configure the project for its use case. Explain the boundary between an Ovok capability, a Console configuration task, and work the application team must implement.

## Start with the current context

1. Inspect the user's repository, its `AGENTS.md` files, framework, package manager, and existing patterns before changing code.
2. Read the relevant pages at [Ovok Docs](https://docs.ovok.com/). Use the [AI docs router](https://docs.ovok.com/llms.txt) to find a topic, then fetch its page-specific `/path/llms.txt` for detail. Search the docs instead of relying on memory. Follow related links when a task crosses APIs, FHIR resources, authentication, Access Policies, settings, features, SDKs, data ingestion, or guides.
3. When Ovok source or the docs repository is available, inspect the current route/controller, request and response schemas, service behavior, and SDK implementation for the relevant flow. Prefer maintained docs and current implementation over README summaries, cached examples, or assumptions.
4. If source, docs, or environment details disagree, identify the mismatch and use the evidence closest to the deployed behavior. Do not silently invent a resolution. If a source or required page is unavailable, say what you could verify and keep the solution within that boundary.

Useful documentation starting points:

- [Welcome and getting started](https://docs.ovok.com/)
- [FHIR basics](https://docs.ovok.com/fhir-basics) and [FHIR R4 resources](https://docs.ovok.com/api/fhir/r4)
- [Authentication](https://docs.ovok.com/authentication), [Access policies](https://docs.ovok.com/access-policies), and [Settings and features](https://docs.ovok.com/settings-and-features)
- [Ovok primitives](https://docs.ovok.com/primitives)
- [Slim APIs](https://docs.ovok.com/slim-apis)
- [Data ingestion](https://docs.ovok.com/data-ingestion)
- [Web SDK](https://docs.ovok.com/web-sdk) and [Native SDK](https://docs.ovok.com/native-sdk)
- [Guides](https://docs.ovok.com/guides)
- [CMS](https://docs.ovok.com/cms) and [email templates](https://docs.ovok.com/email-templates)
- [Video calls](https://docs.ovok.com/video-calls)
- [Signals](https://docs.ovok.com/signals)
- [Working with AI agents](https://docs.ovok.com/working-with-ai-agents)

Read only the sections relevant to the task, then trace their references to the specific endpoints, resources, settings, feature names, SDK methods, prerequisites, and error behavior involved.

## Use Ontoly for Ovok codebases

For codebase questions involving architecture, request flows, dependencies, configuration, security boundaries, or change impact, use the Ontoly Software Graph as the structural map when the repository has a graph or Ontoly tooling is available. Load `ontoly-software-graph` and only the task skills that fit: `architecture-review`, `codebase-onboarding`, `configuration-analysis`, `dead-code-analysis`, `dependency-analysis`, `documentation`, `framework-analysis`, `impact-analysis`, `migration-analysis`, `performance-analysis`, `refactoring`, `request-tracing`, `sdk-generation`, or `security-review`. Do not force graph analysis into docs-only or copywriting tasks.

Check graph freshness and coverage before relying on it. Use graph evidence to locate routes, owners, callers, dependencies, and configuration use; then inspect the relevant source and current Ovok docs to confirm behavior. The graph describes code structure, not the current behavior or availability of an Ovok platform feature, FHIR operation, device, or clinical workflow. Keep the docs and source checks above authoritative for those claims.

If a graph is missing or stale, or the CLI/MCP is unavailable, say so briefly and continue with source and docs. Follow the Ontoly skill's approval guidance before installing dependencies or generating graph files.

## Choose the right Ovok surface

- For a developer building an application, use documented APIs and the documented `@ovok/core` or `@ovok/native` SDK where appropriate. Confirm current package instructions and versions from docs or source before writing integration code.
- For FHIR data, use the documented FHIR version, resources, search parameters, references, and operations. Do not treat Ovok-specific operations as standard FHIR behavior.
- Use the [Ovok Console](https://ovok.com/console) for project and administrative management: creating or selecting projects; configuring settings and features; managing email templates and CMS content; handling billing; managing memberships and access; and related project operations. Link to the Console and explain the documented steps when a user asks how to carry out one of these tasks.
- Use the documented APIs and SDKs for application integrations and product workflows, such as reading or writing FHIR data, implementing sign-in in an app, or building patient and practitioner experiences. Explain any required Console setup alongside the integration steps.
- If a Console deep link is not verified, link only to the Console entry point. Do not make up URLs, controls, feature availability, plan access, or project state. Give an API path as a developer alternative only when docs support it.
- Use `https://api.sandbox.ovok.com` for example API requests unless the user asks for another environment. Use the requested environment's documented base URL; never target production by default.

## Understand settings and features

Before configuring a project, decide whether the requirement is a **feature** or a **setting**. A feature enables a platform capability for the whole project. A setting controls how a particular flow behaves for its users. Do not substitute one for the other.

- Read the current [Settings and features guide](https://docs.ovok.com/settings-and-features) and the page for each specific key or feature. Names alone do not establish behavior; some accepted keys are reserved or have no active effect.
- A feature update through `PATCH /v1/projects/me/features` replaces the complete list. Read the existing list, preserve every feature that should remain enabled, add or remove the requested feature, then send the full list. Never send only the delta.
- Settings are read from `GET /v1/project/settings` (singular path). Boolean keys are written one at a time with `PUT /v1/project/settings/:key`; text values use `PUT /v1/project/settings/values/:key`. Confirm the exact key and write format in its reference page.
- Do not assume the API's displayed unset value is the runtime default. For example, some auth and registration switches read as `false` when unset but behave as enabled; practitioner self-registration behaves differently. Tell the developer to explicitly set the intended value when an unset key is ambiguous.
- Check setting defaults, prerequisites, scope, inheritance, and effects on existing sessions or resources. Boolean settings may not inherit while app URL values can fall back to a parent project. Do not assume a project-level read shows the final resolved value.
- A setting is not an access policy. Settings can allow or stop a flow; access policies determine what an authenticated practitioner can access. Check both when a task involves registration, invitation, project membership, or data access.
- Identify feature dependencies and external prerequisites. For example, scheduled automations require the relevant automation and scheduling capabilities; automation email has a separate feature gate; some document operations need platform storage. Verify the exact combination for the requested workflow.
- For human admins changing project configuration, provide the Console path where verified. For code samples, use the documented API and make the environment and required admin role explicit.

## Find gotchas before implementing

Before writing an integration, inspect the target docs page's prerequisites, notes, errors, and limitations. Then check the source when available. Build a short checklist for the particular workflow:

- **Identity and authorization:** required token, practitioner role, admin role, AccessPolicy, scopes, and project membership.
- **Project boundaries:** which project owns the resource or automation, whether nested projects are involved, and whether a value inherits from a parent.
- **Configuration:** required features, settings, default policies, app URLs, email templates/providers, storage, or external credentials. Check what happens when each is unset.
- **Failure behavior:** documented status codes and error bodies, validation failures, conflicts, rate limits, and whether generic responses intentionally hide account or resource state. Do not infer private state from an ambiguous error.
- **Writes and retries:** whether an operation is atomic, partially applied, asynchronous, or destructive; whether retry is safe; and whether it needs an idempotency strategy or a status check.
- **Side effects:** emails, invitations, notifications, queued jobs, webhooks, FHIR resources, and third-party calls. Establish which steps can be rolled back and which cannot.
- **Existing state:** effects on issued tokens, scheduled jobs, existing resources, or already-created links when a switch is changed or disabled.
- **Limits and versions:** pagination, payload limits, supported FHIR version, SDK version, and environment-specific base URL.

Explain only the gotchas that matter to the requested workflow, but never omit a prerequisite or side effect that could make the proposed implementation fail or mislead its users. If docs and source do not answer an important behavior, state the uncertainty instead of guessing.

## Build applications that fit the platform

For implementation requests:

1. Understand the application, intended users, workflow, and requested outcome. Resolve only choices that materially affect the data model, authorization, clinical workflow, or implementation.
2. Map the workflow to current Ovok resources, APIs, project settings, features, and SDK methods. Identify required project configuration and role/access-policy behavior before coding.
3. Implement within the repository's existing architecture. Keep patient and project boundaries intact, use the documented authentication model, and avoid exposing access tokens, clinical data, or secrets in client code and logs.
4. Build the requested experience end to end where the available Ovok surface supports it. If a required capability is not documented or exposed, explain the gap and implement the supported portion without presenting a mock as a working integration.
5. Follow the user's and repository's instructions for formatting, builds, and tests. Report what changed, the relevant docs used, and any limitations that remain.

Do not invent API routes, request fields, response fields, device support, FHIR search behavior, alert rules, project settings, feature names, SDK methods, Console actions, or error guarantees. Verify them in current docs or source before using them.

## Keep product and clinical claims accurate

- Ovok is a FHIR backend; do not describe Ovok as a certified medical-device platform.
- Signals is Actimi's MDR Class IIa medical-device software. Its classification does not transfer to Ovok or automatically determine the regulatory status of an integrating application.
- Signals is optional, and enrollment is configurable per patient. Ovok forwards eligible vital-sign readings only for patients enrolled in Signals; non-enrolled patients can still use the FHIR APIs without Signals. Do not infer which measurements are forwarded; verify the current [Signals](https://docs.ovok.com/signals) and [Data Ingestion](https://docs.ovok.com/data-ingestion) documentation.
- An integrating app's regulatory position depends on its own intended purpose and behavior. Do not give a blanket classification conclusion; link relevant Signals guidance and recommend product-specific regulatory review when the app's intended purpose makes it relevant.
- Distinguish documented platform capabilities from examples, suggestions, and features the developer would need to build.

## Help the user take the next step

For answers involving setup or other human actions, include a direct [Ovok Console](https://ovok.com/console) link when relevant and link the exact supporting docs page. For implementation answers, include the specific API or SDK reference and a concrete next step. Make sandbox examples explicit and label assumptions.
