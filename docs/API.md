# 🚀 API Documentation

This document provides comprehensive information about the Boutique-To-Box API endpoints.

## 📋 Table of Contents
- [Base URL](#base-url)
- [Authentication](#authentication)
- [Error Handling](#error-handling)
- [Rate Limiting](#rate-limiting)
- [Endpoints](#endpoints)
- [WebSocket Events](#websocket-events)
- [SDKs](#sdks)

## 🌐 Base URL

```
Production: https://api.boutique-to-box.com/v1
Staging: https://staging-api.boutique-to-box.com/v1
Development: http://localhost:3001/api
```

## 🔐 Authentication

### JWT Token Authentication
All protected endpoints require a valid JWT token in the Authorization header:

```http
Authorization: Bearer <your_jwt_token>
```

### Getting a Token
```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "your_password"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "user_123",
      "email": "user@example.com",
      "name": "John Doe"
    }
  }
}
```

## ❌ Error Handling

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": {
      "field": "email",
      "issue": "Invalid email format"
    }
  }
}
```

### HTTP Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `429` - Too Many Requests
- `500` - Internal Server Error

## 🚦 Rate Limiting

- **Free Tier**: 100 requests/hour
- **Pro Tier**: 1,000 requests/hour
- **Enterprise**: Unlimited

Rate limit headers:
```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640995200
```

## 📡 Endpoints

### Authentication

#### Register User
```http
POST /auth/register
```

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

#### Login User
```http
POST /auth/login
```

#### Logout User
```http
POST /auth/logout
Authorization: Bearer <token>
```

#### Get Current User
```http
GET /auth/me
Authorization: Bearer <token>
```

### Design Generation

#### Generate New Design
```http
POST /designs/generate
Authorization: Bearer <token>
Content-Type: application/json
```

**Request Body:**
```json
{
  "prompt": "Modern minimalist summer dress",
  "style": "casual",
  "colors": ["#FF6B6B", "#4ECDC4"],
  "size": "M",
  "fabric": "cotton",
  "occasion": "casual"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "design_123",
    "imageUrl": "https://cdn.boutique-to-box.com/designs/design_123.jpg",
    "modelUrl": "https://cdn.boutique-to-box.com/models/design_123.glb",
    "prompt": "Modern minimalist summer dress",
    "metadata": {
      "generationTime": 4.2,
      "aiModel": "runway-ml-v2",
      "confidence": 0.92
    },
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

#### Get Design by ID
```http
GET /designs/:id
Authorization: Bearer <token>
```

#### Update Design
```http
PUT /designs/:id
Authorization: Bearer <token>
```

#### Delete Design
```http
DELETE /designs/:id
Authorization: Bearer <token>
```

#### List User Designs
```http
GET /designs/user/:userId?page=1&limit=20&sort=createdAt&order=desc
Authorization: Bearer <token>
```

### AI Services

#### Analyze Style
```http
POST /ai/analyze-style
Authorization: Bearer <token>
Content-Type: multipart/form-data
```

**Form Data:**
- `image`: File (required)
- `options`: JSON string (optional)

#### Generate Pattern
```http
POST /ai/generate-pattern
Authorization: Bearer <token>
```

**Request Body:**
```json
{
  "basePattern": "floral",
  "colors": ["#FF6B6B", "#4ECDC4"],
  "complexity": "medium",
  "style": "modern"
}
```

#### Enhance Design
```http
POST /ai/enhance-design/:designId
Authorization: Bearer <token>
```

### 3D Models

#### Generate 3D Model
```http
POST /models/generate
Authorization: Bearer <token>
```

**Request Body:**
```json
{
  "designId": "design_123",
  "bodyMeasurements": {
    "chest": 36,
    "waist": 28,
    "hips": 38,
    "height": 165
  },
  "format": "glb"
}
```

#### Get Model by ID
```http
GET /models/:id
Authorization: Bearer <token>
```

### Manufacturing

#### Create Manufacturing Order
```http
POST /manufacturing/orders
Authorization: Bearer <token>
```

**Request Body:**
```json
{
  "designId": "design_123",
  "quantity": 1,
  "size": "M",
  "shippingAddress": {
    "name": "John Doe",
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zipCode": "10001",
    "country": "US"
  },
  "priority": "standard"
}
```

#### Get Order Status
```http
GET /manufacturing/orders/:orderId
Authorization: Bearer <token>
```

### Analytics

#### Get User Analytics
```http
GET /analytics/user/:userId
Authorization: Bearer <token>
```

#### Get Design Performance
```http
GET /analytics/designs/:designId
Authorization: Bearer <token>
```

## 🔌 WebSocket Events

Connect to WebSocket for real-time updates:
```javascript
const ws = new WebSocket('wss://api.boutique-to-box.com/ws');

// Authentication
ws.send(JSON.stringify({
  type: 'auth',
  token: 'your_jwt_token'
}));
```

### Events

#### Design Generation Progress
```json
{
  "type": "design_progress",
  "data": {
    "designId": "design_123",
    "progress": 75,
    "stage": "enhancing_details",
    "estimatedTime": 30
  }
}
```

#### Manufacturing Updates
```json
{
  "type": "manufacturing_update",
  "data": {
    "orderId": "order_456",
    "status": "printing",
    "estimatedCompletion": "2024-01-20T15:00:00Z"
  }
}
```

## 📚 SDKs

### JavaScript/TypeScript SDK
```bash
npm install @boutique-to-box/sdk
```

```typescript
import { BoutiqueToBoxSDK } from '@boutique-to-box/sdk';

const client = new BoutiqueToBoxSDK({
  apiKey: 'your_api_key',
  environment: 'production'
});

// Generate design
const design = await client.designs.generate({
  prompt: 'Modern summer dress',
  style: 'casual'
});
```

### Python SDK
```bash
pip install boutique-to-box
```

```python
from boutique_to_box import Client

client = Client(api_key='your_api_key')

# Generate design
design = client.designs.generate(
    prompt='Modern summer dress',
    style='casual'
)
```

## 📝 Examples

### Complete Design Generation Flow
```javascript
// 1. Authenticate
const authResponse = await fetch('/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'user@example.com',
    password: 'password'
  })
});

const { token } = await authResponse.json();

// 2. Generate design
const designResponse = await fetch('/api/designs/generate', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    prompt: 'Elegant evening gown',
    colors: ['#000000', '#FFD700']
  })
});

const design = await designResponse.json();

// 3. Generate 3D model
const modelResponse = await fetch('/api/models/generate', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    designId: design.data.id,
    bodyMeasurements: {
      chest: 36,
      waist: 28,
      hips: 38
    }
  })
});

const model = await modelResponse.json();
```

## 🔧 Testing

### Postman Collection
Import our Postman collection for easy API testing:
[Download Collection](https://api.boutique-to-box.com/postman/collection.json)

### cURL Examples
```bash
# Generate design
curl -X POST https://api.boutique-to-box.com/v1/designs/generate \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Modern casual dress",
    "colors": ["#FF6B6B"]
  }'
```

## 📞 Support

- **Documentation**: [docs.boutique-to-box.com](https://docs.boutique-to-box.com)
- **Discord**: [Join our community](https://discord.gg/boutique-to-box)
- **Email**: api-support@boutique-to-box.com
- **Status Page**: [status.boutique-to-box.com](https://status.boutique-to-box.com)