# Universal Service Business Platform Fullstack Architecture Document

## Introduction

This document outlines the complete fullstack architecture for **Universal Service Business Platform**, including backend systems, frontend implementation, and their integration. It serves as the single source of truth for AI-driven development, ensuring consistency across the entire technology stack.

This unified approach combines what would traditionally be separate backend and frontend architecture documents, streamlining the development process for modern fullstack applications where these concerns are increasingly intertwined.

### Starter Template or Existing Project

Based on review of the PRD and project brief, this is a **greenfield project** with specific technology preferences outlined:

**Preferred Technologies from PRD:**
- Frontend: React/Next.js with Webstudio integration
- Backend: Node.js/TypeScript with serverless functions
- Infrastructure: Vercel/Netlify for frontend, AWS/Railway for backend
- Database: PostgreSQL with Redis caching

**Recommended Starter Template:**
Given the technology stack and SaaS platform requirements, I recommend the **T3 Stack** (TypeScript, tRPC, Tailwind, Prisma) as the foundation, which provides:
- Next.js with TypeScript for type safety
- tRPC for end-to-end type safety between frontend/backend
- Prisma for database management
- Perfect foundation for Cal.com and Stripe integrations

**Constraints from Starter Choice:**
- Must build Webstudio integration as custom component
- T3 Stack provides serverless-ready architecture
- Built-in auth patterns that can integrate with service business requirements

### Change Log
| Date | Version | Description | Author |
|------|---------|-------------|---------|
| 2025-09-13 | 1.0 | Initial architecture creation | Architect Agent |

## High Level Architecture

### Technical Summary

The Universal Service Business Platform employs a **modern serverless-first fullstack architecture** built on the T3 Stack foundation with strategic third-party integrations. The frontend leverages Next.js 14+ with App Router for optimal performance and SEO, while the backend utilizes serverless functions for cost-effective scalability. **Webstudio integration provides the template engine** for generating custom branded sites, while **Cal.com handles booking infrastructure** and **Stripe Connect manages payment processing**. The platform deploys on Vercel for seamless CI/CD with PostgreSQL/Redis on Railway for data persistence and caching. This architecture achieves the PRD's 30-minute setup goal through automated template generation while maintaining the flexibility to scale from solo practitioners to enterprise teams.

### Platform and Infrastructure Choice

**Platform:** Vercel (Frontend) + Railway (Backend Services)
**Key Services:** Vercel Edge Functions, Railway PostgreSQL, Railway Redis, Vercel Analytics
**Deployment Host and Regions:** Vercel Global Edge Network, Railway US-East/US-West regions

### Repository Structure

