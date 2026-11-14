# Omni-channel alerts

Read the current [Omni-channel alerts](https://docs.ovok.com/signals/with-ovok/omni-channel-alerts) and [Routing rules](https://docs.ovok.com/signals/with-ovok/routing-rules) pages before building alert delivery.

## Who does what

- Signals decides whether there is an alert and, from the project's `notifications` routing rule (JSON Logic), plans who is told, over which channels, and when to escalate. It announces each step as the `alert.notify` webhook and never contacts a person.
- Ovok, with `OMNICHANNEL_ALERTS_ENABLED` on, resolves each step's roles to people from FHIR and tells them by email and push. It keeps no copy of the alert.
- An app or backend that wants its own channel (SMS, pager, chat) takes `alert.notify` on its own Signals webhook. Never poll alerts to send messages from app code, and never evaluate routing or escalation conditions in app code.

## Set it up

1. The project needs a Signals tenant of its own; shared-tenant projects answer `409 SIGNALS_TENANT_NOT_OWN`.
2. Switch on `OMNICHANNEL_ALERTS_ENABLED` (`PUT /v1/project/settings/OMNICHANNEL_ALERTS_ENABLED`).
3. Save a rule with `PUT /v1/omnichannel-alerts/rules`. Without one, nobody is told.
4. Record recipients in FHIR: `Patient.generalPractitioner` or record shares (`practitioner`), active `CareTeam` members (`caregiver`), `RelatedPerson` (`relatedPerson`), accepted shares with carers and patients (`sharedWith`).
5. Map the `ALERT_NOTIFICATION` and `PATIENT_ALERT_NOTIFICATION` email templates. For push, set the project's push provider and register each signed-in user's device token.
6. Check who a rule reaches with `POST /v1/omnichannel-alerts/preview` before relying on it.

## Verify

- `GET /v1/omnichannel-alerts/rules` returns the rule you saved.
- A preview step lists every expected person, with `email: true` where an email should go.
- In the sandbox, a breaching reading delivers step 0; acknowledging the alert stops later steps.
