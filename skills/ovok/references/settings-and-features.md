# Project settings and features

Read the current [Settings and features docs](https://docs.ovok.com/settings-and-features/llms.txt) and the page for each exact key before making a configuration change. A **feature** enables a capability for the whole project; a **setting** controls a flow or value. Names alone do not prove a key is active.

## Feature list writes replace the full list

`GET /v1/projects/me/features` returns `{ "features": [...] }`. `PATCH /v1/projects/me/features` replaces that entire list. To enable or disable a feature, read the current list immediately before writing, preserve every feature that should remain enabled, and send the complete intended list. Never send only the requested delta.

Concurrent admin updates can still overwrite each other: the endpoint's `409` protection only covers overlapping writes within the same request. If a change races with another admin, reread the list and resolve the intended complete state before retrying.

## Settings read and write formats

- Read both records from singular `GET /v1/project/settings`: `settings` contains Booleans and `values` contains text values.
- Write one Boolean at a time to `PUT /v1/project/settings/:key` with `{"enabled": true}` or `{"enabled": false}`.
- Write one text value to `PUT /v1/project/settings/values/:key` with `{"value": "..."}`. Use `{"value": null}` to remove the stored text value.
- Confirm the exact setting key, payload, actor role, prerequisites, and effect from its current reference page.

## Unset values and runtime behavior

An unset Boolean is shown as `false` by the settings response even when its flow runs as enabled. Current documented examples include patient/practitioner login, invitations, and patient registration (subject to their required policy); practitioner self-registration is disabled when unset. The response does not distinguish an explicitly stored `false` from an unset value. Set the intended value explicitly when the difference matters.

Do not assume a setting is an authorization policy. Settings gate flows; AccessPolicies control what signed-in members can do. Check both when a workflow involves registration, invitations, membership, or patient data.

## Project inheritance

The settings response shows values stored on the current project, not necessarily the resolved runtime value. Boolean settings do not generally inherit. Patient/practitioner app URL text values can fall back to a parent project for generated links; a `null` in the child project's response does not alone prove that no URL resolves. Default AccessPolicies are project-local and are not retroactive to existing memberships. Verify the exact key's inheritance and scope before changing it.