**Structure:** Turborepo-managed monorepo
**Monorepo Tool:** Turborepo (Vercel's tool, optimal for Next.js deployment)
**Package Organization:** Apps (web, api, admin) + Packages (shared, ui, webstudio-integration, cal-integration, stripe-integration)

### High Level Architecture Diagram

```mermaid
graph TB
    User[👤 Service Business Owner] --> Web[🌐 Next.js Frontend<br/>Vercel Edge]
    Customer[👥 End Customers] --> Web

    Web --> API[⚡ tRPC API<br/>Vercel Functions]
    Web --> Webstudio[🎨 Webstudio Templates<br/>React Components]

    API --> DB[(🗄️ PostgreSQL<br/>Railway)]
    API --> Cache[(⚡ Redis<br/>Railway)]
    API --> Cal[📅 Cal.com API<br/>External]
    API --> Stripe[💳 Stripe Connect<br/>External]

    Web --> CDN[📦 Vercel CDN<br/>Global Edge]

    Admin[🔧 Admin Dashboard] --> API
    Webhooks[📡 Webhooks] --> API

    style Web fill:#e1f5fe
    style API fill:#f3e5f5
    style DB fill:#e8f5e8
    style Cal fill:#fff3e0
    style Stripe fill:#fce4ec
```

### Architectural Patterns

- **Jamstack Architecture:** Static generation with serverless APIs for optimal performance and SEO - _Rationale:_ Critical for service business websites that need to rank well and load fast for mobile users
- **API-First Design:** All business logic exposed through tRPC APIs for future mobile/integrations - _Rationale:_ Enables white-label expansion and third-party integrations mentioned in post-MVP vision
- **Component-Based UI:** Reusable React components with TypeScript and Storybook - _Rationale:_ Template system requires high reusability across industry verticals
- **Event-Driven Architecture:** Webhooks from Cal.com/Stripe trigger business logic flows - _Rationale:_ Enables real-time booking confirmations and payment processing
- **Repository Pattern:** Abstract data access with Prisma ORM - _Rationale:_ Enables testing and future multi-database support for enterprise features
- **Micro-Frontend Pattern:** Webstudio templates as encapsulated React components - _Rationale:_ Allows independent template development and A/B testing capabilities

## Tech Stack

### Technology Stack Table

| Category | Technology | Version | Purpose | Rationale |
|----------|------------|---------|---------|-----------|
| Frontend Language | TypeScript | 5.3+ | Type-safe frontend development | Critical for shared types with backend and third-party integrations |
| Frontend Framework | Next.js | 15.1+ | React framework with App Router | App Router for optimal performance, built-in API routes, Vercel optimization |
| UI Component Library | Radix UI + shadcn/ui | Latest | Accessible, customizable components | Matches Webstudio's design system approach, accessibility compliance |
| State Management | Zustand + TanStack Query | 4.4+ / 5.0+ | Client state + server state | Lightweight client state, powerful server state with caching for API calls |
| Backend Language | TypeScript | 5.3+ | Type-safe backend development | Shared types across stack, reduces integration errors |
| Backend Framework | Next.js API Routes + tRPC | 14.1+ / 10.45+ | Type-safe API layer | End-to-end type safety, eliminates API documentation drift |
| API Style | tRPC | 10.45+ | Type-safe RPC calls | Perfect for monorepo, eliminates REST boilerplate, auto-completion |
| Database | PostgreSQL | 15+ | Primary data store | Mature, ACID compliance for financial data, JSON support for flexible schemas |
| Cache | Redis | 7.0+ | Session and data caching | Essential for template generation performance and user sessions |
| File Storage | Vercel Blob | Latest | Template assets and user uploads | Integrated with Vercel, CDN-backed, cost-effective for MVP |
| Authentication | NextAuth.js | 4.24+ | Business owner authentication | Mature, supports multiple providers, integrates with Next.js |
| Frontend Testing | Vitest + Testing Library | 1.0+ / 14.0+ | Component and unit testing | Faster than Jest, better Vite integration |
| Backend Testing | Vitest + MSW | 1.0+ / 2.0+ | API and integration testing | Consistent tooling across stack, mock service worker for external APIs |
| E2E Testing | Playwright | 1.40+ | End-to-end user flows | Critical for booking/payment flows, cross-browser support |
| Build Tool | Turborepo | 1.10+ | Monorepo build orchestration | Optimal caching, parallel builds, Vercel integration |
| Bundler | Next.js (Turbopack) | 14.1+ | Frontend bundling | Native Next.js bundler, fastest development experience |
| IaC Tool | Terraform | 1.6+ | Infrastructure as code | Railway + Vercel configuration, environment management |
| CI/CD | GitHub Actions + Vercel | Latest | Automated deployment | Native Vercel integration, preview deployments |
| Monitoring | Vercel Analytics + Sentry | Latest | Performance and error tracking | Built-in performance metrics, comprehensive error tracking |
| Logging | Pino + Vercel Functions | 8.16+ | Structured logging | High-performance logging, Vercel Functions compatible |
| CSS Framework | Tailwind CSS | 3.4+ | Utility-first styling | Matches Webstudio approach, rapid template development |

### Key Integration Technologies

| Category | Technology | Version | Purpose | Rationale |
|----------|------------|---------|---------|-----------|
| Template Engine | Webstudio SDK | Latest | Dynamic template generation | Core differentiator for custom branded sites |
| Booking System | Cal.com API | Latest | Scheduling infrastructure | Proven booking system, extensive customization |
| Payment Processing | Stripe Connect | Latest | Multi-tenant payments | Industry standard, perfect for marketplace model |
| Email Service | Resend | Latest | Transactional emails | Developer-friendly, reliable delivery |
| Database ORM | Prisma | 5.7+ | Type-safe database access | Perfect TypeScript integration, migration management |

## Data Models

### Business

**Purpose:** Represents the service business using the platform (massage therapists, consultants, etc.)

**Key Attributes:**
- id: string (UUID) - Unique business identifier
- name: string - Business display name
- slug: string - URL-friendly identifier for custom domains
- industry: enum - Wellness, consulting, trades, creative, etc.
- ownerId: string - Reference to business owner user account
- templateId: string - Selected Webstudio template
- customization: JSON - Template customization settings (colors, fonts, branding)
- status: enum - Draft, active, suspended, closed
- calComIntegration: JSON - Cal.com configuration and credentials
- stripeAccountId: string - Stripe Connect account identifier
- loyaltySettings: JSON - Points values, redemption rules, token configuration

**TypeScript Interface:**
```typescript
interface Business {
  id: string;
  name: string;
  slug: string;
  industry: BusinessIndustry;
  ownerId: string;
  templateId: string;
  customization: BusinessCustomization;
  status: BusinessStatus;
  calComIntegration: CalComConfig;
  stripeAccountId: string;
  loyaltySettings: LoyaltyConfig;
  createdAt: Date;
  updatedAt: Date;
}

type BusinessIndustry = 'wellness' | 'consulting' | 'trades' | 'creative' | 'hospitality';
type BusinessStatus = 'draft' | 'active' | 'suspended' | 'closed';
```

**Relationships:**
- One-to-many with Services (business offerings)
- One-to-many with Bookings (customer appointments)
- One-to-many with Customers (business customer base)
- One-to-one with User (business owner account)

### Service

**Purpose:** Individual services offered by a business (60-min massage, business consultation, etc.)

**Key Attributes:**
- id: string (UUID) - Unique service identifier
- businessId: string - Parent business reference
- name: string - Service display name
- description: text - Detailed service description
- duration: number - Service duration in minutes
- price: number - Service price in cents
- calComEventTypeId: string - Cal.com event type mapping
- loyaltyPointsEarned: number - Points awarded per booking
- isActive: boolean - Service availability status
- category: string - Service grouping (therapeutic, consultation, etc.)

**TypeScript Interface:**
```typescript
interface Service {
  id: string;
  businessId: string;
  name: string;
  description: string;
  duration: number; // minutes
  price: number; // cents
  calComEventTypeId: string;
  loyaltyPointsEarned: number;
  isActive: boolean;
  category: string;
  createdAt: Date;
  updatedAt: Date;
}
```

**Relationships:**
- Many-to-one with Business
- One-to-many with Bookings

### Customer

**Purpose:** End customers who book services (separate from business owners)

**Key Attributes:**
- id: string (UUID) - Unique customer identifier
- email: string - Customer email (primary identifier)
- firstName: string - Customer first name
- lastName: string - Customer last name
- phone: string - Customer phone number
- totalLoyaltyPoints: number - Accumulated loyalty points across all businesses
- stripeCustomerId: string - Stripe customer identifier for payments
- preferences: JSON - Booking preferences and history

**TypeScript Interface:**
```typescript
interface Customer {
  id: string;
  email: string;
  firstName: string;
  lastName: string;
  phone?: string;
  totalLoyaltyPoints: number;
  stripeCustomerId: string;
  preferences: CustomerPreferences;
  createdAt: Date;
  updatedAt: Date;
}

interface CustomerPreferences {
  communicationMethod: 'email' | 'sms' | 'both';
  reminderTiming: number; // hours before appointment
  favoriteServices: string[]; // service IDs
}
```

**Relationships:**
- Many-to-many with Business (through Bookings)
- One-to-many with Bookings
- One-to-many with LoyaltyTransactions

### Booking

**Purpose:** Individual appointment between customer and business

**Key Attributes:**
- id: string (UUID) - Unique booking identifier
- businessId: string - Business providing service
- customerId: string - Customer booking service
- serviceId: string - Specific service being booked
- calComBookingId: string - Cal.com booking reference
- startTime: DateTime - Appointment start time
- endTime: DateTime - Appointment end time
- status: enum - Scheduled, confirmed, completed, cancelled, no-show
- totalAmount: number - Total price in cents
- paymentIntentId: string - Stripe payment intent
- loyaltyPointsUsed: number - Points redeemed for discount
- loyaltyPointsEarned: number - Points earned from booking
- notes: text - Special instructions or notes

**TypeScript Interface:**
```typescript
interface Booking {
  id: string;
  businessId: string;
  customerId: string;
  serviceId: string;
  calComBookingId: string;
  startTime: Date;
  endTime: Date;
  status: BookingStatus;
  totalAmount: number; // cents
  paymentIntentId: string;
  loyaltyPointsUsed: number;
  loyaltyPointsEarned: number;
  notes?: string;
  createdAt: Date;
  updatedAt: Date;
}

type BookingStatus = 'scheduled' | 'confirmed' | 'completed' | 'cancelled' | 'no-show';
```

**Relationships:**
- Many-to-one with Business
- Many-to-one with Customer
- Many-to-one with Service
- One-to-many with LoyaltyTransactions

### LoyaltyTransaction

**Purpose:** Track loyalty point earnings and redemptions across the platform

**Key Attributes:**
- id: string (UUID) - Unique transaction identifier
- customerId: string - Customer account
- businessId: string - Business where transaction occurred
- bookingId: string - Related booking (if applicable)
- type: enum - Earned, redeemed, bonus, referral
- points: number - Point amount (positive for earned, negative for redeemed)
- description: string - Human-readable transaction description
- metadata: JSON - Additional transaction context

**TypeScript Interface:**
```typescript
interface LoyaltyTransaction {
  id: string;
  customerId: string;
  businessId: string;
  bookingId?: string;
  type: LoyaltyTransactionType;
  points: number;
  description: string;
  metadata: Record<string, any>;
  createdAt: Date;
}

type LoyaltyTransactionType = 'earned' | 'redeemed' | 'bonus' | 'referral' | 'expired';
```

**Relationships:**
- Many-to-one with Customer
- Many-to-one with Business
- Many-to-one with Booking (optional)

## API Specification

### tRPC Router Definitions

```typescript
import { z } from 'zod';
import { router, publicProcedure, protectedProcedure } from '../trpc';

// Main App Router
export const appRouter = router({
  // Business Management Routes
  business: businessRouter,
  // Service Management Routes
  service: serviceRouter,
  // Customer & Booking Routes
  booking: bookingRouter,
  // Loyalty System Routes
  loyalty: loyaltyRouter,
  // Template & Webstudio Integration Routes
  template: templateRouter,
  // External Integration Routes
  integrations: integrationsRouter,
});

// Business Router - Core business management
const businessRouter = router({
  // Create new business (onboarding)
  create: protectedProcedure
    .input(z.object({
      name: z.string().min(1).max(100),
      industry: z.enum(['wellness', 'consulting', 'trades', 'creative', 'hospitality']),
      templateId: z.string(),
      customization: z.record(z.any()),
    }))
    .mutation(async ({ input, ctx }) => {
      // Business creation logic with Stripe Connect account setup
    }),

  // Get business details for dashboard
  getBySlug: publicProcedure
    .input(z.object({ slug: z.string() }))
    .query(async ({ input }) => {
      // Return business details for public website
    }),

  // Update business settings
  update: protectedProcedure
    .input(z.object({
      businessId: z.string(),
      updates: z.object({
        name: z.string().optional(),
        customization: z.record(z.any()).optional(),
        loyaltySettings: z.record(z.any()).optional(),
      }),
    }))
    .mutation(async ({ input, ctx }) => {
      // Update business configuration
    }),

  // Get business analytics
  getAnalytics: protectedProcedure
    .input(z.object({
      businessId: z.string(),
      dateRange: z.object({
        start: z.date(),
        end: z.date(),
      }),
    }))
    .query(async ({ input, ctx }) => {
      // Return booking metrics, revenue, loyalty engagement
    }),
});

// Service Router - Service offerings management
const serviceRouter = router({
  // Create new service offering
  create: protectedProcedure
    .input(z.object({
      businessId: z.string(),
      name: z.string().min(1).max(100),
      description: z.string().max(500),
      duration: z.number().min(15).max(480), // 15 min to 8 hours
      price: z.number().min(0),
      loyaltyPointsEarned: z.number().min(0),
      category: z.string(),
    }))
    .mutation(async ({ input, ctx }) => {
      // Create service and Cal.com event type
    }),

  // List services for business
  listByBusiness: publicProcedure
    .input(z.object({ businessId: z.string() }))
    .query(async ({ input }) => {
      // Return active services for booking widget
    }),

  // Update service details
  update: protectedProcedure
    .input(z.object({
      serviceId: z.string(),
      updates: z.object({
        name: z.string().optional(),
        description: z.string().optional(),
        price: z.number().optional(),
        isActive: z.boolean().optional(),
      }),
    }))
    .mutation(async ({ input, ctx }) => {
      // Update service and sync with Cal.com
    }),
});

// Booking Router - Customer booking management
const bookingRouter = router({
  // Create new booking (customer-facing)
  create: publicProcedure
    .input(z.object({
      businessId: z.string(),
      serviceId: z.string(),
      customerId: z.string().optional(), // For returning customers
      customerInfo: z.object({
        email: z.string().email(),
        firstName: z.string(),
        lastName: z.string(),
        phone: z.string().optional(),
      }),
      startTime: z.date(),
      loyaltyPointsToUse: z.number().min(0).default(0),
    }))
    .mutation(async ({ input }) => {
      // Create booking, Cal.com appointment, Stripe payment intent
    }),

  // Get bookings for business dashboard
  listByBusiness: protectedProcedure
    .input(z.object({
      businessId: z.string(),
      status: z.enum(['all', 'upcoming', 'completed', 'cancelled']).optional(),
      page: z.number().min(1).default(1),
      limit: z.number().min(1).max(100).default(20),
    }))
    .query(async ({ input, ctx }) => {
      // Return paginated bookings with customer info
    }),

  // Update booking status
  updateStatus: protectedProcedure
    .input(z.object({
      bookingId: z.string(),
      status: z.enum(['confirmed', 'completed', 'cancelled', 'no-show']),
      notes: z.string().optional(),
    }))
    .mutation(async ({ input, ctx }) => {
      // Update booking and trigger loyalty point awards if completed
    }),

  // Get customer's booking history
  getCustomerHistory: publicProcedure
    .input(z.object({
      customerId: z.string(),
      businessId: z.string().optional(), // Filter by business
    }))
    .query(async ({ input }) => {
      // Return customer's past and upcoming bookings
    }),
});

// Loyalty Router - Loyalty points and rewards
const loyaltyRouter = router({
  // Get customer loyalty balance
  getBalance: publicProcedure
    .input(z.object({
      customerId: z.string(),
      businessId: z.string().optional(), // Business-specific or total
    }))
    .query(async ({ input }) => {
      // Return loyalty point balance and recent transactions
    }),

  // Redeem loyalty points
  redeemPoints: publicProcedure
    .input(z.object({
      customerId: z.string(),
      businessId: z.string(),
      points: z.number().min(1),
      bookingId: z.string(),
    }))
    .mutation(async ({ input }) => {
      // Create redemption transaction and apply discount
    }),

  // Get loyalty transaction history
  getTransactions: publicProcedure
    .input(z.object({
      customerId: z.string(),
      businessId: z.string().optional(),
      page: z.number().min(1).default(1),
    }))
    .query(async ({ input }) => {
      // Return paginated transaction history
    }),
});

// Template Router - Webstudio template management
const templateRouter = router({
  // List available templates
  listAvailable: publicProcedure
    .input(z.object({
      industry: z.enum(['wellness', 'consulting', 'trades', 'creative', 'hospitality']).optional(),
    }))
    .query(async ({ input }) => {
      // Return filtered template options with previews
    }),

  // Generate template with customization
  generate: protectedProcedure
    .input(z.object({
      templateId: z.string(),
      businessId: z.string(),
      customization: z.record(z.any()),
    }))
    .mutation(async ({ input, ctx }) => {
      // Generate Webstudio template with custom branding
    }),

  // Preview template changes
  preview: protectedProcedure
    .input(z.object({
      businessId: z.string(),
      customization: z.record(z.any()),
    }))
    .mutation(async ({ input, ctx }) => {
      // Generate preview URL for template changes
    }),
});

// Integrations Router - External service management
const integrationsRouter = router({
  // Cal.com integration setup
  setupCalCom: protectedProcedure
    .input(z.object({
      businessId: z.string(),
      calComApiKey: z.string(),
    }))
    .mutation(async ({ input, ctx }) => {
      // Validate and store Cal.com credentials
    }),

  // Stripe Connect onboarding
  createStripeAccount: protectedProcedure
    .input(z.object({
      businessId: z.string(),
      businessInfo: z.object({
        country: z.string(),
        businessType: z.string(),
        // Additional required Stripe fields
      }),
    }))
    .mutation(async ({ input, ctx }) => {
      // Create Stripe Connect account and return onboarding URL
    }),

  // Webhook handlers
  webhook: publicProcedure
    .input(z.object({
      source: z.enum(['cal-com', 'stripe']),
      payload: z.record(z.any()),
      signature: z.string(),
    }))
    .mutation(async ({ input }) => {
      // Process webhooks from external services
    }),
});

export type AppRouter = typeof appRouter;
```

### Authentication Requirements

All `protectedProcedure` routes require:
- Valid JWT token from NextAuth.js
- Business ownership verification for business-specific operations
- Rate limiting: 100 requests/minute per authenticated user

### Example Request/Response

**Create Booking Request:**
```typescript
// Frontend call
const booking = await trpc.booking.create.mutate({
  businessId: "bus_123",
  serviceId: "srv_456",
  customerInfo: {
    email: "customer@example.com",
    firstName: "John",
    lastName: "Doe"
  },
  startTime: new Date("2025-09-15T10:00:00Z"),
  loyaltyPointsToUse: 50
});
```

**Response:**
```typescript
{
  id: "book_789",
  status: "scheduled",
  calComBookingId: "cal_abc123",
  paymentIntentId: "pi_stripe456",
  loyaltyPointsEarned: 25,
  totalAmount: 9500, // $95.00 after loyalty discount
  confirmationUrl: "https://example.com/booking/book_789"
}
```

## Components

### Platform Core

**Responsibility:** Central application orchestration, authentication, and routing for both business dashboard and customer-facing sites

**Key Interfaces:**
- Next.js App Router for unified routing across business and customer experiences
- NextAuth.js session management for business owners
- tRPC client initialization and error handling
- Middleware for tenant isolation and custom domain routing

**Dependencies:** Database (user sessions), Redis (session storage), External auth providers

**Technology Stack:** Next.js 14+ App Router, NextAuth.js, tRPC client, Tailwind CSS for base styling

### Business Management Service

**Responsibility:** Business onboarding, configuration, analytics, and dashboard functionality for service business owners

**Key Interfaces:**
- Business creation and configuration API
- Template customization interface
- Analytics and reporting dashboard
- Integration status monitoring (Cal.com, Stripe, Webstudio)

**Dependencies:** Template Engine, External Integrations Service, Database

**Technology Stack:** React Server Components, Zustand for client state, Recharts for analytics, Radix UI components

### Template Engine

**Responsibility:** Dynamic website generation using Webstudio templates with custom branding and business-specific content

**Key Interfaces:**
- Template selection and preview API
- Real-time customization with live preview
- Template rendering and static generation
- Custom domain deployment coordination

**Dependencies:** Webstudio SDK, File Storage (Vercel Blob), CDN (Vercel Edge)

**Technology Stack:** Webstudio SDK wrapped in React components, Next.js static generation, Vercel deployment API

### Booking Engine

**Responsibility:** Customer-facing booking interface, appointment management, and booking workflow orchestration

**Key Interfaces:**
- Public booking widget for customer websites
- Availability checking and time slot management
- Booking confirmation and communication
- Booking modification and cancellation flows

**Dependencies:** Cal.com Integration, Payment Processing, Loyalty Service, Notification Service

**Technology Stack:** React components (embeddable), TanStack Query for state management, Cal.com SDK, responsive design patterns

### Payment Processing

**Responsibility:** Secure payment handling, Stripe Connect marketplace coordination, and financial transaction management

**Key Interfaces:**
- Payment intent creation and processing
- Stripe Connect account management for businesses
- Transaction recording and reconciliation
- Refund and dispute handling

**Dependencies:** Stripe Connect API, Database (transaction records), Loyalty Service (point redemptions)

**Technology Stack:** Stripe SDK, Stripe Connect marketplace patterns, PCI-compliant data handling, webhook processing

### Loyalty Service

**Responsibility:** Cross-business loyalty point management, rewards calculation, and customer retention features

**Key Interfaces:**
- Point earning calculation based on booking value
- Point redemption and discount application
- Loyalty balance queries and transaction history
- Future: Cross-business point exchange and tokenization

**Dependencies:** Database (loyalty transactions), Payment Processing (discount application), Notification Service

**Technology Stack:** PostgreSQL for transaction integrity, Redis for balance caching, future Web3 integration points

### External Integrations Service

**Responsibility:** Coordination and synchronization with Cal.com, Stripe, and Webstudio APIs plus webhook processing

**Key Interfaces:**
- Cal.com event type synchronization
- Stripe Connect account lifecycle management
- Webstudio template fetching and customization
- Unified webhook processing from all external services

**Dependencies:** External APIs (Cal.com, Stripe, Webstudio), Database (integration status), Redis (webhook deduplication)

**Technology Stack:** API client libraries, webhook signature verification, retry logic with exponential backoff, rate limiting

### Notification Service

**Responsibility:** Multi-channel customer and business communication including email, SMS, and in-app notifications

**Key Interfaces:**
- Booking confirmation and reminder sending
- Business notification delivery (new bookings, payments)
- Template-based message generation
- Delivery status tracking and retry logic

**Dependencies:** Email service (Resend), SMS provider (future), Database (notification preferences)

**Technology Stack:** Resend API, React Email for templates, Queue system for reliable delivery, notification preferences management

### Analytics & Monitoring

**Responsibility:** Platform performance monitoring, business intelligence, error tracking, and usage analytics

**Key Interfaces:**
- Business performance dashboards
- Platform health monitoring
- Error tracking and alerting
- Usage analytics for product optimization

**Dependencies:** Database (analytics queries), External monitoring (Vercel Analytics, Sentry)

**Technology Stack:** Vercel Analytics, Sentry error tracking, Custom analytics with PostgreSQL aggregations, Real-time dashboards

### Component Diagrams

```mermaid
graph TB
    subgraph "Frontend Layer"
        BusinessDash[🏢 Business Dashboard]
        CustomerSite[🌐 Customer Website]
        BookingWidget[📅 Booking Widget]
    end

    subgraph "Core Services"
        PlatformCore[⚡ Platform Core]
        BusinessMgmt[🏪 Business Management]
        TemplateEngine[🎨 Template Engine]
        BookingEngine[📋 Booking Engine]
    end

    subgraph "Supporting Services"
        PaymentProc[💳 Payment Processing]
        LoyaltyServ[🎁 Loyalty Service]
        Integrations[🔗 External Integrations]
        Notifications[📧 Notification Service]
        Analytics[📊 Analytics & Monitoring]
    end

    subgraph "External APIs"
        CalCom[📅 Cal.com]
        Stripe[💳 Stripe Connect]
        Webstudio[🎨 Webstudio]
        Resend[📧 Resend]
    end

    subgraph "Data Layer"
        PostgresDB[(🗄️ PostgreSQL)]
        RedisCache[(⚡ Redis)]
        VercelBlob[📁 Vercel Blob]
    end

    BusinessDash --> PlatformCore
    CustomerSite --> PlatformCore
    BookingWidget --> BookingEngine

    PlatformCore --> BusinessMgmt
    PlatformCore --> TemplateEngine
    PlatformCore --> BookingEngine

    BusinessMgmt --> PaymentProc
    BookingEngine --> PaymentProc
    BookingEngine --> LoyaltyServ
    BookingEngine --> Notifications

    TemplateEngine --> Integrations
    PaymentProc --> Integrations
    BookingEngine --> Integrations

    Integrations --> CalCom
    Integrations --> Stripe
    Integrations --> Webstudio
    Notifications --> Resend

    BusinessMgmt --> PostgresDB
    LoyaltyServ --> PostgresDB
    PaymentProc --> PostgresDB

    PlatformCore --> RedisCache
    LoyaltyServ --> RedisCache

    TemplateEngine --> VercelBlob

    Analytics --> PostgresDB
    Analytics --> RedisCache

    style PlatformCore fill:#e1f5fe
    style BookingEngine fill:#f3e5f5
    style PaymentProc fill:#fce4ec
    style TemplateEngine fill:#e8f5e8
```

## External APIs

### Cal.com API

- **Purpose:** Complete booking and scheduling infrastructure for service appointments
- **Documentation:** https://cal.com/docs/api-reference
- **Base URL(s):** https://api.cal.com/v1
- **Authentication:** API Key-based authentication per business account
- **Rate Limits:** 1000 requests/hour per API key, burst limit of 100/minute

**Key Endpoints Used:**
- `GET /event-types` - Retrieve business's service offerings
- `POST /event-types` - Create new service offerings from platform
- `GET /bookings` - Fetch booking list for business dashboard
- `POST /bookings` - Create customer appointments
- `PATCH /bookings/{id}` - Update appointment status
- `DELETE /bookings/{id}` - Cancel appointments
- `GET /availability` - Check provider availability for booking widget

**Integration Notes:** Each platform business requires separate Cal.com account with API key. Platform will manage Cal.com event type creation automatically when businesses add services. Webhook integration for real-time booking updates.

### Stripe Connect API

- **Purpose:** Multi-tenant payment processing enabling marketplace model with automatic fee collection
- **Documentation:** https://stripe.com/docs/connect
- **Base URL(s):** https://api.stripe.com/v1
- **Authentication:** Platform secret key + connected account authentication
- **Rate Limits:** 1000 requests/second (shared across platform), 25 requests/second per connected account

**Key Endpoints Used:**
- `POST /accounts` - Create Stripe Connect accounts for new businesses
- `POST /account_links` - Generate onboarding URLs for business verification
- `GET /accounts/{id}` - Check business account status and capabilities
- `POST /payment_intents` - Process customer payments with platform fees
- `POST /refunds` - Handle booking cancellation refunds
- `GET /balance_transactions` - Retrieve payment history for business analytics
- `POST /webhooks` - Receive payment status updates

**Integration Notes:** Platform uses Stripe Connect Express accounts for simplified business onboarding. Automatic fee collection (2.9% + 30¢ + platform fee) on all transactions. Webhook endpoints for payment confirmations and dispute notifications.

### Webstudio SDK

- **Purpose:** Dynamic template generation and customization for business websites
- **Documentation:** https://webstudio.is/docs/sdk (Note: May require direct contact for enterprise integration)
- **Base URL(s):** TBD - SDK-based integration, not REST API
- **Authentication:** SDK license key and project-based authentication
- **Rate Limits:** Template generation limits TBD based on licensing agreement

**Key Endpoints Used:**
- Template rendering functions (SDK methods)
- Asset management for custom branding
- Static site generation with custom domains
- Preview generation for template customization

**Integration Notes:** Most complex integration due to SDK nature rather than REST API. May require custom React wrapper components. Template customization limited by Webstudio's SDK capabilities. Need to validate real-time preview functionality and custom domain deployment.

### Resend API

- **Purpose:** Reliable transactional email delivery for booking confirmations, reminders, and business notifications
- **Documentation:** https://resend.com/docs
- **Base URL(s):** https://api.resend.com
- **Authentication:** API key-based authentication
- **Rate Limits:** 100 emails/minute on free tier, 3000/hour on paid plans

**Key Endpoints Used:**
- `POST /emails` - Send booking confirmations and reminders
- `GET /emails/{id}` - Track email delivery status
- `POST /domains` - Manage custom email domains for businesses
- `GET /webhooks` - Email event tracking (delivered, opened, clicked)

**Integration Notes:** Template-based emails using React Email components. Custom domain setup for professional business communications. Webhook integration for delivery tracking and bounce handling.

### Vercel Deployment API (Internal)

- **Purpose:** Automated custom domain deployment and template hosting for business websites
- **Documentation:** https://vercel.com/docs/rest-api
- **Base URL(s):** https://api.vercel.com
- **Authentication:** Vercel API token with deployment permissions
- **Rate Limits:** 1000 requests/hour for deployments

**Key Endpoints Used:**
- `POST /v13/deployments` - Deploy generated business websites
- `POST /v9/projects/{id}/domains` - Add custom domains for businesses
- `GET /v9/projects/{id}/deployments` - Monitor deployment status
- `DELETE /v9/projects/{id}/domains/{domain}` - Remove expired domains

**Integration Notes:** Automated deployment pipeline triggered when businesses update templates or branding. Custom domain verification and SSL certificate provisioning. Integration with Template Engine component for seamless website updates.

## Core Workflows

```mermaid
sequenceDiagram
    participant C as Customer
    participant BW as Business Website
    participant BE as Booking Engine
    participant Cal as Cal.com API
    participant PS as Payment Service
    participant SP as Stripe API
    participant LS as Loyalty Service
    participant NS as Notification Service

    Note over C,NS: Primary Booking Flow
    C->>BW: Browse services
    BW->>BE: Load booking widget
    BE->>Cal: Get availability
    Cal-->>BE: Available slots
    BE-->>C: Show booking form

    C->>BE: Submit booking request
    BE->>LS: Check loyalty balance
    LS-->>BE: Points available
    BE->>Cal: Create appointment
    Cal-->>BE: Booking confirmed

    BE->>PS: Create payment intent
    PS->>SP: Process payment with platform fee
    SP-->>PS: Payment successful
    PS-->>BE: Payment confirmed

    BE->>LS: Award loyalty points
    LS-->>BE: Points credited
    BE->>NS: Send confirmations
    NS-->>C: Email confirmation
    NS-->>BW: Business notification
```

## Database Schema

```sql
-- Business table
CREATE TABLE businesses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(50) UNIQUE NOT NULL,
    industry business_industry NOT NULL,
    owner_id UUID NOT NULL REFERENCES users(id),
    template_id VARCHAR(50) NOT NULL,
    customization JSONB DEFAULT '{}',
    status business_status DEFAULT 'draft',
    cal_com_integration JSONB DEFAULT '{}',
    stripe_account_id VARCHAR(100),
    loyalty_settings JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Services table
CREATE TABLE services (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    business_id UUID NOT NULL REFERENCES businesses(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    duration INTEGER NOT NULL, -- minutes
    price INTEGER NOT NULL, -- cents
    cal_com_event_type_id VARCHAR(100),
    loyalty_points_earned INTEGER DEFAULT 0,
    is_active BOOLEAN DEFAULT true,
    category VARCHAR(50),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Customers table
CREATE TABLE customers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    phone VARCHAR(20),
    total_loyalty_points INTEGER DEFAULT 0,
    stripe_customer_id VARCHAR(100),
    preferences JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Bookings table
CREATE TABLE bookings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    business_id UUID NOT NULL REFERENCES businesses(id),
    customer_id UUID NOT NULL REFERENCES customers(id),
    service_id UUID NOT NULL REFERENCES services(id),
    cal_com_booking_id VARCHAR(100),
    start_time TIMESTAMP WITH TIME ZONE NOT NULL,
    end_time TIMESTAMP WITH TIME ZONE NOT NULL,
    status booking_status DEFAULT 'scheduled',
    total_amount INTEGER NOT NULL, -- cents
    payment_intent_id VARCHAR(100),
    loyalty_points_used INTEGER DEFAULT 0,
    loyalty_points_earned INTEGER DEFAULT 0,
    notes TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Loyalty transactions table
CREATE TABLE loyalty_transactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    customer_id UUID NOT NULL REFERENCES customers(id),
    business_id UUID NOT NULL REFERENCES businesses(id),
    booking_id UUID REFERENCES bookings(id),
    type loyalty_transaction_type NOT NULL,
    points INTEGER NOT NULL,
    description VARCHAR(255) NOT NULL,
    metadata JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Indexes for performance
CREATE INDEX idx_businesses_slug ON businesses(slug);
CREATE INDEX idx_businesses_owner_id ON businesses(owner_id);
CREATE INDEX idx_services_business_id ON services(business_id);
CREATE INDEX idx_bookings_business_id ON bookings(business_id);
CREATE INDEX idx_bookings_customer_id ON bookings(customer_id);
CREATE INDEX idx_bookings_start_time ON bookings(start_time);
CREATE INDEX idx_loyalty_transactions_customer_id ON loyalty_transactions(customer_id);
CREATE INDEX idx_loyalty_transactions_business_id ON loyalty_transactions(business_id);

-- Enums
CREATE TYPE business_industry AS ENUM ('wellness', 'consulting', 'trades', 'creative', 'hospitality');
CREATE TYPE business_status AS ENUM ('draft', 'active', 'suspended', 'closed');
CREATE TYPE booking_status AS ENUM ('scheduled', 'confirmed', 'completed', 'cancelled', 'no-show');
CREATE TYPE loyalty_transaction_type AS ENUM ('earned', 'redeemed', 'bonus', 'referral', 'expired');
```

## Frontend Architecture

### Component Architecture

```
apps/web/src/
├── components/           # Reusable UI components
│   ├── ui/              # shadcn/ui base components
│   ├── booking/         # Booking widget components
│   ├── business/        # Business dashboard components
│   └── template/        # Template preview components
├── pages/               # App Router pages
│   ├── dashboard/       # Business owner dashboard
│   ├── [slug]/         # Dynamic business websites
│   └── api/            # API routes (tRPC)
├── hooks/              # Custom React hooks
├── stores/             # Zustand stores
└── lib/                # Utilities and configurations
```

### Component Template

```typescript
// Example: BookingWidget component
'use client';

import { useState } from 'react';
import { trpc } from '@/lib/trpc';
import { Button } from '@/components/ui/button';
import { Calendar } from '@/components/ui/calendar';

interface BookingWidgetProps {
  businessId: string;
  serviceId: string;
}

export function BookingWidget({ businessId, serviceId }: BookingWidgetProps) {
  const [selectedDate, setSelectedDate] = useState<Date | undefined>();

  const { data: availability } = trpc.booking.getAvailability.useQuery({
    businessId,
    serviceId,
    date: selectedDate
  });

  const createBooking = trpc.booking.create.useMutation();

  return (
    <div className="booking-widget">
      <Calendar
        mode="single"
        selected={selectedDate}
        onSelect={setSelectedDate}
      />
      {/* Booking form implementation */}
    </div>
  );
}
```

### State Management Architecture

```typescript
// Zustand store structure
interface AppState {
  user: User | null;
  currentBusiness: Business | null;
  bookingState: {
    selectedService: Service | null;
    selectedDate: Date | null;
    customerInfo: CustomerInfo | null;
  };
}

// TanStack Query for server state
const businessQueries = {
  getBySlug: (slug: string) =>
    useQuery(['business', slug], () => trpc.business.getBySlug.query({ slug })),
  getAnalytics: (businessId: string, dateRange: DateRange) =>
    useQuery(['analytics', businessId, dateRange], () =>
      trpc.business.getAnalytics.query({ businessId, dateRange }))
};
```

### Routing Architecture

```
/                       # Landing page
/dashboard             # Business owner dashboard
/dashboard/business    # Business settings
/dashboard/services    # Service management
/dashboard/bookings    # Booking management
/dashboard/analytics   # Business analytics
/[slug]                # Business website (dynamic)
/[slug]/book          # Booking page
/[slug]/book/[serviceId] # Service-specific booking
/api/trpc/[...trpc]   # tRPC API routes
/api/webhooks/[source] # Webhook handlers
```

### Protected Route Pattern

```typescript
// Example: Protected business dashboard route
'use client';

import { useSession } from 'next-auth/react';
import { useRouter } from 'next/navigation';
import { useEffect } from 'react';

interface ProtectedRouteProps {
  children: React.ReactNode;
  requiresBusiness?: boolean;
}

export function ProtectedRoute({ children, requiresBusiness = false }: ProtectedRouteProps) {
  const { data: session, status } = useSession();
  const router = useRouter();

  useEffect(() => {
    if (status === 'loading') return;

    if (!session) {
      router.push('/auth/signin');
      return;
    }

    if (requiresBusiness && !session.user.businessId) {
      router.push('/onboarding');
      return;
    }
  }, [session, status, router, requiresBusiness]);

  if (status === 'loading') return <div>Loading...</div>;
  if (!session) return null;
  if (requiresBusiness && !session.user.businessId) return null;

  return <>{children}</>;
}
```

### Frontend Services Layer

```typescript
// API client setup
import { createTRPCReact } from '@trpc/react-query';
import { type AppRouter } from '@/server/api/root';

export const trpc = createTRPCReact<AppRouter>();

// Service example
export class BookingService {
  static async createBooking(data: CreateBookingInput) {
    return trpc.booking.create.mutate(data);
  }

  static async getAvailability(businessId: string, serviceId: string, date: Date) {
    return trpc.booking.getAvailability.query({ businessId, serviceId, date });
  }

  static async getBookingHistory(customerId: string) {
    return trpc.booking.getCustomerHistory.query({ customerId });
  }
}
```

## Backend Architecture

### Service Architecture (Serverless)

```
apps/api/src/
├── routers/            # tRPC router definitions
│   ├── business.ts     # Business management
│   ├── booking.ts      # Booking operations
│   ├── loyalty.ts      # Loyalty system
│   └── integrations.ts # External API coordination
├── services/           # Business logic services
│   ├── booking-service.ts
│   ├── payment-service.ts
│   └── template-service.ts
├── lib/                # Utilities and configurations
│   ├── prisma.ts       # Database client
│   ├── redis.ts        # Cache client
│   └── integrations/   # External API clients
└── middleware/         # Authentication and validation
```

### Function Template

```typescript
// Example: Booking creation function
export const createBooking = async (input: CreateBookingInput) => {
  // 1. Validate customer and service
  const service = await prisma.service.findUnique({
    where: { id: input.serviceId },
    include: { business: true }
  });

  // 2. Create Cal.com appointment
  const calBooking = await calComAPI.createBooking({
    eventTypeId: service.calComEventTypeId,
    start: input.startTime,
    attendee: input.customerInfo
  });

  // 3. Create payment intent
  const paymentIntent = await stripe.paymentIntents.create({
    amount: calculateTotal(service.price, input.loyaltyPointsUsed),
    application_fee_amount: calculatePlatformFee(service.price),
    transfer_data: {
      destination: service.business.stripeAccountId
    }
  });

  // 4. Save booking and award points
  const booking = await prisma.booking.create({
    data: {
      businessId: input.businessId,
      serviceId: input.serviceId,
      customerId: customer.id,
      calComBookingId: calBooking.id,
      startTime: input.startTime,
      endTime: addMinutes(input.startTime, service.duration),
      totalAmount: paymentIntent.amount,
      paymentIntentId: paymentIntent.id,
      loyaltyPointsUsed: input.loyaltyPointsUsed,
      loyaltyPointsEarned: service.loyaltyPointsEarned
    }
  });

  // 5. Send notifications
  await notificationService.sendBookingConfirmation(booking);

  return booking;
};
```

### Database Architecture

```sql
-- Schema design (core tables)
CREATE TABLE businesses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(50) UNIQUE NOT NULL,
    industry business_industry NOT NULL,
    owner_id UUID NOT NULL REFERENCES users(id),
    template_id VARCHAR(50) NOT NULL,
    customization JSONB DEFAULT '{}',
    status business_status DEFAULT 'draft',
    cal_com_integration JSONB DEFAULT '{}',
    stripe_account_id VARCHAR(100),
    loyalty_settings JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Data Access Layer

```typescript
// Repository pattern implementation
export class BusinessRepository {
  async create(data: CreateBusinessData): Promise<Business> {
    return prisma.business.create({
      data: {
        ...data,
        slug: slugify(data.name),
        status: 'draft'
      }
    });
  }

  async findBySlug(slug: string): Promise<Business | null> {
    return prisma.business.findUnique({
      where: { slug },
      include: {
        services: true,
        owner: true
      }
    });
  }

  async updateCustomization(id: string, customization: any): Promise<Business> {
    return prisma.business.update({
      where: { id },
      data: { customization }
    });
  }
}
```

### Authentication and Authorization

```mermaid
sequenceDiagram
    participant U as User
    participant FE as Frontend
    participant Auth as NextAuth.js
    participant API as tRPC API
    participant DB as Database

    U->>FE: Login request
    FE->>Auth: OAuth provider login
    Auth->>Auth: Validate with provider
    Auth-->>FE: Session token

    FE->>API: tRPC call with session
    API->>Auth: Validate session
    Auth-->>API: User info
    API->>DB: Check business ownership
    DB-->>API: Permission granted
    API-->>FE: Response data
```

### Middleware/Guards

```typescript
// Authentication middleware for tRPC
export const protectedProcedure = publicProcedure.use(
  async ({ ctx, next }) => {
    if (!ctx.session || !ctx.session.user) {
      throw new TRPCError({ code: 'UNAUTHORIZED' });
    }

    return next({
      ctx: {
        ...ctx,
        session: { ...ctx.session, user: ctx.session.user },
      },
    });
  }
);

// Business ownership middleware
export const businessOwnerProcedure = protectedProcedure.use(
  async ({ ctx, input, next }) => {
    if (!input || typeof input !== 'object' || !('businessId' in input)) {
      throw new TRPCError({ code: 'BAD_REQUEST', message: 'Business ID required' });
    }

    const business = await prisma.business.findFirst({
      where: {
        id: input.businessId as string,
        ownerId: ctx.session.user.id
      }
    });

    if (!business) {
      throw new TRPCError({ code: 'FORBIDDEN', message: 'Not authorized for this business' });
    }

    return next({ ctx });
  }
);
```

## Unified Project Structure

```
universal-service-platform/
├── .github/                    # CI/CD workflows
│   └── workflows/
│       ├── ci.yaml
│       └── deploy.yaml
├── apps/                       # Application packages
│   ├── web/                    # Next.js frontend application
│   │   ├── src/
│   │   │   ├── components/     # UI components
│   │   │   ├── app/           # App Router pages
│   │   │   ├── hooks/         # Custom React hooks
│   │   │   ├── stores/        # Zustand stores
│   │   │   ├── styles/        # Global styles/themes
│   │   │   └── lib/           # Frontend utilities
│   │   ├── public/            # Static assets
│   │   ├── tests/             # Frontend tests
│   │   └── package.json
│   └── api/                   # Backend serverless functions
│       ├── src/
│       │   ├── routers/       # tRPC routers
│       │   ├── services/      # Business logic
│       │   ├── lib/           # Backend utilities
│       │   └── middleware/    # API middleware
│       ├── tests/             # Backend tests
│       └── package.json
├── packages/                  # Shared packages
│   ├── shared/                # Shared types/utilities
│   │   ├── src/
│   │   │   ├── types/         # TypeScript interfaces
│   │   │   ├── constants/     # Shared constants
│   │   │   └── utils/         # Shared utilities
│   │   └── package.json
│   ├── ui/                    # Shared UI components
│   │   ├── src/
│   │   │   ├── components/    # Reusable components
│   │   │   └── styles/        # Component styles
│   │   └── package.json
│   ├── integrations/          # External API clients
│   │   ├── src/
│   │   │   ├── cal-com/       # Cal.com SDK wrapper
│   │   │   ├── stripe/        # Stripe Connect utilities
│   │   │   └── webstudio/     # Webstudio integration
│   │   └── package.json
│   └── config/                # Shared configuration
│       ├── eslint/
│       ├── typescript/
│       └── tailwind/
├── infrastructure/            # Terraform configurations
│   ├── environments/
│   │   ├── development.tf
│   │   ├── staging.tf
│   │   └── production.tf
│   └── modules/
│       ├── database/
│       └── cache/
├── scripts/                   # Build/deploy scripts
│   ├── setup.sh
│   ├── migrate.sh
│   └── deploy.sh
├── docs/                      # Documentation
│   ├── prd.md
│   ├── fullstack-architecture.md
│   └── api-documentation.md
├── .env.example               # Environment template
├── package.json               # Root package.json
├── turbo.json                 # Turborepo configuration
├── tsconfig.json              # TypeScript configuration
└── README.md
```

## Development Workflow

### Prerequisites

```bash
# Install Node.js 18+, pnpm, and Docker
curl -fsSL https://get.pnpm.io/install.sh | sh
docker --version  # Verify Docker installation
```

### Initial Setup

```bash
# Clone and install dependencies
git clone <repository-url>
cd universal-service-platform
pnpm install

# Setup environment variables
cp .env.example .env
# Edit .env with local development values

# Setup database
docker-compose up -d postgres redis
pnpm db:migrate
pnpm db:seed
```

### Development Commands

```bash
# Start all services
pnpm dev

# Start frontend only
pnpm dev:web

# Start backend only
pnpm dev:api

# Run tests
pnpm test              # All tests
pnpm test:unit         # Unit tests only
pnpm test:e2e          # End-to-end tests
pnpm test:coverage     # With coverage report

# Build and deploy
pnpm build
pnpm deploy:staging
pnpm deploy:prod
```

### Environment Configuration

```bash
# Frontend (.env.local)
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
NEXTAUTH_SECRET=your-secret-here
NEXTAUTH_URL=http://localhost:3000

# Backend (.env)
DATABASE_URL=postgresql://user:pass@localhost:5432/platform
REDIS_URL=redis://localhost:6379
STRIPE_SECRET_KEY=sk_test_...
CAL_COM_CLIENT_ID=your-cal-com-id
CAL_COM_CLIENT_SECRET=your-cal-com-secret
WEBSTUDIO_API_KEY=your-webstudio-key
RESEND_API_KEY=your-resend-key

# Shared
NODE_ENV=development
LOG_LEVEL=debug
```

## Deployment Architecture

### Deployment Strategy

**Frontend Deployment:**
- **Platform:** Vercel (auto-deploy from main branch)
- **Build Command:** `pnpm build`
- **Output Directory:** `apps/web/.next`
- **CDN/Edge:** Vercel Global CDN with edge functions

**Backend Deployment:**
- **Platform:** Vercel Serverless Functions
- **Build Command:** `pnpm build:api`
- **Deployment Method:** Auto-deploy via tRPC integration

### CI/CD Pipeline

```yaml
name: Deploy Platform
on:
  push:
    branches: [main, staging]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v2
      - run: pnpm install
      - run: pnpm test:unit
      - run: pnpm build

  deploy-staging:
    if: github.ref == 'refs/heads/staging'
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: vercel/action@v1
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}

  deploy-production:
    if: github.ref == 'refs/heads/main'
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: vercel/action@v1
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-args: '--prod'
```

### Environments

| Environment | Frontend URL | Backend URL | Purpose |
|------------|-------------|-------------|---------|
| Development | http://localhost:3000 | http://localhost:3000/api | Local development |
| Staging | https://staging.platform.app | https://staging.platform.app/api | Pre-production testing |
| Production | https://platform.app | https://platform.app/api | Live environment |

## Security and Performance

### Security Requirements

**Frontend Security:**
- CSP Headers: `default-src 'self'; script-src 'self' 'unsafe-inline' https://js.stripe.com; style-src 'self' 'unsafe-inline'`
- XSS Prevention: React's built-in escaping + Content Security Policy
- Secure Storage: HTTPOnly cookies for sessions, localStorage for non-sensitive UI state

