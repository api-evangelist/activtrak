---
generated: '2026-07-17'
method: generated
name: Pull activity and working-hours reports
description: Query the ActivTrak Live Data (Reports) API for activity-log and working-hours data, handling cursor and page pagination.
api: openapi/activtrak-openapi-original.yaml
operations: [getActivityLog, getWorkingHoursForUsers, getWorkingHoursForComputers]
source: >-
  operationIds verified verbatim in openapi/activtrak-openapi-original.yaml
  (paths /reports/v2/activitylog, /reports/v2/workinghours/users,
  /reports/v2/workinghours/computers).
---

# Pull activity and working-hours reports

Read digital-activity data from the Reports (Live Data) API. Data appears near real-time as agents report in; visible data follows the API key user's permissions.

## Auth
- API key in the `x-api-key` header. See `authentication/activtrak-authentication.yml`.

## Steps
1. **Scope the query** — supply exactly one of `clientId`, `groupId`, or `deviceId` plus a `startDate`/`endDate` range (YYYY-MM-DD). Supplying more than one scope returns 400.
2. **Working hours (users)** — `getWorkingHoursForUsers` (`GET /reports/v2/workinghours/users`) returns Working Hour Activity objects plus a `total`. Page with `page`/`pageSize` (default 150, max 1000).
3. **Working hours (computers)** — `getWorkingHoursForComputers` (`GET /reports/v2/workinghours/computers`) for the computer-perspective view; same page-based pagination.
4. **Activity log** — `getActivityLog` (`GET /reports/v2/activitylog`) returns Activity Log objects with cursor-based pagination; pass the returned `cursor` to page forward within the bounded date range.

## Errors
- 400 = invalid date range or more than one of clientId/groupId/deviceId. 401 = bad key. See `errors/activtrak-problem-types.yml`.

## Notes
- Working Hours uses page-based pagination; Activity Log uses cursor-based. ActivTrak plans to converge on cursor-based, at which point page-based requests will be deprecated (see `lifecycle/activtrak-lifecycle.yml`).
