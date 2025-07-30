# API Documentation

This document provides comprehensive information about the GigSync API endpoints, authentication, and data structures.

## 📋 Table of Contents

- [Base URL & Versioning](#base-url--versioning)
- [Authentication](#authentication)
- [Rate Limiting](#rate-limiting)
- [Request/Response Format](#requestresponse-format)
- [Error Handling](#error-handling)
- [Endpoints](#endpoints)
- [Data Models](#data-models)
- [Webhooks](#webhooks)
- [SDK & Libraries](#sdk--libraries)

## 🌐 Base URL & Versioning

### Base URLs
- **Production**: `https://api.gigsync.app/v1`
- **Staging**: `https://api-staging.gigsync.app/v1`
- **Development**: `http://localhost:3000/api/v1`

### API Versioning
- Current version: `v1`
- Version is specified in the URL path
- Backward compatibility maintained for at least 12 months

## 🔐 Authentication

### Authentication Methods

#### 1. Session-based (Web App)
```typescript
// Automatic session handling via cookies
// No additional headers required
```

#### 2. API Key (Third-party integrations)
```http
Authorization: Bearer sk_live_abc123...
```

#### 3. JWT Token (Mobile/SPA)
```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Authentication Endpoints

#### Sign Up
```http
POST /api/auth/signup
Content-Type: application/json

{
  "email": "student@university.edu",
  "password": "securePassword123",
  "full_name": "John Doe",
  "university": "University of Technology"
}
```

**Response:**
```json
{
  "user": {
    "id": "user_123",
    "email": "student@university.edu",
    "full_name": "John Doe"
  },
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "rt_abc123..."
}
```

#### Sign In
```http
POST /api/auth/signin
Content-Type: application/json

{
  "email": "student@university.edu",
  "password": "securePassword123"
}
```

#### Refresh Token
```http
POST /api/auth/refresh
Content-Type: application/json

{
  "refresh_token": "rt_abc123..."
}
```

## 🚦 Rate Limiting

### Limits by Endpoint Type

| Endpoint Type | Authenticated | Unauthenticated |
|---------------|---------------|-----------------|
| Authentication | 5 req/min | 3 req/min |
| User Operations | 100 req/hr | 20 req/hr |
| Gig Operations | 200 req/hr | 50 req/hr |
| File Uploads | 10 req/hr | 5 req/hr |
| Payments | 50 req/hr | N/A |

### Rate Limit Headers
```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 85
X-RateLimit-Reset: 1609459200
```

## 📤 Request/Response Format

### Request Headers
```http
Content-Type: application/json
Authorization: Bearer <token>
X-API-Version: v1
```

### Standard Response Format
```json
{
  "success": true,
  "data": {
    // Response data
  },
  "meta": {
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "pages": 8
    }
  }
}
```

### Pagination
```http
GET /api/gigs?page=2&limit=20&sort=created_at&order=desc
```

### Filtering & Search
```http
GET /api/gigs?category=web-development&budget_min=100&budget_max=500&location=remote
```

## ❌ Error Handling

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      }
    ]
  }
}
```

### HTTP Status Codes

| Code | Meaning | Description |
|------|---------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created |
| 400 | Bad Request | Invalid request data |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 422 | Validation Error | Invalid input data |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error |

### Common Error Codes

| Code | Description |
|------|-------------|
| `AUTHENTICATION_REQUIRED` | User must be authenticated |
| `INVALID_CREDENTIALS` | Invalid email/password |
| `VALIDATION_ERROR` | Request validation failed |
| `RESOURCE_NOT_FOUND` | Requested resource doesn't exist |
| `PERMISSION_DENIED` | User lacks required permissions |
| `RATE_LIMIT_EXCEEDED` | Too many requests |
| `PAYMENT_FAILED` | Payment processing error |

## 🔌 Endpoints

### Users

#### Get Current User Profile
```http
GET /api/users/profile
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "user_123",
    "email": "student@university.edu",
    "full_name": "John Doe",
    "avatar_url": "https://storage.supabase.co/...",
    "university": "University of Technology",
    "major": "Computer Science",
    "graduation_year": 2025,
    "skills": ["React", "Node.js", "Python"],
    "bio": "Passionate computer science student...",
    "location": {
      "city": "San Francisco",
      "state": "CA",
      "country": "US"
    },
    "rating": 4.8,
    "total_gigs": 15,
    "created_at": "2024-01-15T10:30:00Z"
  }
}
```

#### Update User Profile
```http
PUT /api/users/profile
Authorization: Bearer <token>
Content-Type: application/json

{
  "full_name": "John Smith",
  "bio": "Updated bio...",
  "skills": ["React", "Next.js", "TypeScript"]
}
```

#### Get User by ID
```http
GET /api/users/{user_id}
```

#### Upload Avatar
```http
POST /api/users/upload-avatar
Authorization: Bearer <token>
Content-Type: multipart/form-data

{
  "avatar": <file>
}
```

### Gigs

#### List Gigs
```http
GET /api/gigs?page=1&limit=20&category=web-development&location=remote
```

**Query Parameters:**
- `page` (number): Page number (default: 1)
- `limit` (number): Items per page (max: 100, default: 20)
- `category` (string): Filter by category
- `budget_min` (number): Minimum budget
- `budget_max` (number): Maximum budget
- `location` (string): Location filter
- `is_remote` (boolean): Remote work filter
- `skills` (string[]): Required skills
- `urgency` (enum): low, medium, high, urgent
- `sort` (enum): created_at, budget, deadline
- `order` (enum): asc, desc

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "gig_123",
      "title": "Build Landing Page",
      "description": "Need a modern landing page for startup",
      "category": {
        "id": "cat_web",
        "name": "Web Development"
      },
      "creator": {
        "id": "user_456",
        "full_name": "Jane Smith",
        "avatar_url": "https://...",
        "rating": 4.9
      },
      "budget": 500.00,
      "currency": "USD",
      "deadline": "2024-02-15T23:59:59Z",
      "location": {
        "type": "remote"
      },
      "skills_required": ["React", "TypeScript", "Tailwind CSS"],
      "status": "open",
      "urgency": "medium",
      "applications_count": 5,
      "created_at": "2024-01-20T14:30:00Z"
    }
  ],
  "meta": {
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "pages": 8
    }
  }
}
```

#### Create Gig
```http
POST /api/gigs
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Mobile App UI Design",
  "description": "Looking for a talented designer to create...",
  "category_id": "cat_design",
  "budget": 800.00,
  "deadline": "2024-03-01T23:59:59Z",
  "location": {
    "type": "hybrid",
    "city": "San Francisco",
    "state": "CA"
  },
  "skills_required": ["Figma", "UI/UX", "Mobile Design"],
  "urgency": "high"
}
```

#### Get Gig Details
```http
GET /api/gigs/{gig_id}
```

#### Update Gig
```http
PUT /api/gigs/{gig_id}
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Updated title",
  "budget": 600.00
}
```

#### Delete Gig
```http
DELETE /api/gigs/{gig_id}
Authorization: Bearer <token>
```

### Applications

#### Apply to Gig
```http
POST /api/gigs/{gig_id}/apply
Authorization: Bearer <token>
Content-Type: application/json

{
  "cover_letter": "I'm excited to work on this project...",
  "proposed_rate": 450.00,
  "estimated_duration": "2 weeks",
  "portfolio_links": [
    "https://github.com/johndoe/project1",
    "https://johndoe.portfolio.com"
  ]
}
```

#### Get Applications
```http
GET /api/applications?status=pending&gig_id=gig_123
```

#### Update Application Status
```http
PUT /api/applications/{application_id}
Authorization: Bearer <token>
Content-Type: application/json

{
  "status": "accepted"
}
```

### Messages

#### Get Messages for Gig
```http
GET /api/messages/{gig_id}?page=1&limit=50
Authorization: Bearer <token>
```

#### Send Message
```http
POST /api/messages/{gig_id}
Authorization: Bearer <token>
Content-Type: application/json

{
  "content": "Hello! I have a question about the requirements...",
  "recipient_id": "user_456"
}
```

### Payments

#### Create Payment Intent
```http
POST /api/payments/intent
Authorization: Bearer <token>
Content-Type: application/json

{
  "gig_id": "gig_123",
  "amount": 500.00,
  "currency": "USD"
}
```

#### Payment History
```http
GET /api/payments/history?page=1&limit=20
Authorization: Bearer <token>
```

### Categories

#### List Categories
```http
GET /api/categories
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "cat_web",
      "name": "Web Development",
      "description": "Frontend and backend web development",
      "icon": "💻",
      "gig_count": 150
    },
    {
      "id": "cat_design",
      "name": "Design",
      "description": "UI/UX, graphic design, branding",
      "icon": "🎨",
      "gig_count": 89
    }
  ]
}
```

### Search

#### Search Gigs
```http
POST /api/search/gigs
Content-Type: application/json

