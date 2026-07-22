---
title: "Week 9 Worklog"
date: 2026-06-15
weight: 9
chapter: false
pre: "<b>1.9. </b>"
---

# Week 9 Worklog

### Objectives for Week 9

- Finalize the DynamoDB single-table design.
- Build report export functions.
- Implement asynchronous processing.
- Create an event-driven email flow.

### Daily tasks

| Day | Task | Start date | Completion date | Reference |
|---|---|---:|---:|---|
| Monday | Optimize partition/sort keys and attendance queries. | 2026-06-15 | 2026-06-15 | AWS documentation and project materials |
| Tuesday | Build report and CSV/Excel export Lambdas. | 2026-06-16 | 2026-06-16 | AWS documentation and project materials |
| Wednesday | Store reports in S3 by tenant. | 2026-06-17 | 2026-06-17 | AWS documentation and project materials |
| Thursday | Create Step Functions, SQS, and a DLQ for long-running tasks. | 2026-06-18 | 2026-06-18 | AWS documentation and project materials |
| Friday | Connect DynamoDB Streams, EventBridge, an email worker, and SES. | 2026-06-19 | 2026-06-19 | AWS documentation and project materials |

### Outcomes after Week 9

- Completed report generation and S3 storage.
- Understood retries and dead-letter queues.
- Built a basic event-driven flow.
- Differentiated synchronous and asynchronous processing.
