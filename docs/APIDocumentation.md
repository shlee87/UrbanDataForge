# API Documentation

## 1. Overview
### 1.1 Base URL
```
https://api.example.com/v1
```

### 1.2 Authentication
- Method: Bearer Token
- Header: `Authorization: Bearer <token>`
- Token Format: JWT

### 1.3 Rate Limiting
- Requests per minute: 60
- Response Header: `X-RateLimit-Remaining`

## 2. Endpoints

### 2.1 Users
#### GET /users
Retrieve a list of users.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 10) |
| sort | string | No | Sort field (default: "created_at") |

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "email": "string",
      "created_at": "string"
    }
  ],
  "meta": {
    "total": "integer",
    "page": "integer",
    "limit": "integer"
  }
}
```

#### POST /users
Create a new user.

**Request Body:**
```json
{
  "name": "string",
  "email": "string",
  "password": "string"
}
```

**Response:**
```json
{
  "data": {
    "id": "string",
    "name": "string",
    "email": "string",
    "created_at": "string"
  }
}
```

### 2.2 Authentication
#### POST /auth/login
Authenticate a user.

**Request Body:**
```json
{
  "email": "string",
  "password": "string"
}
```

**Response:**
```json
{
  "token": "string",
  "user": {
    "id": "string",
    "name": "string",
    "email": "string"
  }
}
```

## 3. Error Handling
### 3.1 Error Response Format
```json
{
  "error": {
    "code": "string",
    "message": "string",
    "details": {}
  }
}
```

### 3.2 Common Error Codes
| Code | Description |
|------|-------------|
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Too Many Requests |
| 500 | Internal Server Error |

## 4. Data Types
### 4.1 User
```json
{
  "id": "string",
  "name": "string",
  "email": "string",
  "created_at": "string",
  "updated_at": "string"
}
```

### 4.2 Error
```json
{
  "code": "string",
  "message": "string",
  "details": {}
}
```

## 5. Webhooks
### 5.1 Available Events
- `user.created`
- `user.updated`
- `user.deleted`

### 5.2 Webhook Payload
```json
{
  "event": "string",
  "data": {},
  "timestamp": "string"
}
```

## 6. SDK Examples
### 6.1 JavaScript/TypeScript
```typescript
import { ApiClient } from '@example/sdk';

const client = new ApiClient({
  baseUrl: 'https://api.example.com/v1',
  token: 'your-token'
});

// Get users
const users = await client.users.list({
  page: 1,
  limit: 10
});

// Create user
const user = await client.users.create({
  name: 'John Doe',
  email: 'john@example.com',
  password: 'secret'
});
```

### 6.2 Python
```python
from example_sdk import ApiClient

client = ApiClient(
    base_url='https://api.example.com/v1',
    token='your-token'
)

# Get users
users = client.users.list(page=1, limit=10)

# Create user
user = client.users.create(
    name='John Doe',
    email='john@example.com',
    password='secret'
)
```

## 7. Best Practices
### 7.1 Authentication
- Always use HTTPS
- Store tokens securely
- Implement token refresh
- Handle token expiration

### 7.2 Error Handling
- Implement exponential backoff
- Handle rate limiting
- Log errors appropriately
- Provide user-friendly messages

### 7.3 Performance
- Use pagination
- Implement caching
- Optimize payload size
- Use compression

## 8. Versioning
### 8.1 Version Policy
- Major versions in URL
- Minor versions in headers
- Deprecation notices
- Migration guides

### 8.2 Current Versions
- v1: Current stable
- v2: Beta
- v0: Deprecated

## 9. Security
### 9.1 Authentication
- OAuth 2.0
- JWT tokens
- API keys
- Rate limiting

### 9.2 Data Protection
- HTTPS
- Data encryption
- Input validation
- Output sanitization

## 10. Support
### 10.1 Getting Help
- Documentation
- Support email
- Community forum
- Status page

### 10.2 Reporting Issues
- Bug reports
- Feature requests
- Security vulnerabilities
- Performance issues 