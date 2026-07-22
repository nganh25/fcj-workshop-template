---
title: "Tạo xác thực Amazon Cognito"
date: 2026-07-22
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---

# Tạo xác thực Amazon Cognito

User Pool và App Client được khai báo trong `template.yaml`, vì vậy chúng được tạo cùng Backend khi chạy `sam deploy`.

## 1. User Pool

```yaml
AttendanceUserPool:
  Type: AWS::Cognito::UserPool
  Properties:
    UserPoolName: !Sub "${AWS::StackName}-users"
    UsernameAttributes:
      - email
    AutoVerifiedAttributes:
      - email
    Policies:
      PasswordPolicy:
        MinimumLength: 8
        RequireUppercase: true
        RequireLowercase: true
        RequireNumbers: true
        RequireSymbols: true
    Schema:
      - Name: email
        Required: true
        Mutable: true
```

## 2. App Client

```yaml
AttendanceUserPoolClient:
  Type: AWS::Cognito::UserPoolClient
  Properties:
    ClientName: !Sub "${AWS::StackName}-web-client"
    UserPoolId: !Ref AttendanceUserPool
    GenerateSecret: false
    PreventUserExistenceErrors: ENABLED
    ExplicitAuthFlows:
      - ALLOW_USER_PASSWORD_AUTH
      - ALLOW_USER_SRP_AUTH
      - ALLOW_REFRESH_TOKEN_AUTH
```

Không tạo Client Secret vì React SPA không có nơi an toàn để giữ bí mật phía Client.

## 3. JWT Authorizer

```yaml
AttendanceApi:
  Type: AWS::Serverless::HttpApi
  Properties:
    StageName: Prod
    Auth:
      Authorizers:
        CognitoJwtAuthorizer:
          IdentitySource: "$request.header.Authorization"
          JwtConfiguration:
            issuer: !Sub "https://cognito-idp.${AWS::Region}.amazonaws.com/${AttendanceUserPool}"
            audience:
              - !Ref AttendanceUserPoolClient
```

Gắn vào route nghiệp vụ:

```yaml
Auth:
  Authorizer: CognitoJwtAuthorizer
```

Lambda lấy danh tính từ:

```javascript
const claims = event.requestContext?.authorizer?.jwt?.claims;
const userSub = claims?.sub;
const email = claims?.email;
```
