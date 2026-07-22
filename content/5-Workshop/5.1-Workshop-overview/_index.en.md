---
title: "Architecture and Source-Code Overview"
date: 2026-07-22
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---

# Architecture and Source-Code Overview

## Problem

Smart Attendance SaaS manages users, clock-in and clock-out, attendance history, profiles, and reports for multiple organizations.

## Main Flow

```text
Route 53 → CloudFront → S3 React SPA
                         │
                    Amazon Cognito
                         │ JWT
                         ▼
                  API Gateway HTTP API
                         │
                         ▼
                      Lambda
                         │
                         ▼
                     DynamoDB
```

## Source Structure

```text
smart-attendance-saas
├── backend
│   ├── template.yaml
│   ├── buildspec.yml
│   ├── server.js
│   └── src
│       ├── handler
│       ├── shared
│       └── emailWorker.js
└── frontend
    └── src
        ├── context/AuthContext.jsx
        └── utils/api.js
```

`template.yaml` defines infrastructure; `server.js` supports local execution; handlers implement business logic; `AuthContext.jsx` manages authentication state; and `api.js` sends API requests with JWTs.
