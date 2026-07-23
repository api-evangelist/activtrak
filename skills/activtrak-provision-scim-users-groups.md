---
generated: '2026-07-17'
method: generated
name: Provision users and groups via SCIM
description: Create, update, and deactivate ActivTrak users and groups through the SCIM 2.0 Administration API.
api: openapi/activtrak-openapi-original.yaml
operations: [getUsers, createUser, updateUser, deleteUser, getGroups, createGroup, updateGroup, deleteGroup]
source: >-
  operationIds verified verbatim in openapi/activtrak-openapi-original.yaml
  (paths /scim/v1/users, /scim/v1/users/{userId}, /scim/v1/groups,
  /scim/v1/groups/{groupId}).
---

# Provision users and groups via SCIM

Manage ActivTrak identities with the SCIM 2.0 surface of the Administration API (Beta). A special-use API key is required for the SCIM endpoints — request one from integrations-feedback@activtrak.com.

## Auth
- API key in the `x-api-key` header (Bearer form also accepted). See `authentication/activtrak-authentication.yml`.

## Steps
1. **List existing users** — `getUsers` (`GET /scim/v1/users`) to inventory current SCIM users; filter by `userName` when reconciling against your IdP.
2. **Create a user** — `createUser` (`POST /scim/v1/users`) with the SCIM User resource (`userName`, `emails`, enterprise/employeeNumber extension). If the user already exists, ActivTrak updates the existing record.
3. **Replace a user** — `updateUser` (`PUT /scim/v1/users/{userId}`) to overwrite a user with the provided entity (display name, employee number, active/tracking state).
4. **Create a group** — `createGroup` (`POST /scim/v1/groups`) with `displayName`; then set membership via `updateGroup` (`PUT /scim/v1/groups/{groupId}`) supplying `members[].value` = user ids.
5. **List groups** — `getGroups` (`GET /scim/v1/groups`) to confirm membership.
6. **Deprovision** — `deleteUser` (`DELETE /scim/v1/users/{userId}`) adds the user to the Do Not Track list and deletes their agents; `deleteGroup` (`DELETE /scim/v1/groups/{groupId}`) removes a group.

## Errors
- Plain-JSON error message envelope (not RFC 9457). 400 = invalid SCIM payload; 401 = missing/invalid key; 404 = unknown userId/groupId. See `errors/activtrak-problem-types.yml`.

## Notes
- No `Idempotency-Key` header; mutations are keyed on resource id (`updateUser`/`updateGroup` are safe to retry). See `conventions/activtrak-conventions.yml`.
- SCIM schemas retrievable via `getSchemas` / `getSchema` (core, enterprise, and custom `urn:ietf:params:scim:schemas:extension:activtrak:2.0` extensions).