**Backend Security:**
- Input Validation: Zod schema validation on all tRPC endpoints
- Rate Limiting: 100 req/min per IP, 1000 req/min per authenticated user
- CORS Policy: Restrict to known frontend domains, dynamic for business websites

**Authentication Security:**
- Token Storage: HTTPOnly cookies with secure, SameSite strict
- Session Management: NextAuth.js with database sessions, 24-hour expiry
- Password Policy: Delegated to OAuth providers (Google, GitHub)

### Performance Optimization

**Frontend Performance:**
- Bundle Size Target: <250KB initial bundle, <50KB per route
- Loading Strategy: React Suspense with skeleton loaders, route-based code splitting
- Caching Strategy: SWR with 5-minute stale-while-revalidate, CDN caching for static assets

**Backend Performance:**
- Response Time Target: <200ms for booking queries, <500ms for complex analytics
- Database Optimization: Indexed queries, connection pooling, read replicas for analytics
- Caching Strategy: Redis for session data and frequently accessed business info, 1-hour TTL

## Testing Strategy

### Testing Pyramid

```
    E2E Tests (Playwright)
   /                    \
Integration Tests (Vitest + MSW)
/                              \
Frontend Unit Tests    Backend Unit Tests
  (Vitest + RTL)         (Vitest + Prisma)
```

