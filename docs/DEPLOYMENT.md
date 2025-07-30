# Deployment Guide

This guide covers deploying GigSync to various environments and platforms.

## 📋 Table of Contents

- [Overview](#overview)
- [Environment Setup](#environment-setup)
- [Local Development](#local-development)
- [Staging Deployment](#staging-deployment)
- [Production Deployment](#production-deployment)
- [Database Migrations](#database-migrations)
- [Monitoring & Health Checks](#monitoring--health-checks)
- [Rollback Procedures](#rollback-procedures)
- [Troubleshooting](#troubleshooting)

## 🌟 Overview

GigSync uses a modern deployment stack optimized for performance, scalability, and developer experience:

- **Frontend**: Vercel (Next.js)
- **Database**: Supabase (PostgreSQL)
- **File Storage**: Supabase Storage
- **Authentication**: Supabase Auth
- **CI/CD**: GitHub Actions
- **Monitoring**: Sentry, Vercel Analytics

## 🛠️ Environment Setup

### Required Accounts & Services

Before deploying, you'll need accounts with:

1. **Vercel** - Frontend hosting
2. **Supabase** - Database and backend services
3. **GitHub** - Code repository and CI/CD
4. **Stripe** - Payment processing
5. **Sentry** - Error monitoring (optional)
6. **SendGrid/Resend** - Email services

### Environment Variables

Create environment configurations for each deployment stage:

#### Development (.env.local)
```bash
# Database & Auth
NEXT_PUBLIC_SUPABASE_URL=http://localhost:54321
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-local-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-local-service-role-key

# Auth
NEXTAUTH_SECRET=local-development-secret
NEXTAUTH_URL=http://localhost:3000

# Payments (Test)
STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_test_...

# Email (Test)
RESEND_API_KEY=re_test_...

# Monitoring (Optional)
NEXT_PUBLIC_SENTRY_DSN=your-sentry-dsn
```

#### Staging (.env.staging)
```bash
# Database & Auth
NEXT_PUBLIC_SUPABASE_URL=https://your-staging-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-staging-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-staging-service-role-key

# Auth
NEXTAUTH_SECRET=staging-secret-key
NEXTAUTH_URL=https://staging.gigsync.app

# Payments (Test)
STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_test_...

# Email
RESEND_API_KEY=re_staging_...

# Monitoring
NEXT_PUBLIC_SENTRY_DSN=your-sentry-dsn
SENTRY_ENVIRONMENT=staging
```

#### Production (.env.production)
```bash
# Database & Auth
NEXT_PUBLIC_SUPABASE_URL=https://your-prod-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-prod-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-prod-service-role-key

# Auth
NEXTAUTH_SECRET=production-secret-key
NEXTAUTH_URL=https://gigsync.app

# Payments (Live)
STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_live_...

# Email
RESEND_API_KEY=re_prod_...

# Monitoring
NEXT_PUBLIC_SENTRY_DSN=your-sentry-dsn
SENTRY_ENVIRONMENT=production
```

## 💻 Local Development

### Setup Steps

1. **Clone Repository**
   ```bash
   git clone https://github.com/Mahas1234/GigSync.git
   cd GigSync
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Setup Environment**
   ```bash
   cp .env.example .env.local
   # Edit .env.local with your development credentials
   ```

4. **Setup Local Database**
   ```bash
   # Install Supabase CLI
   npm install -g @supabase/cli
   
   # Start local Supabase
   npx supabase start
   
   # Run migrations
   npx supabase db reset
   ```

5. **Start Development Server**
   ```bash
   npm run dev
   ```

### Development Commands

```bash
# Development server
npm run dev

# Type checking
npm run type-check

# Linting
npm run lint
npm run lint:fix

# Testing
npm run test
npm run test:watch
npm run test:coverage

# Database operations
npm run db:reset
npm run db:migrate
npm run db:seed
npm run db:generate-types
```

## 🧪 Staging Deployment

### Automatic Staging Deployment

Staging deploys automatically when code is pushed to the `develop` branch:

```yaml
# .github/workflows/staging.yml
name: Deploy to Staging

on:
  push:
    branches: [develop]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Vercel
        uses: vercel/action@v1
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
          vercel-args: '--env ENVIRONMENT=staging'
```

### Manual Staging Deployment

```bash
# Deploy to staging using Vercel CLI
npx vercel --target staging

# Or using GitHub Actions manually
gh workflow run deploy-staging.yml
```

### Staging Database Setup

1. **Create Staging Supabase Project**
   ```bash
   # Link to staging project
   npx supabase link --project-ref your-staging-ref
   
   # Push database changes
   npx supabase db push
   ```

2. **Seed Staging Data**
   ```bash
   # Run staging seed script
   npm run db:seed:staging
   ```

## 🚀 Production Deployment

### Pre-deployment Checklist

Before deploying to production:

- [ ] All tests pass
- [ ] Code review completed
- [ ] Security review completed (if applicable)
- [ ] Performance testing completed
- [ ] Database migrations tested
- [ ] Environment variables configured
- [ ] Monitoring alerts configured
- [ ] Rollback plan prepared

### Production Deployment Process

#### 1. Automated Deployment (Recommended)

Production deploys automatically when code is pushed to the `main` branch:

```yaml
# .github/workflows/production.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - name: Install dependencies
        run: npm ci
      - name: Run tests
        run: npm run test:ci
      - name: Run type check
        run: npm run type-check
      - name: Run security audit
        run: npm audit --audit-level high

  deploy:
    needs: test
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Vercel
        uses: vercel/action@v1
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
          vercel-args: '--prod'
```

#### 2. Manual Deployment

```bash
# Deploy to production using Vercel CLI
npx vercel --prod

# Or create a release tag
git tag v1.0.0
git push origin v1.0.0
```

### Domain Configuration

#### 1. Custom Domain Setup (Vercel)

```bash
# Add custom domain
npx vercel domains add gigsync.app

# Configure DNS
# Add CNAME record: www -> cname.vercel-dns.com
# Add A record: @ -> 76.76.19.61
```

#### 2. SSL Certificate

Vercel automatically provides SSL certificates for custom domains.

### Production Database Setup

1. **Create Production Supabase Project**
   ```bash
   # Create production project in Supabase dashboard
   # Configure production settings
   ```

2. **Run Production Migrations**
   ```bash
   # Link to production project
   npx supabase link --project-ref your-prod-ref
   
   # Push schema changes
   npx supabase db push
   ```

3. **Configure Database Settings**
   ```sql
   -- Enable Row Level Security
   ALTER TABLE users ENABLE ROW LEVEL SECURITY;
   ALTER TABLE gigs ENABLE ROW LEVEL SECURITY;
   ALTER TABLE applications ENABLE ROW LEVEL SECURITY;
   
   -- Configure connection pooling
   ALTER SYSTEM SET max_connections = 100;
   ALTER SYSTEM SET shared_buffers = '256MB';
   ```

## 🗄️ Database Migrations

### Migration Workflow

1. **Create Migration**
   ```bash
   # Create new migration file
   npx supabase migration new add_user_preferences
   ```

2. **Write Migration SQL**
   ```sql
   -- migrations/20240120000000_add_user_preferences.sql
   CREATE TABLE user_preferences (
     id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
     user_id UUID REFERENCES users(id) ON DELETE CASCADE,
     email_notifications BOOLEAN DEFAULT true,
     sms_notifications BOOLEAN DEFAULT false,
     created_at TIMESTAMPTZ DEFAULT now()
   );
   
   -- Add RLS policy
   ALTER TABLE user_preferences ENABLE ROW LEVEL SECURITY;
   
   CREATE POLICY user_preferences_select 
   ON user_preferences FOR SELECT 
   USING (auth.uid() = user_id);
   ```

3. **Test Migration Locally**
   ```bash
   # Apply migration locally
   npx supabase db reset
   
   # Verify migration works
   npm run test:integration
   ```

4. **Deploy Migration**
   ```bash
   # Deploy to staging first
   npx supabase db push --env staging
   
   # Deploy to production
   npx supabase db push --env production
   ```

### Migration Best Practices

- Always test migrations locally first
- Use transactions for complex migrations
- Create rollback scripts for destructive changes
- Deploy during low-traffic periods
- Monitor database performance after migrations

## 📊 Monitoring & Health Checks

### Application Monitoring

#### 1. Health Check Endpoint

```typescript
// app/api/health/route.ts
export async function GET() {
  const health = {
    status: 'healthy',
    timestamp: new Date().toISOString(),
    version: process.env.APP_VERSION,
    environment: process.env.NODE_ENV,
    checks: {
      database: await checkDatabase(),
      redis: await checkCache(),
      external_apis: await checkExternalAPIs()
    }
  };
  
  const isHealthy = Object.values(health.checks)
    .every(check => check.status === 'ok');
  
  return Response.json(health, { 
    status: isHealthy ? 200 : 503 
  });
}
```

#### 2. Monitoring Setup

```typescript
// lib/monitoring.ts
import * as Sentry from '@sentry/nextjs';

export function setupMonitoring() {
  Sentry.init({
    dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
    environment: process.env.NODE_ENV,
    tracesSampleRate: 0.1,
    beforeSend(event) {
      // Filter sensitive data
      return event;
    }
  });
}
```

### Performance Monitoring

```typescript
// Track Core Web Vitals
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

function sendToAnalytics(metric) {
  // Send to your analytics service
  analytics.track('Core Web Vital', {
    name: metric.name,
    value: metric.value,
    rating: metric.rating
  });
}

getCLS(sendToAnalytics);
getFID(sendToAnalytics);
getFCP(sendToAnalytics);
getLCP(sendToAnalytics);
getTTFB(sendToAnalytics);
```

### Alerts Configuration

Set up alerts for:
- Application errors (> 5% error rate)
- Response time (> 2 seconds average)
- Database performance issues
- Payment processing failures
- High memory/CPU usage

## 🔄 Rollback Procedures

### Application Rollback

#### 1. Vercel Rollback
```bash
# List deployments
npx vercel ls

# Rollback to previous deployment
npx vercel rollback [deployment-url]
```

#### 2. GitHub Rollback
```bash
# Revert commit
git revert HEAD

# Force rollback to specific commit
git reset --hard <commit-hash>
git push --force-with-lease origin main
```

### Database Rollback

#### 1. Migration Rollback
```sql
-- Create rollback migration
-- migrations/20240120000001_rollback_user_preferences.sql
DROP TABLE IF EXISTS user_preferences;
```

#### 2. Point-in-Time Recovery
```bash
# Restore database to specific time (Supabase Pro)
# Use Supabase dashboard for point-in-time recovery
```

### Emergency Procedures

1. **Critical Bug in Production**
   ```bash
   # Immediate rollback
   npx vercel rollback
   
   # Create hotfix branch
   git checkout -b hotfix/critical-bug
   # Fix bug
   # Deploy hotfix
   ```

2. **Database Issues**
   ```bash
   # Check database status
   npx supabase status
   
   # Contact Supabase support if needed
   # Implement emergency read-only mode
   ```

## 🔧 Troubleshooting

### Common Deployment Issues

#### 1. Build Failures

```bash
# Clear cache
rm -rf .next node_modules
npm install

# Check for TypeScript errors
npm run type-check

# Check for linting errors
npm run lint
```

#### 2. Environment Variable Issues

```bash
# Verify environment variables
npx vercel env ls

# Add missing variables
npx vercel env add VARIABLE_NAME
```

#### 3. Database Connection Issues

```bash
# Test database connection
npx supabase test db

# Check connection string
npx supabase status
```

#### 4. Performance Issues

```bash
# Analyze bundle size
npm run analyze

# Check for memory leaks
npm run test:memory

# Monitor performance
# Use Vercel Analytics dashboard
```

### Debugging Production Issues

1. **Check Application Logs**
   - Vercel Function logs
   - Sentry error reports
   - Browser console errors

2. **Database Performance**
   - Supabase dashboard metrics
   - Slow query analysis
   - Connection pool status

3. **External Service Status**
   - Stripe dashboard
   - Email service status
   - Third-party API status

### Getting Help

- **Vercel Support**: [vercel.com/support](https://vercel.com/support)
- **Supabase Support**: [supabase.com/support](https://supabase.com/support)
- **GitHub Issues**: [Create an issue](https://github.com/Mahas1234/GigSync/issues)
- **Community**: [GitHub Discussions](https://github.com/Mahas1234/GigSync/discussions)

---

**Happy deploying! 🚀**