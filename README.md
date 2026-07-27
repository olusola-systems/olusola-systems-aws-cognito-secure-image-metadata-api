# olusola-systems-aws-cognito-secure-image-metadata-api

# Securing an Image Metadata API with Amazon Cognito

## Overview

This project focused on securing a serverless Image Metadata API using Amazon Cognito and OAuth 2.0. Instead of exposing the API publicly, I implemented user authentication and JWT-based authorization so that only authenticated users could access protected endpoints.

The implementation included configuring an Amazon Cognito User Pool, enabling the Managed Login experience, implementing the OAuth 2.0 Authorization Code Grant flow, and integrating API Gateway with a JWT Authorizer. By the end of the project, I successfully completed the authentication flow, verified that unauthenticated requests were rejected, and confirmed that authenticated users could obtain an authorization code for secure API access.

---

## AWS Services Used

- Amazon Cognito
- Amazon API Gateway (HTTP API)
- AWS Lambda
- Amazon DynamoDB
- Amazon S3
- AWS Identity and Access Management (IAM)

---

## Architecture

<img width="2880" height="1800" alt="Architecture" src="https://github.com/user-attachments/assets/8d87758e-72c6-48f0-ac5a-e7ce76b7073c" />


---

## What I Built

**Amazon Cognito User Pool**

Created an Amazon Cognito User Pool to provide centralized identity management for the application. The User Pool authenticates users and issues JWT tokens that are validated by API Gateway before requests reach the backend.

Configured:
- User Pool
- App Client
- Managed Login
- OAuth 2.0
- Authorization Code Grant

---

**OAuth Configuration**

Configured OAuth 2.0 using the Authorization Code Grant flow to enable secure authentication without exposing user credentials.

Configured:
- Authorization Code Grant
- Callback URL
- Default Redirect URL
- Sign-out URL
- OpenID Connect Scopes
- Email Scope
- Phone Scope

Successfully configured the authorization flow required for secure API access.

---

**Managed Login**

Configured Amazon Cognito Managed Login to provide a secure, hosted authentication experience for application users.

Configured:
- Managed Login
- Authentication Provider
- Redirect Configuration
- OAuth Client Integration

Verified that users could successfully authenticate through the Managed Login page.

---

**JWT Authorization**

Protected the Image Metadata API by attaching a JWT Authorizer to API Gateway.

Instead of allowing anonymous requests, API Gateway now validates JWT access tokens before forwarding requests to the Lambda function.

Unauthenticated requests correctly returned:

```json
{
  "message": "Unauthorized"
}
```

confirming that authentication was being enforced before backend execution.

---

**Authorization Flow**

Successfully completed the OAuth 2.0 Authorization Code flow.

After authentication, Amazon Cognito redirected the application with a valid authorization code, confirming that the authentication workflow was correctly configured and ready for token exchange.

---

## Challenges

**Challenge 1 — Managed Login Page Was Unavailable**

Problem: The Managed Login page displayed a "Login pages unavailable" message, preventing users from signing in.

Cause: Although the User Pool and App Client had already been created, no Managed Login branding style had been assigned to the application client. Without an assigned branding style, Cognito could not render the Managed Login interface.

Resolution: Created a Managed Login branding style, assigned it to the correct App Client, and verified that the login page became available.

---

**Challenge 2 — OAuth Redirect Was Not Completing**

Problem: After successfully signing in, Cognito displayed a confirmation page instead of redirecting users back to the application.

Cause: The callback URL and default redirect URL were incomplete, preventing Cognito from determining where to redirect users after authentication.

Resolution: Configured the callback URL, default redirect URL, and sign-out URL consistently within the App Client configuration. Once updated, Cognito successfully redirected with an authorization code.

---

**Challenge 3 — API Requests Returned "Unauthorized"**

Problem: Requests sent directly to the protected API consistently returned an Unauthorized response.

Cause: Initially, this appeared to be an API configuration issue. After investigation, I confirmed that the JWT Authorizer was functioning correctly and rejecting requests because no valid JWT access token had been supplied.

Resolution: Verified that API Gateway was enforcing authentication exactly as intended. This confirmed that the security layer was functioning correctly and that the remaining work was to exchange the authorization code for JWT tokens before invoking the API.

---

**Challenge 4 — Tracing the OAuth Authentication Flow**