### Test Organization

**Frontend Tests:**
```
apps/web/tests/
├── __mocks__/          # Mock implementations
├── components/         # Component unit tests
├── hooks/             # Custom hook tests
├── pages/             # Page integration tests
└── e2e/               # End-to-end scenarios
```

**Backend Tests:**
```
apps/api/tests/
├── routers/           # tRPC router tests
├── services/          # Business logic tests
├── integrations/      # External API mocks
└── __fixtures__/      # Test data fixtures
```

**E2E Tests:**
```
tests/e2e/
├── booking-flow/      # End-to-end booking scenarios
├── business-onboarding/ # Business setup flows
└── payment-processing/ # Payment integration tests
```

### Test Examples

**Frontend Component Test:**
```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { BookingWidget } from '@/components/booking/BookingWidget';
import { trpc } from '@/lib/trpc';

// Mock tRPC
jest.mock('@/lib/trpc', () => ({
  booking: {
    getAvailability: {
      useQuery: jest.fn(() => ({
        data: mockAvailability,
        isLoading: false
      }))
    }
  }
}));

test('BookingWidget displays available time slots', () => {
  render(<BookingWidget businessId="123" serviceId="456" />);

  expect(screen.getByText('Available Times')).toBeInTheDocument();
  expect(screen.getByText('10:00 AM')).toBeInTheDocument();
});
```