{
  "query": "react developer",
  "filters": {
    "budget_range": [100, 1000],
    "location": "remote",
    "skills": ["React", "TypeScript"]
  },
  "sort": {
    "field": "relevance",
    "order": "desc"
  },
  "page": 1,
  "limit": 20
}
```

## 📊 Data Models

### User Model
```typescript
interface User {
  id: string;
  email: string;
  full_name: string;
  avatar_url?: string;
  university?: string;
  major?: string;
  graduation_year?: number;
  skills: string[];
  bio?: string;
  location?: Location;
  rating: number;
  total_gigs: number;
  created_at: string;
  updated_at: string;
}
```

### Gig Model
```typescript
interface Gig {
  id: string;
  title: string;
  description: string;
  category: Category;
  creator: User;
  budget: number;
  currency: string;
  deadline?: string;
  location: Location;
  skills_required: string[];
  status: 'open' | 'in_progress' | 'completed' | 'cancelled';
  urgency: 'low' | 'medium' | 'high' | 'urgent';
  applications_count: number;
  created_at: string;
  updated_at: string;
}
```

### Application Model
```typescript
interface Application {
  id: string;
  gig: Gig;
  applicant: User;
  cover_letter: string;
  proposed_rate: number;
  estimated_duration: string;
  portfolio_links: string[];
  status: 'pending' | 'accepted' | 'rejected' | 'withdrawn';
  created_at: string;
  updated_at: string;
}
```

### Location Model
```typescript
interface Location {
  type: 'remote' | 'on_site' | 'hybrid';
  city?: string;
  state?: string;
  country?: string;
  coordinates?: [number, number]; // [lat, lng]
}
```

## 🔔 Webhooks

### Webhook Events

| Event | Description |
|-------|-------------|
| `gig.created` | New gig posted |
| `gig.updated` | Gig information updated |
| `application.submitted` | New application received |
| `application.accepted` | Application accepted |
| `payment.completed` | Payment successfully processed |
| `message.sent` | New message in conversation |

### Webhook Configuration
```http
POST /api/webhooks
Authorization: Bearer <token>
Content-Type: application/json