Problem: Authentication involved multiple AWS services, making it difficult to determine exactly where failures were occurring.

Cause: An OAuth authentication request passes through Amazon Cognito, redirect URIs, the client application, API Gateway, and JWT validation. Since each component depends on the previous stage, failures can appear to originate from the wrong service.

Resolution: Validated each stage independently by checking the Cognito domain, Managed Login configuration, OAuth settings, redirect behavior, and API Gateway authorization. Breaking the authentication workflow into smaller validation steps made troubleshooting significantly more efficient.

---

## Lessons Learned

This project reinforced that authentication is an end-to-end workflow rather than a single AWS service. Successfully securing the API required understanding how Amazon Cognito, OAuth 2.0, API Gateway, and JWT validation work together throughout the authentication lifecycle.

I also gained a much deeper appreciation for callback and redirect URIs. Small configuration differences can interrupt the OAuth flow even when every other service is configured correctly. Carefully validating redirect settings became one of the most effective troubleshooting techniques during this project.

Another important lesson was learning to interpret security responses before assuming something was broken. Receiving an "Unauthorized" response initially appeared to indicate a failure, but it actually confirmed that API Gateway was correctly enforcing JWT authentication. Recognizing expected security behavior prevented unnecessary debugging of components that were already functioning correctly.

Finally, this project reinforced the value of structured troubleshooting. Instead of changing multiple configurations simultaneously, validating each stage of the authentication flow independently made it easier to isolate issues, understand service interactions, and confidently complete the implementation.

---

## Key Takeaways

- Authentication and authorization solve different problems and must work together to properly secure APIs.
- OAuth 2.0 depends heavily on correctly configured callback and redirect URIs.
- An Unauthorized response often confirms that security controls are functioning correctly.
- Breaking distributed authentication workflows into smaller validation steps makes troubleshooting more efficient.
- Understanding how Amazon Cognito integrates with API Gateway is essential when building secure serverless applications.

---

## Skills Demonstrated

- Amazon Cognito
- OAuth 2.0 Authorization Code Grant
- API Gateway JWT Authorizers
- JWT Authentication
- HTTP APIs
- AWS Lambda
- Amazon DynamoDB
- AWS IAM
- Identity Management
- Secure API Design
- Authentication Workflow Troubleshooting

---

## Screenshots

### Amazon Cognito User Pool Overview
<img width="2880" height="1800" alt="User Pool Overview" src="https://github.com/user-attachments/assets/6b41882b-9f1d-47a6-bc34-b92608160ff7" />

### App Client Configuration
<img width="2880" height="1800" alt="client configuration" src="https://github.com/user-attachments/assets/03f8d58b-a356-442d-9444-cbc9bf03e4c0" />

### Managed Login Configuration
<img width="2880" height="1800" alt="Managed Login Configuration" src="https://github.com/user-attachments/assets/e9320757-0d8a-470b-9558-f51262477399" />

### Managed Login Page
<img width="2880" height="1800" alt="Managed Login Page" src="https://github.com/user-attachments/assets/25858f70-442b-4f80-9d35-2631704e9c15" />

### Successful Authorization Code Redirect
<img width="2880" height="1800" alt="Successful Authorization Code Redirect" src="https://github.com/user-attachments/assets/64a761ce-daa8-4456-b2dd-32358db060db" />

### API Gateway JWT Authorizer
<img width="2880" height="1800" alt="API Gateway JWT Authorizer" src="https://github.com/user-attachments/assets/40f190e3-bf57-4fb6-95e5-fc0d5782cfba" />

### Unauthorized API Response
<img width="2880" height="1800" alt="Unauthorized API Response" src="https://github.com/user-attachments/assets/03cb9e29-23df-4f23-8879-98b742cdc8f6" />
---

## Outcome

Successfully transformed a publicly accessible serverless Image Metadata API into an authenticated service protected by Amazon Cognito and API Gateway JWT authorization. The completed authentication workflow provides a secure foundation for token-based API access and future frontend integration.

---

## Next Project

Week 22 — Completing the OAuth Token Exchange

The next phase of this project will focus on exchanging the authorization code for JWT tokens using Amazon Cognito's token endpoint. After obtaining an access token, the client application will use it to invoke protected API Gateway endpoints, completing the end-to-end authentication workflow from user sign-in to authorized API access.