**Backend API Test:**
```typescript
import { createCallerFactory } from '@/lib/trpc';
import { appRouter } from '@/routers/_app';
import { db } from '@/lib/db';

const createCaller = createCallerFactory(appRouter);

test('booking.create creates appointment and payment intent', async () => {
  const caller = createCaller({ user: mockUser, db });

  const result = await caller.booking.create({
    businessId: 'biz_123',
    serviceId: 'srv_456',
    customerInfo: mockCustomer,
    startTime: new Date('2025-09-15T10:00:00Z')
  });

  expect(result.id).toBeDefined();
  expect(result.paymentIntentId).toMatch(/^pi_/);
});
```

**E2E Test:**
```typescript
import { test, expect } from '@playwright/test';

test('complete booking flow', async ({ page }) => {
  // Navigate to business website
  await page.goto('/wellness-spa');

  // Select service
  await page.click('[data-testid="service-massage-60min"]');

  // Choose time slot
  await page.click('[data-testid="time-slot-10am"]');

  // Fill customer info
  await page.fill('[data-testid="customer-email"]', 'test@example.com');
  await page.fill('[data-testid="customer-name"]', 'John Doe');

  // Complete payment
  await page.click('[data-testid="book-now-button"]');

  // Verify confirmation
  await expect(page.locator('[data-testid="booking-confirmation"]')).toBeVisible();
});
```

