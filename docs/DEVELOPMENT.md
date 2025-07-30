# Development Guide

This guide provides comprehensive information for developers working on GigSync.

## 📋 Table of Contents

- [Development Environment Setup](#development-environment-setup)
- [Project Structure](#project-structure)
- [Development Workflow](#development-workflow)
- [Database Schema](#database-schema)
- [API Endpoints](#api-endpoints)
- [Authentication & Authorization](#authentication--authorization)
- [Testing Strategy](#testing-strategy)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)

## 🛠️ Development Environment Setup

### Required Tools

| Tool | Version | Purpose |
|------|---------|---------|
| Node.js | 18+ | Runtime environment |
| npm/yarn | Latest | Package management |
| Git | Latest | Version control |
| VS Code | Latest | Recommended IDE |
| Supabase CLI | Latest | Database management |

### Recommended VS Code Extensions

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "ms-vscode.vscode-typescript-next",
    "supabase.supabase-vscode",
    "ms-playwright.playwright",
    "streetsidesoftware.code-spell-checker"
  ]
}
```

### Environment Variables

Create a `.env.local` file:

```bash
# Database & Auth
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Authentication
NEXTAUTH_SECRET=your-random-secret
NEXTAUTH_URL=http://localhost:3000

# Payments
STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Email
RESEND_API_KEY=re_...

# File Upload
NEXT_PUBLIC_MAX_FILE_SIZE=5242880 # 5MB
NEXT_PUBLIC_ALLOWED_FILE_TYPES=image/jpeg,image/png,application/pdf

# Analytics (Optional)
NEXT_PUBLIC_VERCEL_ANALYTICS_ID=your-analytics-id
```

### Local Development Setup

```bash
# Clone and setup
git clone https://github.com/Mahas1234/GigSync.git
cd GigSync

# Install dependencies
npm install

# Setup environment
cp .env.example .env.local
# Edit .env.local with your values

# Setup database
npm run db:setup
npm run db:migrate
npm run db:seed

# Start development server
npm run dev
```

## 📁 Project Structure

```
GigSync/
├── public/                 # Static assets
│   ├── images/            # Image assets
│   ├── icons/             # Icon files
│   └── manifest.json      # PWA manifest
├── src/
│   ├── app/               # Next.js 13+ app directory
│   │   ├── (auth)/        # Auth route group
│   │   ├── (dashboard)/   # Dashboard routes
│   │   ├── api/           # API routes
│   │   ├── globals.css    # Global styles
│   │   ├── layout.tsx     # Root layout
│   │   └── page.tsx       # Home page
│   ├── components/        # React components
│   │   ├── ui/            # Base UI components
│   │   ├── forms/         # Form components
│   │   ├── layout/        # Layout components
│   │   └── features/      # Feature-specific components
│   ├── hooks/             # Custom React hooks
│   ├── lib/               # Utility libraries
│   │   ├── auth.ts        # Auth configuration
│   │   ├── db.ts          # Database client
│   │   ├── payments.ts    # Payment integration
│   │   └── utils.ts       # General utilities
│   ├── stores/            # State management
│   │   ├── authStore.ts   # Authentication state
│   │   ├── gigsStore.ts   # Gigs state
│   │   └── uiStore.ts     # UI state
│   ├── types/             # TypeScript type definitions
│   │   ├── database.ts    # Database types
│   │   ├── auth.ts        # Auth types
│   │   └── api.ts         # API types
│   └── middleware.ts      # Next.js middleware
├── docs/                  # Documentation
├── tests/                 # Test files
│   ├── __mocks__/         # Test mocks
│   ├── components/        # Component tests
│   ├── pages/             # Page tests
│   └── utils/             # Utility tests
├── .github/               # GitHub workflows
├── supabase/              # Supabase configuration
│   ├── migrations/        # Database migrations
│   └── seed.sql           # Seed data
└── package.json
```

## 🔄 Development Workflow

### Git Workflow

```bash
# Start new feature
git checkout main
git pull origin main
git checkout -b feature/gig-search

# Make changes and commit
git add .
git commit -m "feat(search): add advanced gig filtering"

# Push and create PR
git push origin feature/gig-search
# Create PR on GitHub
```

### Daily Development Tasks

```bash
# Start development
npm run dev                # Start dev server
npm run dev:db            # Start Supabase locally

# Code quality
npm run lint              # Check linting issues
npm run lint:fix          # Fix linting issues
npm run type-check        # TypeScript checks
npm run format            # Format code

# Testing
npm run test              # Run all tests
npm run test:watch        # Watch mode
npm run test:coverage     # Coverage report
npm run test:e2e          # End-to-end tests

# Database
npm run db:reset          # Reset database
npm run db:migrate        # Run migrations
npm run db:seed           # Seed data
npm run db:generate       # Generate types
```

## 🗄️ Database Schema

### Core Tables

#### Users
```sql
create table users (
  id uuid primary key default gen_random_uuid(),
  email text unique not null,
  full_name text,
  avatar_url text,
  university text,
  major text,
  graduation_year integer,
  skills text[],
  bio text,
  location jsonb,
  rating decimal(3,2) default 0,
  total_gigs integer default 0,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

#### Gigs
```sql
create table gigs (
  id uuid primary key default gen_random_uuid(),
  title text not null,
  description text not null,
  category_id uuid references categories(id),
  creator_id uuid references users(id),
  budget decimal(10,2) not null,
  currency text default 'USD',
  deadline timestamptz,
  location jsonb,
  is_remote boolean default false,
  skills_required text[],
  status gig_status default 'open',
  urgency urgency_level default 'medium',
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create type gig_status as enum ('open', 'in_progress', 'completed', 'cancelled');
create type urgency_level as enum ('low', 'medium', 'high', 'urgent');
```

#### Applications
```sql
create table applications (
  id uuid primary key default gen_random_uuid(),
  gig_id uuid references gigs(id),
  applicant_id uuid references users(id),
  cover_letter text,
  proposed_rate decimal(10,2),
  estimated_duration interval,
  status application_status default 'pending',
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create type application_status as enum ('pending', 'accepted', 'rejected', 'withdrawn');
```

### Indexes and Performance

```sql
-- Search optimization
create index idx_gigs_search on gigs using gin(to_tsvector('english', title || ' ' || description));
create index idx_gigs_location on gigs using gist((location->>'coordinates')::point);
create index idx_gigs_category_status on gigs(category_id, status);
create index idx_gigs_created_at on gigs(created_at desc);

-- User performance
create index idx_users_university on users(university);
create index idx_users_skills on users using gin(skills);
```

## 🔌 API Endpoints

### Authentication
```
POST   /api/auth/signin          # Sign in user
POST   /api/auth/signup          # Register new user
POST   /api/auth/signout         # Sign out user
GET    /api/auth/session         # Get current session
```

### Users
```
GET    /api/users/profile        # Get current user profile
PUT    /api/users/profile        # Update user profile
GET    /api/users/[id]           # Get user by ID
POST   /api/users/upload-avatar  # Upload profile picture
```

### Gigs
```
GET    /api/gigs                 # List gigs (with filters)
POST   /api/gigs                 # Create new gig
GET    /api/gigs/[id]            # Get gig details
PUT    /api/gigs/[id]            # Update gig
DELETE /api/gigs/[id]            # Delete gig
POST   /api/gigs/[id]/apply      # Apply to gig
```

### Applications
```
GET    /api/applications         # Get user's applications
PUT    /api/applications/[id]    # Update application status
DELETE /api/applications/[id]    # Withdraw application
```

### Messages
```
GET    /api/messages/[gigId]     # Get messages for gig
POST   /api/messages/[gigId]     # Send message
```

### Payments
```
POST   /api/payments/intent      # Create payment intent
POST   /api/payments/webhook     # Stripe webhook
GET    /api/payments/history     # Payment history
```

## 🔐 Authentication & Authorization

### User Roles

```typescript
type UserRole = 'student' | 'employer' | 'admin';

interface User {
  id: string;
  email: string;
  role: UserRole;
  permissions: Permission[];
}
```

### Permission System

```typescript
const permissions = {
  'gigs:create': ['student', 'employer'],
  'gigs:apply': ['student'],
  'gigs:manage': ['employer', 'admin'],
  'users:manage': ['admin'],
  'payments:process': ['employer', 'admin']
};
```

### Middleware Protection

```typescript
// middleware.ts
import { createMiddleware } from '@supabase/auth-helpers-nextjs';

export const middleware = createMiddleware({
  forceHttps: process.env.NODE_ENV === 'production',
  matcher: ['/dashboard/:path*', '/api/protected/:path*']
});
```

## 🧪 Testing Strategy

### Test Structure

```
tests/
├── unit/                  # Unit tests
│   ├── components/        # Component tests
│   ├── hooks/            # Hook tests
│   └── utils/            # Utility tests
├── integration/          # Integration tests
│   ├── api/              # API tests
│   └── pages/            # Page tests
├── e2e/                  # End-to-end tests
│   ├── auth.spec.ts      # Authentication flows
│   ├── gigs.spec.ts      # Gig management
│   └── payments.spec.ts  # Payment flows
└── fixtures/             # Test data
```

### Testing Tools

- **Unit Tests**: Jest + React Testing Library
- **Integration Tests**: Jest + Supertest
- **E2E Tests**: Playwright
- **Visual Testing**: Chromatic (planned)

### Example Test

```typescript
// tests/components/GigCard.test.tsx
import { render, screen } from '@testing-library/react';
import { GigCard } from '@/components/GigCard';

const mockGig = {
  id: '1',
  title: 'Web Development Task',
  description: 'Build a landing page',
  budget: 500,
  deadline: new Date('2024-12-31'),
  skills: ['React', 'TypeScript']
};

describe('GigCard', () => {
  it('renders gig information correctly', () => {
    render(<GigCard gig={mockGig} />);
    
    expect(screen.getByText('Web Development Task')).toBeInTheDocument();
    expect(screen.getByText('$500')).toBeInTheDocument();
    expect(screen.getByText('React')).toBeInTheDocument();
  });
});
```

## 🚀 Deployment

### Environments

- **Development**: `http://localhost:3000`
- **Staging**: `https://gigsync-staging.vercel.app`
- **Production**: `https://gigsync.app`

### Deployment Process

```bash
# Production deployment
git checkout main
git pull origin main
git tag v1.0.0
git push origin v1.0.0

# Vercel automatically deploys from main branch
```

### Environment Configuration

Each environment has its own configuration:

```javascript
// next.config.js
const config = {
  env: {
    CUSTOM_KEY: process.env.NODE_ENV === 'production' 
      ? 'production-value' 
      : 'development-value'
  }
};
```

## 🐛 Troubleshooting

### Common Issues

#### Database Connection Issues
```bash
# Check Supabase connection
npx supabase status

# Reset local database
npx supabase db reset
```

#### Build Errors
```bash
# Clear Next.js cache
rm -rf .next

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

#### Type Errors
```bash
# Regenerate database types
npm run db:generate

# Check TypeScript
npm run type-check
```

### Debug Mode

```bash
# Enable debug logging
DEBUG=* npm run dev

# Database query logging
DATABASE_URL="postgres://..."?log=true npm run dev
```

### Performance Monitoring

- **Core Web Vitals**: Lighthouse CI
- **Error Tracking**: Sentry
- **Analytics**: Vercel Analytics
- **Performance**: Next.js built-in metrics

## 📞 Getting Help

- **Documentation**: Check this guide first
- **GitHub Issues**: [Report bugs](https://github.com/Mahas1234/GigSync/issues)
- **Discussions**: [Ask questions](https://github.com/Mahas1234/GigSync/discussions)
- **Email**: [bejugammahaswi@gmail.com](mailto:bejugammahaswi@gmail.com)

---

Happy coding! 🚀