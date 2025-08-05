# API Documentation

Complete reference for all public APIs in the reazon_repo project.

## Overview

This section provides comprehensive documentation for all REST APIs, GraphQL endpoints, and other public interfaces.

## Authentication

### API Key Authentication
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     https://api.example.com/v1/endpoint
```

### OAuth 2.0
```bash
curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     https://api.example.com/v1/endpoint
```

## Base URL

- **Production**: `https://api.example.com/v1`
- **Staging**: `https://staging-api.example.com/v1`
- **Development**: `http://localhost:3000/api/v1`

## Common Response Format

All API responses follow this standard format:

```json
{
  "success": true,
  "data": {},
  "message": "Operation completed successfully",
  "timestamp": "2024-01-01T12:00:00Z",
  "version": "1.0.0"
}
```

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input provided",
    "details": [
      {
        "field": "email",
        "message": "Email format is invalid"
      }
    ]
  },
  "timestamp": "2024-01-01T12:00:00Z"
}
```

## HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | OK - Request successful |
| 201 | Created - Resource created successfully |
| 400 | Bad Request - Invalid request format |
| 401 | Unauthorized - Authentication required |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 422 | Unprocessable Entity - Validation failed |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error - Server error |

## Rate Limiting

- **Limit**: 1000 requests per hour per API key
- **Headers**: 
  - `X-RateLimit-Limit`: Maximum requests per hour
  - `X-RateLimit-Remaining`: Remaining requests in current window
  - `X-RateLimit-Reset`: Timestamp when limit resets

## Pagination

For endpoints that return lists, pagination follows this format:

### Request Parameters
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 20, max: 100)
- `sort`: Sort field and direction (e.g., `created_at:desc`)

### Response Format
```json
{
  "success": true,
  "data": {
    "items": [...],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "totalPages": 8,
      "hasNext": true,
      "hasPrev": false
    }
  }
}
```

## API Endpoints

### User Management
- [Get User Profile](./endpoints/users/get-profile.md)
- [Update User Profile](./endpoints/users/update-profile.md)
- [Delete User Account](./endpoints/users/delete-account.md)

### Data Operations
- [List Items](./endpoints/data/list-items.md)
- [Create Item](./endpoints/data/create-item.md)
- [Update Item](./endpoints/data/update-item.md)
- [Delete Item](./endpoints/data/delete-item.md)

### File Management
- [Upload File](./endpoints/files/upload.md)
- [Download File](./endpoints/files/download.md)
- [Delete File](./endpoints/files/delete.md)

## SDKs and Libraries

### JavaScript/TypeScript
```bash
npm install @reazon/api-client
```

```typescript
import { ReazonClient } from '@reazon/api-client';

const client = new ReazonClient({
  apiKey: 'your-api-key',
  baseUrl: 'https://api.example.com/v1'
});

const user = await client.users.getProfile();
```

### Python
```bash
pip install reazon-python
```

```python
from reazon import ReazonClient

client = ReazonClient(api_key='your-api-key')
user = client.users.get_profile()
```

### cURL Examples

#### Basic GET Request
```bash
curl -X GET \
  "https://api.example.com/v1/users/profile" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json"
```

#### POST Request with Data
```bash
curl -X POST \
  "https://api.example.com/v1/users" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com"
  }'
```

## Testing

### Postman Collection
Download our [Postman collection](./postman/reazon-api.postman_collection.json) for easy API testing.

### Test Environment
- **Base URL**: `https://api-test.example.com/v1`
- **Test API Key**: Contact support for test credentials

## Webhooks

Configure webhooks to receive real-time notifications:

### Supported Events
- `user.created`
- `user.updated`
- `user.deleted`
- `data.created`
- `data.updated`
- `data.deleted`

### Webhook Payload Example
```json
{
  "event": "user.created",
  "data": {
    "id": "user_123",
    "email": "user@example.com",
    "createdAt": "2024-01-01T12:00:00Z"
  },
  "timestamp": "2024-01-01T12:00:00Z",
  "signature": "sha256=..."
}
```

## API Versioning

- Current version: `v1`
- Version header: `Accept: application/vnd.reazon.v1+json`
- Deprecation notices: 6 months before removal
- Changelog: [API Changelog](./CHANGELOG.md)

## Support

- **Documentation Issues**: [GitHub Issues](https://github.com/reazon/api-docs/issues)
- **API Support**: support@reazon.com
- **Status Page**: [status.reazon.com](https://status.reazon.com)