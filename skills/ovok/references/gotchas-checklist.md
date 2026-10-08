# Integration gotchas checklist

Read the current docs page's prerequisites, limitations, and error behavior, then check these items that apply to the requested workflow. Do not invent a missing behavior; report unresolved gaps.

- **Identity and authorization:** identify the actor, token type, role, tenant, membership, and required AccessPolicy interactions. Check that each operation is supported for that identity.
- **Project and patient boundaries:** establish which project owns each resource, whether a child project is involved, how patient scope is enforced, and which values or policies are project-local.
- **Configuration:** check exact feature gates, settings, defaults, default policies, app URLs, email templates/providers, storage, and external credentials. Separate Console setup from application implementation.
- **Failure behavior:** verify status codes and error shapes, validation, conflicts, rate limits, and whether an ambiguous response intentionally hides resource state. Do not infer private account/resource state from it.
- **Writes and retries:** determine whether writes are atomic, partial, asynchronous, destructive, or idempotent; establish safe retry and completion/status-check behavior before implementing.
- **Side effects:** identify emails, invitations, notifications, queued work, webhooks, FHIR writes, and third-party calls. Know what can and cannot be rolled back.
- **Existing state:** check effects on issued tokens, memberships, scheduled jobs, existing resources, links, and already-running workflows when configuration changes.
- **Limits and versions:** verify pagination, payload limits, environment differences, FHIR version, backend capability, and installed SDK declarations.

**Wrong:** assume `/fhir/R4/Observation/$lastn` is a standard FHIR route. It is Ovok-specific; verify it in the current operation docs and the target server's `/fhir/R4/metadata` CapabilityStatement.