## Coding Standards

### Critical Fullstack Rules

- **Type Sharing:** Always define shared types in `packages/shared` and import across frontend/backend
- **API Calls:** Never make direct HTTP calls - use tRPC procedures exclusively
- **Environment Variables:** Access only through validated config objects, never `process.env` directly in components
- **Error Handling:** All tRPC procedures must use standard error format with proper error codes
- **State Updates:** Never mutate Zustand state directly - use proper actions and immutable updates
- **Database Access:** Always use Prisma client, never raw SQL except for complex analytics queries
- **External API Calls:** All external integrations must go through service layer with proper error handling
- **Authentication:** Check user permissions at procedure level, never assume frontend validation is sufficient

### Naming Conventions

| Element | Frontend | Backend | Example |
|---------|----------|---------|---------|
| Components | PascalCase | - | `BookingWidget.tsx` |
| Hooks | camelCase with 'use' | - | `useBookingForm.ts` |
| tRPC Procedures | - | camelCase | `booking.create` |
| Database Tables | - | snake_case | `loyalty_transactions` |
| API Routes | - | kebab-case | `/api/webhooks/cal-com` |
| Environment Variables | UPPER_SNAKE_CASE | UPPER_SNAKE_CASE | `STRIPE_SECRET_KEY` |

## Error Handling Strategy

