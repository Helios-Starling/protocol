# Error Codes Specification
**Version:** 1.0.0  
**Status:** Draft  
**Last Updated:** 2024-01-08

## 1. Overview

This document specifies the standard error codes used in the Helios-Starling protocol. Implementations MUST support these error codes and MAY add their own custom codes following the specified format.

## 2. Error Response Format

All error responses follow this structure:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable description",
    "details": {                    // Optional
      "field": "additional data"
    }
  }
}
```

## 3. Error Code Format

Error codes MUST:
- Be in UPPER_SNAKE_CASE
- Be purely ASCII
- Not exceed 63 characters
- Start with a category prefix

## 4. Standard Error Categories

### 4.1. Protocol Errors (PROTOCOL_*)

| Code | Description | Details Field |
|------|-------------|---------------|
| `PROTOCOL_INVALID_MESSAGE` | Message does not conform to protocol specification | `{ "violations": [] }` |
| `PROTOCOL_VERSION_MISMATCH` | Protocol version not compatible | `{ "supported": [], "received": "" }` |
| `PROTOCOL_INVALID_TYPE` | Invalid message type | `{ "received": "", "expected": [] }` |
| `PROTOCOL_SCHEMA_VIOLATION` | Message schema validation failed | `{ "errors": [] }` |

### 4.2. Method Errors (METHOD_*)

| Code | Description | Details Field |
|------|-------------|---------------|
| `METHOD_NOT_FOUND` | Requested method does not exist | `{ "method": "" }` |
| `METHOD_INVALID_NAME` | Method name format is invalid | `{ "reason": "" }` |
| `METHOD_EXECUTION_ERROR` | Error during method execution | `{ "reason": "" }` |
| `METHOD_TIMEOUT` | Method execution exceeded timeout | `{ "timeout": 0 }` |
| `METHOD_NOT_ALLOWED` | Method execution not permitted | `{ "reason": "" }` |

### 4.3. Request Errors (REQUEST_*)

| Code | Description | Details Field |
|------|-------------|---------------|
| `REQUEST_INVALID_ID` | Invalid request ID format | `{ "received": "" }` |
| `REQUEST_DUPLICATE` | Request ID already in use | `{ "requestId": "" }` |
| `REQUEST_CANCELLED` | Request was cancelled | `{ "reason": "" }` |
| `REQUEST_TIMEOUT` | Request timed out waiting for response | `{ "timeout": 0 }` |
| `REQUEST_PAYLOAD_TOO_LARGE` | Request payload exceeds size limit | `{ "size": 0, "limit": 0 }` |

### 4.4. Connection Errors (CONNECTION_*)

| Code | Description | Details Field |
|------|-------------|---------------|
| `CONNECTION_ERROR` | Generic connection error | `{ "reason": "" }` |
| `CONNECTION_CLOSED` | Connection was closed | `{ "code": 0, "reason": "" }` |
| `CONNECTION_TIMEOUT` | Connection timed out | `{ "timeout": 0 }` |
| `CONNECTION_LIMIT` | Server connection limit reached | `{ "limit": 0 }` |
| `CONNECTION_REJECTED` | Connection rejected by server | `{ "reason": "" }` |

### 4.5. State Errors (STATE_*)

| Code | Description | Details Field |
|------|-------------|---------------|
| `STATE_TOKEN_INVALID` | Invalid recovery token | `{ "reason": "" }` |
| `STATE_TOKEN_EXPIRED` | Recovery token has expired | `{ "expiredAt": 0 }` |
| `STATE_RECOVERY_FAILED` | State recovery failed | `{ "reason": "" }` |
| `STATE_NOT_FOUND` | No state found for recovery | `{ "tokenId": "" }` |
| `STATE_CORRUPTED` | State data is corrupted | `{ "reason": "" }` |

### 4.6. Authorization Errors (AUTH_*)

| Code | Description | Details Field |
|------|-------------|---------------|
| `AUTH_REQUIRED` | Authentication is required | `{ "reason": "" }` |
| `AUTH_FAILED` | Authentication failed | `{ "reason": "" }` |
| `AUTH_EXPIRED` | Authentication has expired | `{ "expiredAt": 0 }` |
| `AUTH_INVALID_TOKEN` | Invalid authentication token | `{ "reason": "" }` |
| `AUTH_INSUFFICIENT_SCOPE` | Insufficient permissions | `{ "required": [], "provided": [] }` |

### 4.7. Rate Limit Errors (RATE_*)

| Code | Description | Details Field |
|------|-------------|---------------|
| `RATE_LIMIT_EXCEEDED` | Rate limit exceeded | `{ "limit": 0, "reset": 0 }` |
| `RATE_QUOTA_EXCEEDED` | Usage quota exceeded | `{ "quota": 0, "reset": 0 }` |

## 5. Examples

### 5.1. Protocol Error

```json
{
  "success": false,
  "error": {
    "code": "PROTOCOL_INVALID_MESSAGE",
    "message": "Message does not conform to protocol specification",
    "details": {
      "violations": [
        "missing required field 'timestamp'",
        "invalid message type"
      ]
    }
  }
}
```

### 5.2. Method Error

```json
{
  "success": false,
  "error": {
    "code": "METHOD_EXECUTION_ERROR",
    "message": "Failed to fetch user profile",
    "details": {
      "reason": "database connection failed"
    }
  }
}
```

### 5.3. Authorization Error

```json
{
  "success": false,
  "error": {
    "code": "AUTH_INSUFFICIENT_SCOPE",
    "message": "Insufficient permissions to access this resource",
    "details": {
      "required": ["admin:read", "admin:write"],
      "provided": ["admin:read"]
    }
  }
}
```

## 6. Custom Error Codes

Implementations MAY define custom error codes following these rules:

1. MUST start with a custom prefix (e.g., `APP_*`)
2. MUST follow the same UPPER_SNAKE_CASE format
3. SHOULD include meaningful details
4. SHOULD be documented

Example custom error:

```json
{
  "success": false,
  "error": {
    "code": "APP_INVALID_DATE_FORMAT",
    "message": "Invalid date format in booking request",
    "details": {
      "field": "startDate",
      "received": "2024-13-45",
      "expected": "YYYY-MM-DD"
    }
  }
}
```

## 7. Implementation Guidelines

### 7.1. Error Creation

Implementations SHOULD:
1. Provide helper functions for creating standard errors
2. Include stack traces in development mode
3. Sanitize sensitive information in error details
4. Log errors appropriately

### 7.2. Error Handling

Implementations SHOULD:
1. Have consistent error handling across the application
2. Translate internal errors to protocol errors
3. Include contextual information when relevant
4. Maintain error tracking and monitoring

---

## Appendix A: Changes

### Version 1.0.0
- Initial error codes specification