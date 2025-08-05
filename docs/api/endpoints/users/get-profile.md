# Get User Profile

Retrieve the authenticated user's profile information.

## Endpoint

```
GET /users/profile
```

## Authentication

Requires Bearer token authentication.

## Parameters

### Headers

| Name | Type | Required | Description |
|------|------|----------|-------------|
| Authorization | string | Yes | Bearer token for authentication |
| Content-Type | string | Yes | Must be `application/json` |

### Query Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| include | string | No | - | Comma-separated list of additional fields to include (e.g., `preferences,settings`) |

## Response

### Success Response (200 OK)

```json
{
  "success": true,
  "data": {
    "id": "user_123456789",
    "email": "john.doe@example.com",
    "username": "johndoe",
    "firstName": "John",
    "lastName": "Doe",
    "avatar": "https://cdn.example.com/avatars/user_123456789.jpg",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-03-20T14:22:33Z",
    "isVerified": true,
    "lastLoginAt": "2024-03-20T09:15:22Z",
    "timezone": "America/New_York",
    "locale": "en-US"
  },
  "message": "Profile retrieved successfully",
  "timestamp": "2024-03-20T14:22:35Z"
}
```

### Error Responses

#### 401 Unauthorized
```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or expired authentication token"
  },
  "timestamp": "2024-03-20T14:22:35Z"
}
```

#### 404 Not Found
```json
{
  "success": false,
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User profile not found"
  },
  "timestamp": "2024-03-20T14:22:35Z"
}
```

## Code Examples

### JavaScript/TypeScript

```typescript
import { ReazonClient } from '@reazon/api-client';

const client = new ReazonClient({
  apiKey: 'your-api-key'
});

try {
  const profile = await client.users.getProfile();
  console.log('User profile:', profile.data);
} catch (error) {
  console.error('Error fetching profile:', error.message);
}
```

### Python

```python
from reazon import ReazonClient

client = ReazonClient(api_key='your-api-key')

try:
    profile = client.users.get_profile()
    print(f"User profile: {profile.data}")
except Exception as error:
    print(f"Error fetching profile: {error}")
```

### cURL

```bash
curl -X GET \
  "https://api.example.com/v1/users/profile" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json"
```

### With Additional Fields

```bash
curl -X GET \
  "https://api.example.com/v1/users/profile?include=preferences,settings" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json"
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique user identifier |
| email | string | User's email address |
| username | string | User's unique username |
| firstName | string | User's first name |
| lastName | string | User's last name |
| avatar | string | URL to user's profile picture |
| createdAt | string | ISO 8601 timestamp of account creation |
| updatedAt | string | ISO 8601 timestamp of last profile update |
| isVerified | boolean | Whether the user's email is verified |
| lastLoginAt | string | ISO 8601 timestamp of last login |
| timezone | string | User's timezone (IANA format) |
| locale | string | User's preferred locale (RFC 5646 format) |

## Usage Notes

- Profile data is cached for 5 minutes
- The `include` parameter allows fetching additional user data in a single request
- Rate limit: 100 requests per minute per user
- Avatar URLs are signed and expire after 24 hours

## Related Endpoints

- [Update User Profile](./update-profile.md)
- [Upload Profile Picture](./upload-avatar.md)
- [Get User Preferences](./get-preferences.md)