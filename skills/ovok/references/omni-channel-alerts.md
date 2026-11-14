# Omni-channel alerts

Read the current [Omni-channel alerts](https://docs.ovok.com/signals/with-ovok/omni-channel-alerts) and [Routing rules](https://docs.ovok.com/signals/with-ovok/routing-rules) pages before building alert delivery.

## Who does what

- Signals sends alert events (`alert.triggered`, `alert.empty`, `alert.note`, `alert.inactive`, `alert.refired`, `alert.resolved`, `alert.acknowledged`) as it always does. Omni-channel alerts is part of Ovok and changes nothing in Signals.
- Ovok, with `OMNICHANNEL_ALERTS_ENABLED` on, subscribes to the project tenant's alert events and runs the project's rule (JSON Logic, stored with the project) on each one. It tells the people the rule names, over the channels it names, by email and push. It keeps no copy of the alert.
- Say: "Signals sends alert events. Omni-channel alerts in Ovok tells the people your rule names, over the channels it names." Never "Signals notifies", and say nothing clinical or safety-related about Ovok's part.
- An app or backend that wants delivery of its own (SMS, pager, chat) takes Signals' alert webhooks directly. Never poll alerts to send messages from app code, and never evaluate the rule's conditions in app code.

## Set it up

1. The project needs a Signals tenant of its own; shared-tenant projects cannot use it.
2. Switch on `OMNICHANNEL_ALERTS_ENABLED` (`PUT /v1/project/settings/OMNICHANNEL_ALERTS_ENABLED`). It is off by default.
3. Save a rule with `PUT /v1/omnichannel-alerts/rules` (admin only, body `{ "logic": ... }`). `GET` answers `{ "notifications": { "logic" } | null }`, `DELETE` answers `204`. Without a rule, nobody is told.
4. Record recipients in FHIR: `Patient.generalPractitioner` or record shares (`practitioner`), active `CareTeam` participants (`caregiver`), `RelatedPerson` (`relatedPerson`), the people the patient shares readings with (`sharedWith`).
5. Map the `ALERT_NOTIFICATION` (practitioners) and `PATIENT_ALERT_NOTIFICATION` (related persons and patients; falls back to `ALERT_NOTIFICATION`) email templates. Params: `firstName`, `lastName`, `patientName`, `alertedAt`, `event`, `alertsUrl`, plus the common ones. For push, set the project's push provider and register each signed-in user's device token.
6. Check who a rule reaches with `POST /v1/omnichannel-alerts/preview` before relying on it.

## The rule

- The rule answers zero to 10 entries `{ "notify": { "channels": [...], "to": [...] } }`. There is no timing field.
- Channels: `push`, `email`. Recipients: `practitioner`, `caregiver`, `relatedPerson`, `sharedWith`.
- Variables: `event`, `alert.id`, `alert.code`, `alert.reason`, `alert.message`, `alert.value`, `patient.id`.
- Example: `{"logic":{"if":[{"==":[{"var":"event"},"alert.triggered"]},{"notify":{"channels":["push","email"],"to":["practitioner","caregiver"]}},[]]}}`.
- Preview body: `{ patientId, event? (default alert.triggered), alert?: { reason?, code?, message?, value? }, notifications? }`. It answers `{ notify: [{ channels, to, recipients: [{ reference, display, email }], unknownChannels, unknownRoles }] }` and sends nothing.
- MCP tools: `get_alert_routing_rule`, `set_alert_routing_rule`, `delete_alert_routing_rule`, `preview_alert_routing_rule`, `get_push_provider`.

## Errors

- `422 UNKNOWN_NOTIFY_LABELS`: an entry names a channel or recipient Ovok does not know.
- `422 INVALID_NOTIFICATIONS`: the rule fails on some event type; the message names it.
- `409`: the project changed during the write; repeat it.

## Delivery

- A claim per send stops duplicates when Signals retries. A rule that fails tells nobody of that event and is logged.
- A person without an email address or a registered device is skipped on that channel.
- Push `data` is `{ type: 'ovok.alert', alertId, patientId, event }`; the lock-screen words contain no name or value.

## Verify

- `GET /v1/omnichannel-alerts/rules` returns the rule you saved.
- A preview lists every expected person, with an `email` where an email should go.
- In the sandbox, a breaching reading delivers to the people the rule names for `alert.triggered`.
