# 🩰 Dance Studio Management System called Project Archie

A full-stack Next.js 15 application built with Claude Code 2.0's multi-agent architecture demonstrating how to create production-ready business applications from a Product Requirements Document (PRD).

**Built with:** Claude Code 2.0, Sonnet 4.5, custom slash commands, parallel sub-agents, and design-first workflow.

---

## 🎯 Project Overview

This repository showcases a **reusable multi-agent framework** for building full-stack applications, demonstrated through a real-world dance studio management system.

### Why This Framework Matters

Traditional AI-assisted development struggles with:
- **Context window exhaustion** - Agent forgets early decisions
- **Agent conflicts** - Multiple agents overwriting each other's work
- **Lack of reusability** - Every project starts from scratch

This framework solves these with:
- **Two-phase architecture** - Design phase generates specs → Implementation phase reads specs
- **Parallel agent execution** - 7 specialists working simultaneously after UI design
- **File-based coordination** - Each agent writes to isolated folders (no conflicts)
- **Custom slash commands** - Repeatable workflows (`/dev:design-app`, `/dev:implement-app`)

---

## 📁 Repository Structure

```
dance-studio-app/
├── .claude/                          # ⭐ Multi-Agent Framework (Reusable)
│   ├── agents/
│   │   ├── coordination/
│   │   │   └── orchestrator.md              # Multi-agent coordinator
│   │   ├── implementation/
│   │   │   ├── react-typescript-specialist.md
│   │   │   └── stagehand-expert.md
│   │   └── research-planning/
│   │       ├── ui-designer.md               # Visual design
│   │       ├── shadcn-expert.md             # Component selection
│   │       ├── system-architect.md          # Service layer design
│   │       ├── nextjs-expert.md             # Next.js patterns
│   │       ├── stripe-expert.md             # ⭐ Payment processing
│   │       ├── calendar-expert.md           # ⭐ Scheduling systems
│   │       └── auth-expert.md               # ⭐ Multi-role auth
│   ├── commands/
│   │   ├── dev/
│   │   │   ├── design-app.md                # Phase 1: Design workflow
│   │   │   └── implement-app.md             # Phase 2: Implementation workflow
│   │   └── design/
│   │       └── setup-folders.md             # Initialize output folders
│   ├── outputs/                      # Generated design specs
│   └── settings.json
├── app/                              # Next.js 15 Application
│   ├── app/
│   │   ├── (auth)/                  # Authentication routes
│   │   ├── (dashboard)/             # Role-specific dashboards
│   │   ├── (public)/                # Landing, class listings
│   │   ├── api/                     # API routes
│   │   └── prisma/                  # Database schema
│   ├── components/                   # UI components (shadcn/ui)
│   ├── lib/
│   │   ├── services/
│   │   │   ├── stripe.service.ts           # Payment processing
│   │   │   ├── calendar.service.ts         # Scheduling
│   │   │   ├── notification.service.ts     # Email/SMS
│   │   │   └── attendance.service.ts
│   │   ├── auth.ts                         # NextAuth.js config
│   │   ├── permissions.ts                  # RBAC system
│   │   └── prisma.ts
│   ├── package.json
│   ├── README.md
│   └── DEPLOYMENT.md
├── docs/
│   └── PRD.md                        # Product Requirements Document
├── CLAUDE.md                          # Project context for Claude Code
└── README.md                          # This file
```

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- PostgreSQL 16+ (or Supabase account)
- Stripe account ([Get API keys](https://stripe.com))
- Google OAuth credentials ([Setup guide](https://next-auth.js.org/providers/google))
- Claude Code 2.0 installed

### 1. Clone & Install

```bash
git clone https://github.com/yourusername/dance-studio-app.git
cd dance-studio-app/app
npm install
```

### 2. Environment Setup

```bash
cp .env.example .env.local
```

Edit `.env.local`:

```env
# Database
DATABASE_URL="postgresql://user:pass@localhost:5432/dancestudio"

# NextAuth
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="generate-with: openssl rand -base64 32"

# Google OAuth
GOOGLE_CLIENT_ID="your-client-id.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="your-secret"

# Stripe
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_PUBLISHABLE_KEY="pk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# Cron Jobs
CRON_SECRET="random-secret-for-cron-endpoints"

# Email (optional)
EMAIL_SERVER="smtp://user:pass@smtp.sendgrid.net:587"
EMAIL_FROM="noreply@yourstudio.com"
```

### 3. Database Setup

```bash
# Run Prisma migrations
npx prisma migrate dev --name init
npx prisma generate

# Optional: Seed demo data
npx prisma db seed
```

### 4. Start Development Server

```bash
npm run dev
```

Visit **http://localhost:3000** 🎉

### 5. Create Admin Account

```bash
# Sign up via UI, then promote to admin:
npx prisma studio
# Navigate to User table → Edit your user → Change role to "admin"
```

---

## 🎓 How This Was Built

### The Two-Phase Workflow

#### Phase 1: Design (`/dev:design-app docs/PRD.md`)

**Orchestrator coordinates 8 specialized agents:**

```
Sequential:  ui-designer.md
             └─ Creates wireframes (all agents need this)

Parallel:    ┌─ shadcn-expert.md        (Component selection)
             ├─ stripe-expert.md         (Payment integration)
             ├─ calendar-expert.md       (Scheduling system)
             ├─ auth-expert.md           (Multi-role auth)
             ├─ system-architect.md      (Service layer)
             ├─ nextjs-expert.md         (App Router design)
             └─ stagehand-expert.md      (Test specs)

Sequential:  orchestrator.md
             └─ Synthesizes MANIFEST.md
```

**Output:** Comprehensive design specifications in `.claude/outputs/design/`

#### Phase 2: Implementation (`/dev:implement-app design-folder/ app/`)

**Single agent reads ALL design specs and implements:**

1. **Project Setup** - Next.js, dependencies, Prisma
2. **Authentication** - NextAuth.js, role-based middleware
3. **Services** - Stripe, Calendar, Notification
4. **API Routes** - Class management, enrollment, payments, webhooks
5. **UI Components** - Dashboards, forms, calendars
6. **Integration** - End-to-end flows working
7. **Production** - Deployment, docs, monitoring

**Output:** Deployable application in `app/`

---

## 🎨 Key Features

### For Students/Parents
- ✅ Browse classes by age, skill level, schedule
- ✅ Online registration with Stripe Checkout
- ✅ View personal schedule and attendance
- ✅ Manage subscriptions and payments
- ✅ Family accounts (multiple students)

### For Instructors
- ✅ View teaching schedule
- ✅ Mark attendance (QR code or manual)
- ✅ Add progress notes for students
- ✅ Message students/parents

### For Studio Admins
- ✅ Create recurring classes (RRULE)
- ✅ Manage instructors and students
- ✅ Financial reports and analytics
- ✅ Refund processing
- ✅ Waitlist management

---

## 🏗️ Architecture Highlights

### Service Layer Pattern

**Three core services (singletons):**

```typescript
// StripeService - Payment processing
const stripe = StripeService.getInstance()
await stripe.createSubscription(customerId, priceId)

// CalendarService - Recurring schedule management
const calendar = CalendarService.getInstance()
await calendar.generateOccurrences(classId, 90) // 90 days

// NotificationService - Email/SMS
const notifier = NotificationService.getInstance()
await notifier.sendReminder(userId, classOccurrence)
```

### Database Schema (PostgreSQL + Prisma)

**Key Tables:**
- `users` - All accounts (admin, instructor, student, parent)
- `family_accounts` - Parent-student relationships
- `classes` - Recurring class definitions (RRULE)
- `class_occurrences` - Materialized instances (90 days ahead)
- `enrollments` - Student-class relationships
- `payments` - Stripe payment records
- `subscriptions` - Recurring billing
- `attendance` - Check-in records

### Recurring Schedule Magic

**Store pattern once, generate instances:**

```typescript
// Define class once
const class = {
  name: "Ballet Beginner",
  recurrence_rule: "FREQ=WEEKLY;BYDAY=MO,WE", // Monday & Wednesday
  start_time: "17:00:00",
  duration_minutes: 60
}

// Cron job generates 90 days of occurrences
// Output: [
//   { start_time: "2025-01-06T22:00:00Z", end_time: "2025-01-06T23:00:00Z" },
//   { start_time: "2025-01-08T22:00:00Z", end_time: "2025-01-08T23:00:00Z" },
//   ...
// ]
```

### Multi-Role Authorization

**Permission-based access control:**

```typescript
// Define permissions once
const PERMISSIONS = {
  'classes:create': ['admin'],
  'attendance:mark': ['admin', 'instructor', 'front_desk'],
  'payments:view-all': ['admin']
}

// Check in API routes
await requirePermission('classes:create') // Throws if unauthorized

// Check in UI components
const { can } = usePermissions()
{can('classes:edit') && <button>Edit Class</button>}
```

---

## 💰 Cost Breakdown

### Monthly Operating Costs

| Service | Usage | Cost |
|---------|-------|------|
| **Vercel** | Hobby plan (personal project) | $0 |
| **Supabase** | 500MB DB, 2GB bandwidth | $0 |
| **Stripe** | 2.9% + $0.30 per transaction | Variable |
| **Resend** | 3,000 emails/month | $0 |
| **Total** | (Assuming $1,000 revenue/month) | ~$30/month |

**Scales to:**
- 200 students
- 50 classes/week
- 1,000 enrollments

---

## 🔧 Development Commands

```bash
# Development
npm run dev              # Start dev server (port 3000)
npm run build            # Production build
npm start                # Start production server
npm run lint             # ESLint

# Database
npx prisma studio        # Open database GUI
npx prisma migrate dev   # Create migration
npx prisma generate      # Update Prisma client

# Testing (Stagehand)
npm run test:e2e         # Run E2E tests

# Cron Jobs (manual trigger)
curl -X POST http://localhost:3000/api/cron/generate-occurrences \
  -H "Authorization: Bearer $CRON_SECRET"
```

---

## 🎯 Adapting This Framework for Your Project

### Want to build a different app? Here's how:

#### 1. Copy `.claude/` folder to your project

```bash
cp -r .claude /path/to/your-project/
```

#### 2. Customize agents for your domain

**Keep core agents:**
- orchestrator.md (coordination)
- ui-designer.md (wireframes)
- shadcn-expert.md (components)
- system-architect.md (integration)
- nextjs-expert.md (Next.js patterns)
- react-typescript-specialist.md (implementation)

**Replace domain-specific agents:**
```diff
- stripe-expert.md
+ paypal-expert.md

- calendar-expert.md
+ booking-expert.md

- auth-expert.md (if using same NextAuth pattern, keep it!)
```

**Add your own specialists:**
- `blockchain-expert.md` for Web3 apps
- `ml-expert.md` for AI/ML features
- `video-expert.md` for video platforms
- `cms-expert.md` for content-heavy sites

#### 3. Write your PRD

Define:
- Features and user stories
- Target users and roles
- Tech stack preferences
- Success metrics

#### 4. Run the workflow

```bash
# Design phase
/dev:design-app docs/your-prd.md

# Implementation phase
/dev:implement-app .claude/outputs/design/projects/your-app/[timestamp] app/
```

#### 5. Deploy and iterate

The framework gives you a **solid foundation**. Polish based on user feedback!

---

## 📚 Documentation

- **CLAUDE.md** - Project context for Claude Code (auto-loaded)
- **LESSON.md** - Complete tutorial on building with this framework
- **app/README.md** - Application-specific setup guide
- **app/DEPLOYMENT.md** - Production deployment instructions
- **docs/PRD.md** - Original Product Requirements Document

---

## 🤖 Claude Code 2.0 Features Showcased

### New in 2.0
- `/rewind` - Revert code and conversation (Press Escape twice)
- `/usage` - Real-time API usage tracking
- `/context` - View context usage breakdown
- Background tasks - Run servers while Claude Code available
- Sub-agent slash commands - Agents invoke custom commands as tools

### Advanced Patterns
- **Multi-agent orchestration** - 8 agents coordinated by orchestrator
- **Parallel execution** - 7 agents running simultaneously
- **File-based outputs** - No agent conflicts
- **Two-phase architecture** - Maximizes context windows
- **Custom slash commands** - Repeatable workflows

---

## 🎯 Known Limitations

- **Nested comment threads** - Only top-level comments analyzed in feedback (Phase 2 feature)
- **Multi-location support** - Currently single studio (could add location field)
- **Video library** - Not implemented (Phase 2 feature)
- **Mobile app** - Web-only (React Native app planned for Phase 3)

---

## 🚢 Deployment

### Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
cd app/
vercel

# Set environment variables in Vercel dashboard
vercel env add DATABASE_URL
vercel env add NEXTAUTH_SECRET
# ... etc
```

See **app/DEPLOYMENT.md** for complete guide including:
- Database setup (Supabase/Railway)
- Stripe webhook configuration
- Cron job setup
- Domain configuration

---

## 🤝 Contributing

This is a demonstration project. Feel free to:
- Fork for your own projects
- Adapt the multi-agent framework
- Submit issues for bugs
- Share your own workflows

---

## 📄 License

MIT License - See LICENSE file

---

## 🙏 Acknowledgments

- **Claude Code 2.0** by Anthropic
- **Sonnet 4.5** (claude-sonnet-4-5-20250929)
- **Next.js 15** by Vercel
- **shadcn/ui** by shadcn
- **Stripe** for payments
- **Prisma** for database ORM
- Inspired by production dance studios worldwide

---

## 🔗 Resources

- [Claude Code Documentation](https://docs.claude.com/code)
- [Builder Pack (Full Framework)](Link to video description)
- [Next.js 15 Docs](https://nextjs.org/docs)
- [Stripe Integration Guide](https://stripe.com/docs)
- [Prisma Documentation](https://www.prisma.io/docs)

---

**Built entirely using Claude Code 2.0's multi-agent architecture from a single PRD.**

Ready to build your own? Copy the `.claude/` folder and start with your PRD! 🚀# Project-Archie