{
  "url": "https://your-app.com/webhooks/gigsync",
  "events": ["gig.created", "application.submitted"],
  "secret": "webhook_secret_key"
}
```

### Webhook Payload Example
```json
{
  "event": "application.submitted",
  "data": {
    "application": {
      "id": "app_123",
      "gig_id": "gig_456",
      "applicant_id": "user_789"
    }
  },
  "timestamp": "2024-01-20T15:30:00Z"
}
```

## 📚 SDK & Libraries

### JavaScript/TypeScript SDK
```bash
npm install @gigsync/sdk
```

```typescript
import { GigSyncClient } from '@gigsync/sdk';

const client = new GigSyncClient({
  apiKey: 'your-api-key',
  environment: 'production' // or 'staging'
});

// List gigs
const gigs = await client.gigs.list({
  category: 'web-development',
  limit: 10
});

// Create gig
const newGig = await client.gigs.create({
  title: 'Build E-commerce Site',
  description: 'Need a full-stack developer...',
  budget: 1500
});
```

### Python SDK
```bash
pip install gigsync-python
```

```python
from gigsync import GigSyncClient

client = GigSyncClient(api_key='your-api-key')

# List gigs
gigs = client.gigs.list(category='web-development', limit=10)

# Apply to gig
application = client.applications.create(
    gig_id='gig_123',
    cover_letter='I would love to work on this project...'
)
```

## 🛠️ Testing the API

### Using cURL
```bash
# Get gigs
curl -H "Authorization: Bearer your-token" \
     "https://api.gigsync.app/v1/gigs?limit=5"

# Create gig
curl -X POST \
     -H "Authorization: Bearer your-token" \
     -H "Content-Type: application/json" \
     -d '{"title":"Test Gig","description":"Test description","budget":100}' \
     "https://api.gigsync.app/v1/gigs"
```

### Postman Collection
[Download Postman Collection](https://api.gigsync.app/postman/collection.json)

## 📞 Support

- **API Status**: [status.gigsync.app](https://status.gigsync.app)
- **Documentation**: [docs.gigsync.app](https://docs.gigsync.app)
- **Support Email**: [api-support@gigsync.app](mailto:api-support@gigsync.app)
- **GitHub Issues**: [API Issues](https://github.com/Mahas1234/GigSync/issues)

---

**Happy building! 🚀**