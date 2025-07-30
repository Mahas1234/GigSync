# GigSync - A Gig Sharing Platform

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![GitHub Stars](https://img.shields.io/github/stars/Mahas1234/GigSync?style=social)](https://github.com/Mahas1234/GigSync/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/Mahas1234/GigSync)](https://github.com/Mahas1234/GigSync/issues)
[![GitHub Pull Requests](https://img.shields.io/github/issues-pr/Mahas1234/GigSync)](https://github.com/Mahas1234/GigSync/pulls)

GigSync is a modern gig-sharing platform specifically designed for students to earn money through flexible, part-time gigs. It connects students with verified employers offering hourly jobs, freelance work, and campus tasks, helping them gain valuable experience while earning extra income.

## 🎯 Vision

Empowering students to balance their studies with meaningful work opportunities while building their professional skills and network.

## ✨ Features

### 🔥 Core Features

- **🎪 Gig Creation & Acceptance** - Students can browse and accept gigs that match their skills and schedule
- **💬 Real-time Messaging** - Seamless communication between students and employers
- **👤 Student Profiles & Ratings** - Build reputation through verified reviews and portfolio showcases
- **💳 Secure Payments** - Safe and reliable payment processing with multiple options
- **📍 Location-Based Matching** - Find opportunities near campus or remote work options
- **🏷️ Smart Categories & Filters** - Advanced search by skills, time commitment, pay rate, and more

### 🚀 Advanced Features

- **⚡ Instant Booking** - Quick acceptance system for urgent gigs
- **🎨 Student Portfolio** - Showcase previous work, skills, and achievements
- **🤝 Skill-Based Matching** - AI-powered recommendations based on student profiles
- **🔔 Smart Notifications** - Real-time updates on applications, messages, and payments
- **🛡️ Admin Dashboard** - Comprehensive management for platform oversight
- **🎁 Referral System** - Earn rewards for bringing new users to the platform
- **⚖️ Dispute Resolution** - Fair conflict resolution system with mediation
- **🌍 Multi-Language Support** - Accessible for international students
- **🔌 Open APIs** - Third-party integrations for university systems

## 🛠️ Tech Stack

### Frontend
- **Framework**: React.js with Next.js 14+
- **Styling**: Tailwind CSS
- **State Management**: Zustand / Redux Toolkit
- **UI Components**: Shadcn/ui
- **Maps**: Mapbox GL JS
- **Real-time**: Socket.io-client

### Backend
- **Runtime**: Node.js 18+
- **Framework**: Express.js / Fastify
- **Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **File Storage**: Supabase Storage
- **Real-time**: Socket.io
- **Email**: Resend / SendGrid

### Payment & Security
- **Payments**: Stripe, PayPal, or crypto solutions
- **Security**: JWT, bcrypt, rate limiting
- **Validation**: Zod / Joi

### DevOps & Hosting
- **Hosting**: Vercel (frontend) / Railway (backend)
- **CI/CD**: GitHub Actions
- **Monitoring**: Sentry
- **Analytics**: Vercel Analytics

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ and npm/yarn
- Git
- Supabase account (for database and auth)

### Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Mahas1234/GigSync.git
   cd GigSync
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   ```

3. **Environment Configuration**
   ```bash
   cp .env.example .env.local
   ```
   Update the following variables:
   ```env
   NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
   SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
   NEXTAUTH_SECRET=your_nextauth_secret
   NEXTAUTH_URL=http://localhost:3000
   ```

4. **Database Setup**
   ```bash
   npm run db:setup
   npm run db:seed
   ```

5. **Start Development Servers**
   ```bash
   # Frontend (Next.js)
   npm run dev
   
   # Backend API (if separate)
   npm run server
   ```

6. **Open in browser**
   Navigate to [http://localhost:3000](http://localhost:3000)

## 📚 Documentation

- [Development Guide](./docs/DEVELOPMENT.md)
- [API Documentation](./docs/API.md)
- [Architecture Overview](./docs/ARCHITECTURE.md)
- [Contributing Guidelines](./CONTRIBUTING.md)
- [Deployment Guide](./docs/DEPLOYMENT.md)

## 🤝 Contributing

We welcome contributions from developers, designers, and students! Please read our [Contributing Guidelines](./CONTRIBUTING.md) before getting started.

### Quick Contribution Steps

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 🐛 Issue Reporting

Found a bug or have a feature request? Please check our [issue tracker](https://github.com/Mahas1234/GigSync/issues) and create a new issue if needed.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🌟 Support the Project

If you find GigSync helpful, please consider:
- ⭐ Starring the repository
- 🐛 Reporting bugs
- 💡 Suggesting new features
- 🤝 Contributing code
- 📢 Sharing with other developers

## 📞 Contact & Community

- **Email**: [bejugammahaswi@gmail.com](mailto:bejugammahaswi@gmail.com)
- **GitHub Issues**: [Report bugs or request features](https://github.com/Mahas1234/GigSync/issues)
- **Discussions**: [Join community discussions](https://github.com/Mahas1234/GigSync/discussions)

## 🙏 Acknowledgments

- Built with ❤️ for the student community
- Inspired by the need for flexible student employment
- Thanks to all contributors and supporters

---

**Made with 💙 by students, for students**

