---
title: "Week 8 Worklog"
date: 2026-06-08
weight: 8
chapter: false
pre: "<b>1.8. </b>"
---

# Week 8 Worklog

### Objectives for Week 8

- Deploy API Gateway HTTP API v2.
- Configure a Cognito JWT authorizer.
- Develop business Lambda functions.
- Connect the backend to DynamoDB.

### Daily tasks

| Day | Task | Start date | Completion date | Reference |
|---|---|---:|---:|---|
| Monday | Design routes for clock-in, clock-out, attendance, profile, and admin functions. | 2026-06-08 | 2026-06-08 | AWS documentation and project materials |
| Tuesday | Configure the JWT authorizer and CORS. | 2026-06-09 | 2026-06-09 | AWS documentation and project materials |
| Wednesday | Build the clock-in Lambda and prevent duplicate records. | 2026-06-10 | 2026-06-10 | AWS documentation and project materials |
| Thursday | Build clock-out, attendance, and profile-update functions. | 2026-06-11 | 2026-06-11 | AWS documentation and project materials |
| Friday | Test the frontend-to-API-Gateway-to-Lambda-to-DynamoDB flow. | 2026-06-12 | 2026-06-12 | AWS documentation and project materials |

### Outcomes after Week 8

- Implemented the business APIs.
- Blocked requests without valid tokens.
- Stored and queried tenant-aware data.
- Completed the synchronous attendance flow.
