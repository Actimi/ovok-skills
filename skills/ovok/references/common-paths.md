# Stable Ovok path scaffolding

These paths are structural reminders, not a frozen API contract. Fetch the current page-specific docs and the target environment's CapabilityStatement before relying on any route, method, search parameter, or operation contract. Start at the [docs router](https://docs.ovok.com/llms.txt).

## Tenant authentication

The eight documented tenant-auth routes are:

| Flow | Patient | Practitioner |
| --- | --- | --- |
| Start sign-in | `POST /auth/tenant/Patient/login/start` | `POST /auth/tenant/Practitioner/login/start` |
| Verify MFA when required | `POST /auth/tenant/Patient/login/mfa` | `POST /auth/tenant/Practitioner/login/mfa` |
| Exchange session code for tokens | `POST /auth/tenant/Patient/login/token` | `POST /auth/tenant/Practitioner/login/token` |
| Register | `POST /auth/tenant/Patient/register` | `POST /auth/tenant/Practitioner/register` |

Read [Authentication](https://docs.ovok.com/authentication/llms.txt) and the audience-specific sign-in/registration page for PKCE, `tenantCode`, MFA, request shapes, and flow-specific setup. Do not copy a route shape as a substitute for those details.

## Project settings and features

| Purpose | Path |
| --- | --- |
| Read the current project's complete feature list | `GET /v1/projects/me/features` |
| Replace the complete feature list | `PATCH /v1/projects/me/features` |
| Read project Boolean and text settings | `GET /v1/project/settings` |
| Write one Boolean setting | `PUT /v1/project/settings/:key` |
| Write or unset one text setting | `PUT /v1/project/settings/values/:key` |

See [references/settings-and-features.md](settings-and-features.md) for payloads, replacement behavior, unset defaults, and inheritance.

## FHIR capability discovery

`GET /fhir/R4/metadata` returns the target server's FHIR R4 `CapabilityStatement`. Check its `fhirVersion`, resource interactions, search parameters, and operations before relying on them. The path itself does not prove that a route or operation works for every project, token, or environment.

For example requests, use `https://api.sandbox.ovok.com` unless the user specifies another documented environment. Read [CapabilityStatement](https://docs.ovok.com/api/fhir/r4/capability-statement/llms.txt) and the resource/operation docs alongside the live response.