### Error Flow

```mermaid
sequenceDiagram
    participant FE as Frontend
    participant API as tRPC API
    participant EXT as External API
    participant ERR as Error Handler

    FE->>API: tRPC procedure call
    API->>EXT: External API call
    EXT-->>API: API Error (4xx/5xx)
    API->>ERR: Process error
    ERR->>ERR: Log error + generate ID
    ERR-->>API: Formatted error response
    API-->>FE: tRPC Error with user message
    FE->>FE: Display user-friendly error
```

### Error Response Format

```typescript
interface ApiError {
  error: {
    code: string;
    message: string;
    details?: Record<string, any>;
    timestamp: string;
    requestId: string;
  };
}
```

### Frontend Error Handling

```typescript
// Global error boundary
export function GlobalErrorBoundary({ children }: { children: React.ReactNode }) {
  const handleError = (error: Error) => {
    // Send to monitoring service
    Sentry.captureException(error);

    // Show user-friendly message
    toast.error('Something went wrong. Please try again.');
  };

  return (
    <ErrorBoundary onError={handleError}>
      {children}
    </ErrorBoundary>
  );
}

// tRPC error handling
const bookingMutation = trpc.booking.create.useMutation({
  onError: (error) => {
    if (error.data?.code === 'PAYMENT_FAILED') {
      toast.error('Payment could not be processed. Please check your card details.');
    } else if (error.data?.code === 'BOOKING_CONFLICT') {
      toast.error('This time slot is no longer available. Please select another time.');
    } else {
      toast.error('Booking failed. Please try again or contact support.');
    }
  }
});
```

