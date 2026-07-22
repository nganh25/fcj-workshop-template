---
title: "Prerequisites"
date: 2026-07-22
weight: 2
chapter: false
pre: "<b>5.2. </b>"
---

# Prerequisites

## Tools

```powershell
node --version
npm --version
git --version
aws --version
sam --version
```

Install Node.js, npm, Git, AWS CLI v2, AWS SAM CLI, and Visual Studio Code.

## AWS Configuration

```powershell
aws configure
aws sts get-caller-identity
```

Recommended Region:

```text
ap-southeast-1
```

## Run the Project Locally

Backend:

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend
npm install
node server.js
```

Frontend:

```powershell
cd C:\Users\nguye\smart-attendance-saas\frontend
npm install
npm run dev
```

## Validate SAM

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend
sam validate --lint
sam build
```
