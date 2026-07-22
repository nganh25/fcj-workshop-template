---
title: "Xây dựng Backend bằng AWS SAM"
date: 2026-07-22
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

# Xây dựng Backend bằng AWS SAM

## 1. DynamoDB Table

```yaml
AttendanceTable:
  Type: AWS::DynamoDB::Table
  Properties:
    BillingMode: PAY_PER_REQUEST
    AttributeDefinitions:
      - AttributeName: PK
        AttributeType: S
      - AttributeName: SK
        AttributeType: S
    KeySchema:
      - AttributeName: PK
        KeyType: HASH
      - AttributeName: SK
        KeyType: RANGE
    PointInTimeRecoverySpecification:
      PointInTimeRecoveryEnabled: true
```

## 2. Lambda Runtime

```yaml
Globals:
  Function:
    Runtime: nodejs22.x
    Architectures:
      - arm64
    MemorySize: 128
    Timeout: 10
    Environment:
      Variables:
        TABLE_NAME: !Ref AttendanceTable
        TENANT_ID: demo
```

## 3. Check-in

```javascript
const now = new Date();
const date = now.toISOString().slice(0, 10);

await documentClient.send(new PutCommand({
  TableName: process.env.TABLE_NAME,
  Item: {
    PK: `TENANT#${process.env.TENANT_ID}`,
    SK: `ATTENDANCE#${userSub}#${date}`,
    userSub,
    email,
    attendanceDate: date,
    checkInAt: now.toISOString()
  },
  ConditionExpression: "attribute_not_exists(PK) AND attribute_not_exists(SK)"
}));
```

Nếu `ConditionalCheckFailedException`, trả `409 Conflict`.

## 4. Check-out

```javascript
await documentClient.send(new UpdateCommand({
  TableName: process.env.TABLE_NAME,
  Key: {
    PK: `TENANT#${process.env.TENANT_ID}`,
    SK: `ATTENDANCE#${userSub}#${date}`
  },
  UpdateExpression: "SET checkOutAt = :value, updatedAt = :value",
  ConditionExpression: "attribute_exists(PK) AND attribute_exists(SK)",
  ExpressionAttributeValues: { ":value": now.toISOString() },
  ReturnValues: "ALL_NEW"
}));
```

## 5. History

```javascript
const result = await documentClient.send(new QueryCommand({
  TableName: process.env.TABLE_NAME,
  KeyConditionExpression: "PK = :pk AND begins_with(SK, :prefix)",
  ExpressionAttributeValues: {
    ":pk": `TENANT#${process.env.TENANT_ID}`,
    ":prefix": `ATTENDANCE#${userSub}#`
  },
  ScanIndexForward: false,
  Limit: 31
}));
```

## 6. IAM

```yaml
Policies:
  - DynamoDBCrudPolicy:
      TableName: !Ref AttendanceTable
```

Hàm history chỉ cần `DynamoDBReadPolicy`.

## 7. Build

```powershell
cd backend\src
npm install @aws-sdk/client-dynamodb @aws-sdk/lib-dynamodb
cd ..
sam validate --lint
sam build
```
