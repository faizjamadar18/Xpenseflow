<div align="center">
  <!-- <img src="public/logo.png" alt="Welth Logo" width="180"/> -->
  <h1 align="center">Xpenseflow — For Smarter Money</h1>
  <p align="center"><strong></strong></p>
  <p align="center">AI-Powered Personal Finance — Receipt Scanning + Budget Alerts + Monthly Insights</p>



  <br/>

  <p align="center">
    <img src="https://img.shields.io/badge/Next.js-16-000000?style=flat-square&logo=nextdotjs&logoColor=white" alt="Next.js"/>
    <img src="https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=black" alt="React"/>
    <img src="https://img.shields.io/badge/Prisma-6.0-2D3748?style=flat-square&logo=prisma&logoColor=white" alt="Prisma"/>
    <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL"/>
    <img src="https://img.shields.io/badge/Clerk-Auth-6C47FF?style=flat-square&logo=clerk&logoColor=white" alt="Clerk"/>
    <img src="https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white" alt="Tailwind CSS"/>
    <img src="https://img.shields.io/badge/Gemini_AI-4285F4?style=flat-square&logo=googlegemini&logoColor=white" alt="Gemini AI"/>
    <img src="https://img.shields.io/badge/Inngest-Jobs-1F1F1F?style=flat-square&logo=inngest&logoColor=white" alt="Inngest"/>
    <img src="https://img.shields.io/badge/Resend-Email-000000?style=flat-square&logo=resend&logoColor=white" alt="Resend"/>
    <img src="https://img.shields.io/badge/license-MIT-999999?style=flat-square" alt="License"/>
  </p>
</div>

---

## 📺 Demo Video

<div align="center">

