---
generated: '2026-07-17'
method: generated
name: Manage app access (consumers)
description: List, create, patch, and remove ActivTrak app users (consumers) and their role/group access.
api: openapi/activtrak-openapi-original.yaml
operations: [getAllConsumers, getConsumer, createConsumers, patchConsumers, deleteConsumers]
source: >-
  operationIds verified verbatim in openapi/activtrak-openapi-original.yaml
  (paths /admin/v1/consumers, /admin/v1/consumers/{id}). Consumers API is Early Access.
---

# Manage app access (consumers)

Consumers are ActivTrak app users who view reports or administer the account (the same users under Settings > Access > App Access). This API is Early Access.

## Auth
- API key in the `x-api-key` header. See `authentication/activtrak-authentication.yml`.

## Steps
1. **List consumers** — `getAllConsumers` (`GET /admin/v1/consumers`); pass `includeViewableGroups=true` to expand each consumer's group access. Admin/Configurator roles carry `groupid -1` (All Users and Groups, current and future).
2. **Inspect one** — `getConsumer` (`GET /admin/v1/consumers/{id}`) for a single consumer's roles and viewableGroups.
3. **Create** — `createConsumers` (`POST /admin/v1/consumers`) with a list of consumers; exactly one role per consumer from {Admin, Configurator, PowerUser, Viewer}. PowerUser/Viewer may be scoped to specific `groupid`/`groupname` or to `-1` for all.
4. **Patch** — `patchConsumers` (`PATCH /admin/v1/consumers`) to change any of `viewableGroups`, `roles`, `ssoEnabled`; identify each by `id` or `username`.
5. **Delete** — `deleteConsumers` (`DELETE /admin/v1/consumers`) with `consumerIds` and/or `usernames` (at least one required).

## Errors
- Batch calls return a per-item list of success / failures / warnings. 400 on invalid role combinations. See `errors/activtrak-problem-types.yml`.

## Notes
- Batch create/patch/delete are list-based; inspect the returned per-item status rather than relying on the HTTP status alone. See `conventions/activtrak-conventions.yml`.
