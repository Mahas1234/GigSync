# Contributing to GigSync

Thank you for your interest in contributing to GigSync! 🎉 We welcome contributions from developers, designers, students, and anyone passionate about creating better opportunities for student employment.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Types of Contributions](#types-of-contributions)
- [Development Setup](#development-setup)
- [Development Workflow](#development-workflow)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation Standards](#documentation-standards)
- [Issue Reporting](#issue-reporting)
- [Community Guidelines](#community-guidelines)

## 📜 Code of Conduct

This project adheres to our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to [bejugammahaswi@gmail.com](mailto:bejugammahaswi@gmail.com).

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ and npm/yarn
- Git
- Basic understanding of React/Next.js
- Supabase account (for database features)

### First-time Setup

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/your-username/GigSync.git
   cd GigSync
   ```
3. **Add the original repository as upstream**:
   ```bash
   git remote add upstream https://github.com/Mahas1234/GigSync.git
   ```
4. **Install dependencies**:
   ```bash
   npm install
   ```
5. **Set up environment variables**:
   ```bash
   cp .env.example .env.local
   # Edit .env.local with your credentials
   ```

## 🎯 Types of Contributions

We welcome various types of contributions:

### 🐛 Bug Reports
- Use the bug report template
- Include steps to reproduce
- Provide system information
- Add screenshots if applicable

### 💡 Feature Requests
- Use the feature request template
- Explain the use case
- Consider the impact on students
- Provide mockups if possible

### 🔧 Code Contributions
- Bug fixes
- New features
- Performance improvements
- UI/UX enhancements
- Test coverage improvements

### 📚 Documentation
- API documentation
- Code comments
- User guides
- Developer tutorials

### 🎨 Design Contributions
- UI/UX improvements
- Accessibility enhancements
- Mobile responsiveness
- Brand assets

## 🛠️ Development Setup

### Environment Configuration

Create a `.env.local` file with the following variables:

```env
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# Authentication
NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=http://localhost:3000

# Payment Processing (Development)
STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
STRIPE_SECRET_KEY=your_stripe_secret_key

# Email Service
RESEND_API_KEY=your_resend_api_key

# Analytics (Optional)
NEXT_PUBLIC_VERCEL_ANALYTICS_ID=your_analytics_id
```

### Database Setup

```bash
# Run database migrations
npm run db:migrate

# Seed development data
npm run db:seed
```

### Development Commands

```bash
# Start development server
npm run dev

# Run tests
npm test

# Run tests in watch mode
npm run test:watch

# Lint code
npm run lint

# Format code
npm run format

# Type checking
npm run type-check

# Build for production
npm run build
```

## 🔄 Development Workflow

### Branch Naming Convention

- `feature/description` - New features
- `fix/description` - Bug fixes
- `docs/description` - Documentation updates
- `refactor/description` - Code refactoring
- `test/description` - Test additions/improvements

### Commit Message Format

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): description

[optional body]

[optional footer]
```

Examples:
```
feat(auth): add student email verification
fix(gigs): resolve payment processing error
docs(api): update authentication endpoints
test(payments): add unit tests for stripe integration
```

### Development Process

1. **Create a new branch** from main:
   ```bash
   git checkout main
   git pull upstream main
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes** following our coding standards

3. **Test your changes**:
   ```bash
   npm run test
   npm run lint
   npm run type-check
   ```

4. **Commit your changes**:
   ```bash
   git add .
   git commit -m "feat(scope): description"
   ```

5. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create a Pull Request** on GitHub

## 📝 Pull Request Process

### Before Creating a PR

- [ ] Code follows our style guidelines
- [ ] Tests pass locally
- [ ] Documentation is updated
- [ ] Commits follow our format
- [ ] Branch is up to date with main

### PR Description Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots (if applicable)
Add screenshots for UI changes

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] Tests added/updated
```

### Review Process

1. **Automated checks** must pass
2. **Code review** by maintainers
3. **Testing** on staging environment
4. **Approval** by at least one maintainer
5. **Merge** into main branch

## 🎨 Coding Standards

### TypeScript/JavaScript

- Use TypeScript for all new code
- Follow ESLint and Prettier configurations
- Use meaningful variable and function names
- Add JSDoc comments for complex functions
- Prefer const over let, avoid var

### React/Next.js

- Use functional components with hooks
- Follow React best practices
- Use proper state management (Zustand/Redux)
- Implement proper error boundaries
- Optimize for performance (useMemo, useCallback)

### CSS/Styling

- Use Tailwind CSS for styling
- Follow mobile-first approach
- Ensure accessibility (WCAG guidelines)
- Use semantic HTML elements
- Test across different devices

### File Structure

```
src/
├── components/
│   ├── ui/           # Reusable UI components
│   ├── forms/        # Form components
│   └── layout/       # Layout components
├── pages/            # Next.js pages
├── hooks/            # Custom React hooks
├── utils/            # Utility functions
├── types/            # TypeScript definitions
├── stores/           # State management
└── styles/           # Global styles
```

## 🧪 Testing Guidelines

### Testing Strategy

- **Unit Tests**: Individual functions and components
- **Integration Tests**: Component interactions
- **End-to-End Tests**: User workflows
- **Performance Tests**: Load and stress testing

### Writing Tests

```typescript
// Component test example
import { render, screen } from '@testing-library/react'
import { GigCard } from '../GigCard'

describe('GigCard', () => {
  it('displays gig information correctly', () => {
    const mockGig = {
      title: 'Web Development',
      description: 'Build a landing page',
      price: 50
    }
    
    render(<GigCard gig={mockGig} />)
    
    expect(screen.getByText('Web Development')).toBeInTheDocument()
    expect(screen.getByText('$50')).toBeInTheDocument()
  })
})
```

### Test Coverage

- Maintain minimum 80% code coverage
- Focus on critical user paths
- Test error scenarios
- Include accessibility tests

## 📖 Documentation Standards

### Code Documentation

- Use TypeScript for type safety
- Add JSDoc comments for public APIs
- Document complex algorithms
- Include usage examples

### API Documentation

- Use OpenAPI/Swagger specifications
- Include request/response examples
- Document error codes
- Provide authentication details

## 🐛 Issue Reporting

### Bug Reports

Include the following information:

1. **Environment**: OS, browser, Node.js version
2. **Steps to reproduce**: Clear, numbered steps
3. **Expected behavior**: What should happen
4. **Actual behavior**: What actually happens
5. **Screenshots/Videos**: If applicable
6. **Additional context**: Logs, error messages

### Feature Requests

Include the following information:

1. **Problem statement**: What issue does this solve?
2. **Proposed solution**: How should it work?
3. **Use cases**: Who would benefit?
4. **Alternatives**: Other solutions considered
5. **Implementation notes**: Technical considerations

## 🤝 Community Guidelines

### Communication

- Be respectful and inclusive
- Use clear, concise language
- Ask questions when unsure
- Help others when possible
- Follow our Code of Conduct

### Getting Help

- Check existing issues and documentation
- Ask in GitHub Discussions
- Join our community channels
- Attend office hours (if available)

### Recognition

Contributors are recognized through:

- GitHub contributor graphs
- Release notes mentions
- Special contributor badges
- Community shoutouts

## 🎓 Student Contributors

We especially encourage student contributions! Here are some beginner-friendly areas:

- **Documentation improvements**
- **UI/UX enhancements**
- **Test coverage**
- **Accessibility improvements**
- **Performance optimizations**

Look for issues labeled with `good-first-issue` or `student-friendly`.

## 📞 Contact

- **Maintainer**: [bejugammahaswi@gmail.com](mailto:bejugammahaswi@gmail.com)
- **GitHub Issues**: [Report bugs or questions](https://github.com/Mahas1234/GigSync/issues)
- **Discussions**: [Community discussions](https://github.com/Mahas1234/GigSync/discussions)

Thank you for contributing to GigSync! Together, we're building something amazing for the student community. 🚀

---

**Happy coding!** 💻✨