# Zeradon

> A job marketplace connecting customers with local workers — built with Next.js, Prisma, and Stripe.

---

## 📋 Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Tech Stack](#tech-stack)
- [Database Schema](#database-schema)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)

---

## Overview

Zeradon is a two-sided marketplace where **customers** post jobs and **workers** find opportunities in their area. The platform is free for both sides — workers pay a small **$2.99/month** subscription only if they want to message customers.

---

## How It Works

### 👤 Customer Flow (Free)
1. Sign up and create an account
2. Post a job with a title, description, price, and city/state location
3. Receive messages from interested workers
4. Choose the best worker for the job
5. Mark the job as complete and rate the worker

### 🔧 Worker Flow (Free + Optional Subscription)
1. Sign up and select **Worker Account**
2. Browse available jobs filtered by price and location
3. Subscribe for **$2.99/month** to unlock messaging
4. Message customers and accept jobs
5. Build your ratings over time

### 💳 Paywall Logic

Only workers with an **active subscription** can send messages to customers.

```
if user.role === "worker" && subscription.status !== "active"
  → block messaging
```

Customers can always message back for free.

---

## Tech Stack

| Layer          | Technology                          |
|----------------|-------------------------------------|
| **Framework**  | [Next.js](https://nextjs.org/)      |
| **ORM**        | [Prisma](https://www.prisma.io/)    |
| **Auth**       | [Auth.js](https://authjs.dev/)      |
| **Billing**    | [Stripe](https://stripe.com/)       |
| **Hosting**    | [Vercel](https://vercel.com/)       |
| **Mobile**     | [Flutter](https://flutter.dev/)     |

---

## Database Schema

### Users
| Field       | Type                     |
|-------------|--------------------------|
| id          | String (PK)              |
| name        | String                   |
| email       | String (unique)          |
| password    | String                   |
| role        | Enum: `customer, worker` |
| createdAt   | DateTime                 |

### Jobs
| Field       | Type                                        |
|-------------|---------------------------------------------|
| id          | String (PK)                                 |
| title       | String                                      |
| description | String                                      |
| category    | String                                      |
| price       | Float                                       |
| city        | String                                      |
| state       | String                                      |
| status      | Enum: `open, in_progress, completed`        |
| userId      | String (FK → Users)                         |
| createdAt   | DateTime                                    |

### Messages
| Field      | Type                |
|------------|---------------------|
| id         | String (PK)         |
| content    | String              |
| senderId   | String (FK → Users) |
| receiverId | String (FK → Users) |
| jobId      | String (FK → Jobs)  |
| createdAt  | DateTime            |

### Subscriptions
| Field                | Type                              |
|----------------------|-----------------------------------|
| id                   | String (PK)                       |
| userId               | String (FK → Users)               |
| stripeCustomerId     | String                            |
| stripeSubscriptionId | String                            |
| status               | Enum: `active, canceled, past_due`|
| currentPeriodEnd     | DateTime                          |
| createdAt            | DateTime                          |

---

## Project Structure

```
zeradon/
├── backend/          # API routes, Prisma schema, server logic
├── frontend/         # Next.js web app
├── mobile/           # Flutter mobile app
├── .gitignore
└── README.md
```

---

## Getting Started

### Prerequisites
- Node.js 18+
- Flutter SDK
- PostgreSQL database
- Stripe account

### Installation

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/zeradon.git
cd zeradon

# Install frontend/backend dependencies
cd frontend
npm install

# Set up the database
npx prisma generate
npx prisma migrate dev

# Run the development server
npm run dev
```

```bash
# Run the Flutter mobile app
cd mobile
flutter pub get
flutter run
```

---

## Environment Variables

Create a `.env` file in the `frontend/` directory:

```env
# Database
DATABASE_URL="postgresql://..."

# Auth.js
NEXTAUTH_SECRET="your-secret"
NEXTAUTH_URL="http://localhost:3000"

# Stripe
STRIPE_SECRET_KEY="sk_..."
STRIPE_WEBHOOK_SECRET="whsec_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_..."
```

---

## License

This project is private and proprietary. All rights reserved © Zeradon.