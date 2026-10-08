# Worked flow: practitioner reviews recent blood-pressure observations

Use this as a decision trace, not a drop-in implementation. Read the [Ovok Docs router](https://docs.ovok.com/llms.txt), then fetch the relevant page-specific material:

- [Practitioner sign-in](https://docs.ovok.com/authentication/practitioner-login/llms.txt) and [project setup](https://docs.ovok.com/authentication/project-setup/llms.txt)
- [AccessPolicy model](https://docs.ovok.com/access-policies/policy-model/llms.txt) and [FHIR R4 Observation](https://docs.ovok.com/api/fhir/r4/observation/llms.txt)
- [Get last observations](https://docs.ovok.com/primitives/last-observations/llms.txt) and [CapabilityStatement](https://docs.ovok.com/api/fhir/r4/capability-statement/llms.txt)
- If using an SDK, also check the installed package's docs and declarations and the current [Web SDK](https://docs.ovok.com/web-sdk/llms.txt) or [Native SDK](https://docs.ovok.com/native-sdk/llms.txt) guide.

## Admin setup versus app code

**Console/admin:** select a sandbox project; configure only the documented project settings/features the workflow needs; assign the practitioner a least-privilege AccessPolicy that grants the necessary Patient and Observation access and scopes the records to their authorized patient population. Confirm exact Console controls and effective policy scope in current docs before changing them.

**Application:** authenticate the practitioner through the documented tenant sign-in flow, retain the token according to the app's security architecture, and render the recent observation values. Do not create Console/project administration inside the practitioner-facing app unless explicitly requested and supported by a documented API.

## Discovery and request

Before implementing, verify `GET /fhir/R4/metadata` against the sandbox target. Confirm `fhirVersion`, the Observation interactions/search parameters, and whether the Ovok `lastn` operation is advertised. The docs describe this operation as:

```http
GET /fhir/R4/Observation/$lastn?patient=Patient/{patient-id}&max=20&code=8480-6,8462-4
Authorization: Bearer <practitioner-access-token>
```

`$lastn` is Ovok-specific, not a generic FHIR route. The example LOINC codes identify systolic (`8480-6`) and diastolic (`8462-4`) blood pressure; verify that these match the stored Observation coding for this workflow. Confirm exact operation authorization and supported codes in the current operation docs and target environment. If unavailable, use only an advertised, documented Observation search with supported parameters; do not silently substitute behavior.

## Review and report

Exercise the flow in sandbox with synthetic accounts and records, including an unauthorized practitioner, an out-of-scope patient, a missing setup prerequisite, invalid query values, empty results, and server failure. Verify the rendered units, timestamps, and observation codes from the returned resources; do not turn readings into a diagnosis or clinical recommendation.

Report changed files, the docs used, required Console setup, assumptions, unhappy paths exercised, and anything not verified.
