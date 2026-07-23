---
generated: '2026-07-17'
method: generated
name: Bulk-import HR user data
description: Submit a CSV of HR user data to ActivTrak's HR Data Connector for automated user and group management.
api: openapi/activtrak-openapi-original.yaml
operations: [importUserData]
source: >-
  operationId verified verbatim in openapi/activtrak-openapi-original.yaml
  (path /hrdc/v2/bulk). Bulk Import (HR Data Connector) is Beta.
---

# Bulk-import HR user data

The Bulk Import API (HR Data Connector, HRDC) ingests a comma-separated user file to drive automated user management: create/update users, create/update groups, add/remove group membership, add users to Do Not Track, and delete users. Beta surface.

## Auth
- API key in the `x-api-key` header. See `authentication/activtrak-authentication.yml`.

## Steps
1. **Prepare the CSV** — build the HR data file per the HRDC template. Contact integrations-feedback@activtrak.com for the setup guide and sample templates.
2. **Submit** — `importUserData` (`POST /hrdc/v2/bulk`) with the CSV payload. ActivTrak applies the described user/group mutations from the file contents.

## Errors
- 400 on malformed CSV or invalid rows; 401 on bad key. Plain-JSON error message (not RFC 9457). See `errors/activtrak-problem-types.yml`.

## Notes
- This is a batch, file-driven surface — treat it as the system of record sync from your HR/IdP rather than a per-user call. Complements the SCIM provisioning skill.