[![Watch the Welth Demo](https://img.youtube.com/vi/JfZncqAAA4E/maxresdefault.jpg)](https://www.youtube.com/watch?v=JfZncqAAA4E)

🌐 **Live App:** [xpenseflow.vercel.app](https://xpenseflow.vercel.app)

</div>

---

## Overview

**Welth** is an AI-powered personal finance platform that turns messy money tracking into something you'll actually do. Snap a picture of a receipt and Gemini extracts the amount, date, and merchant automatically. Log income and expenses, watch your balances update in real time, and let the app warn you *before* you blow your monthly budget.

Behind the scenes, scheduled **Inngest** jobs check your spending every 6 hours, email a budget alert once you cross 80%, and deliver a monthly financial report with **AI-generated insights** — all rendered with **React Email** and delivered through **Resend**.

Built on **Next.js 16**, **Prisma/PostgreSQL**, and **Clerk**, Welth is a production-ready, full-stack application with a dark, animated marketing site and a protected dashboard.

---

## Key Features

### 💸 Transactions

| Feature | Description |
|---------|-------------|
| **AI Receipt Scan** | Upload a receipt photo — Gemini 2.5 Flash reads it and auto-fills amount, date, description, and merchant |
| **Income & Expense** | Log transactions against any account with amount, date, and description |
| **Transaction Timeline** | Browse the complete history for an account, newest first |
| **Bulk Delete** | Select and remove many transactions at once — account balances are recalculated atomically |

### 🏦 Accounts & Budget

| Feature | Description |
|---------|-------------|
| **Multiple Accounts** | Track both `CURRENT` and `SAVINGS` accounts in one place |
| **Default Account** | Mark any account as default — it drives the monthly budget calculation |
| **Auto Balance Updates** | Creating or deleting a transaction updates the account balance inside a Prisma `$transaction` |
| **Monthly Budget** | Set a monthly budget and watch a progress bar track how much you've used |

### 🔔 AI Alerts & Reports

| Feature | Description |
|---------|-------------|
| **Budget Alerts** | Email alert (via Resend) as soon as monthly spend passes **80%** of your budget |
| **Monthly Reports** | First-of-the-month email summarizing total income, expenses, and net |
| **AI Insights** | Gemini generates 3 friendly, actionable spending insights for the report |

### 🎨 Landing & Auth

| Feature | Description |
|---------|-------------|
| **Animated Landing Page** | Motion-powered hero, "Difference", workflow, capabilities, and CTA sections |
| **Clerk Authentication** | Sign-up / sign-in with `dashboard`, `account`, and `transaction` routes protected by middleware |
| **Dark Theme** | Polished dark UI built on Tailwind CSS v4 + shadcn/ui |

---

## How It Works

### AI Receipt Scanning Pipeline
```
Receipt Image → Base64 (Buffer) → Gemini 2.5 Flash (vision) → Structured JSON → Transaction Form → Account Balance Update
```
- **Input**: the uploaded file is read into an `ArrayBuffer` and encoded to a Base64 string
- **Model**: `gemini-2.5-flash` receives the image as `inlineData` plus an extraction prompt
- **Output**: valid JSON `{ amount, date, description, merchantName }`, cleaned of markdown fences and parsed
- **Commit**: the transaction is created and `account.balance` is incremented inside a single `db.$transaction`

### Budget Alert & Monthly Report Pipeline
```
Inngest Cron → Aggregate Monthly Expenses → Threshold Check (≥ 80%) → Gemini Insights → React Email → Resend → Inbox
```
- **Budget Alerts**: cron `0 */6 * * *` runs every 6 hours, aggregates the default account's expenses for the current month, and emails when usage hits **80%** (once per month via `lastAlertSent`)
- **Monthly Reports**: cron `0 0 1 * *` runs on the first of each month and summarizes the previous month's income, expenses, and net
- **AI Insights**: Gemini turns the raw numbers into 3 concise, conversational recommendations
- **Delivery**: `@react-email/components` templates rendered server-side and sent with Resend

---

## Tech Stack

<p align="center">
  <img src="https://skillicons.dev/icons?i=nextjs,react,tailwind,postgres,prisma,js,git,vercel" />
</p>

| Layer | Technology |
|-------|-----------|
| **Framework** | [Next.js 16](https://nextjs.org) (App Router, Server Actions) + React 19 |
| **Styling** | [Tailwind CSS v4](https://tailwindcss.com) + [shadcn/ui](https://ui.shadcn.com) (Radix UI) |
| **Auth** | [Clerk](https://clerk.com) (middleware-protected routes & synced users) |
| **Database** | [PostgreSQL](https://www.postgresql.org) + [Prisma ORM](https://www.prisma.io) |
| **AI** | [Google Gemini 2.5 Flash](https://ai.google.dev) (receipt OCR + financial insights) |
| **Background Jobs** | [Inngest](https://www.inngest.com) (cron-triggered functions) |
| **Email** | [Resend](https://resend.com) + [React Email](https://react.email) |
| **Charts** | [Recharts](https://recharts.org) |
| **Animation** | [Motion](https://motion.dev) |
| **Deployment** | [Vercel](https://vercel.com) |

---

## Project Structure

```
welth/
├── middleware.js                   # Clerk route protection (dashboard / account / transaction)
├── next.config.mjs
├── components.json                 # shadcn/ui configuration
├── package.json
│
├── prisma/
│   ├── schema.prisma               # User, Account, Transaction, Budget models
│   └── migrations/
│
├── app/
│   ├── layout.js                   # Root layout (ClerkProvider + Header + Toaster)
│   ├── page.jsx                    # Animated landing page
│   ├── globals.css                 # Tailwind theme & custom styles
│   │
│   ├── (auth)/
│   │   ├── sign-in/[[...sign-in]]/ # Clerk sign-in
│   │   └── sign-up/[[...sign-up]]/ # Clerk sign-up
│   │
│   ├── (main)/
│   │   ├── dashboard/
│   │   │   ├── page.jsx            # Accounts grid + budget progress
│   │   │   └── _components/
│   │   │       ├── account-card.jsx
│   │   │       └── budget-progress.jsx
│   │   ├── transaction/
│   │   │   ├── create/page.jsx     # Add transaction + AI receipt scan
│   │   │   └── _components/
│   │   │       └── transaction-form.jsx
│   │   └── account/[id]/
│   │       ├── page.jsx            # Account detail + transaction history
│   │       └── _components/
│   │           ├── transaction-table.jsx
│   │           └── transaction-chart.jsx
│   │
│   └── api/
│       ├── inngest/route.js        # Inngest background function endpoint
│       └── seed/route.js           # Demo data seeding route
│
├── actions/                        # Server actions ("use server")
│   ├── account.js                  # Default account, account + transactions, bulk delete
│   ├── budget.js                   # Get & update monthly budget
│   ├── dashboard.js                # Create account, list user accounts
│   ├── transaction.js              # Create transaction + AI receipt scanner
│   ├── send-email.js               # Resend transport
│   └── seed.js                     # Generate 30 days of demo transactions
│
├── components/
│   ├── header.jsx                  # Sticky nav (Clerk SignedIn / SignedOut)
│   ├── logo.jsx
│   ├── navlink.jsx
│   ├── create-account-drawer.jsx
│   ├── global/                     # background, container, icons, cn
│   ├── section/                    # hero, difference, workflow, capability, cta, footer
│   └── ui/                         # shadcn/ui primitives
│
├── data/                           # Static landing content
│   ├── landing.js
│   ├── capability.js
│   ├── difference.js
│   ├── workflow.js
│   ├── routes.js
│   ├── navlinks.js
│   └── footer.js
│
├── hooks/
│   └── use-fetch.js
│
├── lib/
│   ├── prisma.js                   # Prisma client singleton
│   ├── checkUser.js                # Sync Clerk user → DB user
│   ├── utils.js
│   └── inngest/
│       ├── client.js               # Inngest client
│       └── functions.js            # Budget alerts + monthly reports cron jobs
│
└── emails/
    └── template.jsx                # React Email templates (report + budget alert)
```

---

## Quick Start

### Prerequisites
- Node.js 20+
- A PostgreSQL database ([Supabase](https://supabase.com), [Neon](https://neon.com), or local)
- A [Clerk](https://clerk.com) account (publishable + secret key)
- A [Google AI (Gemini)](https://ai.google.dev) API key
- A [Resend](https://resend.com) API key
- An [Inngest](https://www.inngest.com) account (event + signing key)

### Setup

```bash
# Clone the repo
git clone https://github.com/faizjamadar18/Finance-platform-powered-by-AI.git
cd Finance-platform-powered-by-AI

# Install dependencies (runs `prisma generate` on postinstall)
npm install

# Configure environment variables in .env
#   DATABASE_URL=          # pooled connection string
#   DIRECT_URL=            # direct connection string (migrations)
#   NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
#   CLERK_SECRET_KEY=
#   GEMINI_API_KEY=
#   RESEND_API_KEY=
#   INNGEST_EVENT_KEY=
#   INNGEST_SIGNING_KEY=

# Apply the database schema
npx prisma migrate deploy

# Run the app
npm run dev
```

Optional extras:

```bash
npm run email    # Preview React Email templates locally
npm run lint     # Run ESLint
```

---

## Database Schema

| Table | Purpose | Key Columns |
|-------|---------|-------------|
| `users` | App users synced from Clerk | `id`, `clerkUserId` (unique), `email` (unique), `name`, `imageUrl` |
| `accounts` | Bank / cash accounts | `id`, `name`, `type` (`CURRENT`/`SAVINGS`), `balance`, `isDefault`, `userId` (FK) |
| `transactions` | Income & expense records | `id`, `type` (`INCOME`/`EXPENSE`), `amount`, `description`, `date`, `receiptUrl`, `status`, `userId` (FK), `accountId` (FK) |
| `budgets` | Monthly budget per user | `id`, `amount`, `lastAlertSent`, `userId` (unique FK) |

---

## Deployment

Welth deploys effortlessly on **Vercel**:

1. Push the repo to GitHub
2. Import the project into [Vercel](https://vercel.com/new)
3. Add all environment variables from your `.env` in the Vercel dashboard
4. Deploy, then add the production domain to your Clerk instance
5. Register the Inngest app so the cron functions run in production — done 🚀

---

## Roadmap

- [ ] Recurring transactions & subscriptions
- [ ] Multi-currency support
- [ ] CSV / PDF export of transactions
- [ ] Shared & family budgets
- [ ] Bank sync (Plaid / open banking)
- [ ] Mobile app

---

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

<div align="center">
  <sub>Built with 💚 for smarter spending</sub>
  <br/>
  <sub>© 2026 Welth — AI-Powered Personal Finance</sub>
</div>