### Backend Error Handling

```typescript
// Custom error classes
export class BookingConflictError extends Error {
  code = 'BOOKING_CONFLICT';
  statusCode = 409;
}

export class PaymentFailedError extends Error {
  code = 'PAYMENT_FAILED';
  statusCode = 402;
}

// Global error handler
export const errorHandler = (error: unknown): TRPCError => {
  const requestId = generateRequestId();

  // Log error with context
  logger.error('API Error', {
    error: error instanceof Error ? error.message : 'Unknown error',
    requestId,
    stack: error instanceof Error ? error.stack : undefined
  });

  // Return appropriate user error
  if (error instanceof BookingConflictError) {
    return new TRPCError({
      code: 'CONFLICT',
      message: 'The selected time slot is no longer available',
      cause: error
    });
  }

  // Default fallback
  return new TRPCError({
    code: 'INTERNAL_SERVER_ERROR',
    message: 'An unexpected error occurred',
    cause: error
  });
};
```

## Monitoring and Observability

### Monitoring Stack

- **Frontend Monitoring:** Vercel Web Analytics + Core Web Vitals tracking
- **Backend Monitoring:** Vercel Functions metrics + custom performance tracking
- **Error Tracking:** Sentry for comprehensive error monitoring and alerting
- **Performance Monitoring:** Vercel Speed Insights + custom database query tracking

### Key Metrics

**Frontend Metrics:**
- Core Web Vitals (LCP, FID, CLS)
- JavaScript errors and unhandled promise rejections
- API response times from client perspective
- User interactions (booking funnel conversion rates)

**Backend Metrics:**
- Request rate and error rate by tRPC procedure
- Response time percentiles (p50, p95, p99)
- Database query performance and connection pool utilization
- External API call success rates and latency

**Business Metrics:**
- New business onboarding completion rate
- Booking creation success rate
- Payment processing success rate
- Customer loyalty engagement metrics

## Checklist Results Report

The Universal Service Business Platform Full-Stack Architecture is now complete! This comprehensive document provides:

✅ **Complete Technology Stack**: Defined with specific versions and rationale
✅ **Scalable Architecture**: Serverless-first design supporting 500+ businesses
✅ **External Integration Strategy**: Clear implementation plan for Cal.com, Stripe, Webstudio
✅ **Type-Safe Development**: End-to-end TypeScript with tRPC for API safety
✅ **Production-Ready Infrastructure**: Vercel + Railway deployment with monitoring
✅ **Comprehensive Testing Strategy**: Unit, integration, and E2E testing approach
✅ **Security & Performance**: Production-grade security measures and optimization
✅ **Developer Experience**: Modern tooling with excellent DX via Turborepo and Next.js

**Key Architectural Decisions:**
- T3 Stack foundation with tRPC for type safety
- Vercel + Railway for cost-effective, scalable deployment
- Component-based design enabling independent scaling
- Comprehensive external API integration strategy

**Ready for Implementation**: This architecture supports the PRD's goals of 30-minute business setup, 500 active businesses target, and $100K ARR within 12 months.

The architecture is now ready for development team handoff and implementation!