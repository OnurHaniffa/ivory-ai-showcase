# Ivory AI

> Full-stack AI-powered virtual receptionist and lead generation platform for local businesses.

![Status](https://img.shields.io/badge/Status-Active_Development-brightgreen)
![Python](https://img.shields.io/badge/Python-3.12-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-412991)
![Tests](https://img.shields.io/badge/Tests-141%2B_passing-success)

> **Note:** This repository is a project showcase. The source code is maintained in a private repository.

---

## Overview

Ivory AI is a production-grade SaaS platform that serves as an AI-powered virtual receptionist for local businesses, starting with dental practices. The system operates on two levels:

1. **B2B Lead Generation Engine** — Automatically discovers businesses via Google Maps scraping, analyzes their websites for gaps (no chatbot, no online booking, no after-hours support), enriches leads with owner data, generates personalized AI chatbot demos, and sends targeted outreach emails.

2. **Customer-Facing SaaS Product** — Business owners sign up, go through automated onboarding (website auto-scraped), and receive an embeddable AI chatbot widget. The chatbot acts as a virtual receptionist — answering questions, capturing leads, and handling after-hours visitors.

**Business Model:** 30-day free trial → $97/month per business via Lemon Squeezy.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        IVORY AI PLATFORM                         │
├─────────────┬──────────────┬──────────────┬─────────────────────┤
│  Scraping   │   Chatbot    │   Outreach   │   Customer Portal   │
│  Pipeline   │   Engine     │   System     │   & Dashboard       │
├─────────────┼──────────────┼──────────────┼─────────────────────┤
│ Google Maps │ OpenAI GPT-4 │ Resend Email │ JWT Auth + Sessions │
│ Playwright  │ Dynamic      │ Open/Click   │ Widget Customizer   │
│ Website     │ Prompts from │ Tracking     │ Conversation Viewer │
│ Analyzer    │ Scraped Data │ Follow-up    │ Lead Management     │
│ Hunter.io   │ Lead Capture │ Automation   │ Billing Portal      │
├─────────────┴──────────────┴──────────────┴─────────────────────┤
│              FastAPI + SQLAlchemy + PostgreSQL                    │
│              Docker + Railway Deployment                         │
└─────────────────────────────────────────────────────────────────┘
```

---

## Tech Stack

### Backend
| Technology | Purpose |
|---|---|
| **FastAPI** | Async web framework — the entire REST API |
| **SQLAlchemy 2.0** | Async ORM with PostgreSQL (prod) / SQLite (dev) |
| **Alembic** | Database migrations (8 migration files) |
| **OpenAI API** (GPT-4o-mini) | Powers the conversational AI chatbot |
| **Playwright** | Headless browser for Google Maps scraping & website analysis |
| **BeautifulSoup + lxml** | HTML parsing for content extraction |
| **Resend** | Transactional email delivery |
| **Hunter.io** | Email discovery for lead enrichment |
| **APScheduler** | Background job scheduling |
| **PyJWT + bcrypt** | Authentication with HTTP-only cookie tokens |
| **Pydantic Settings** | Typed configuration management |

### Frontend
| Technology | Purpose |
|---|---|
| **Tailwind CSS** | Self-hosted, purged/minified build (55KB) |
| **Vanilla JavaScript** | Embeddable chat widget (~400 lines) |
| **Alpine.js** | Lightweight dashboard interactivity |
| **Jinja2** | Server-side HTML templating |

### Infrastructure
| Technology | Purpose |
|---|---|
| **Docker** | Production-ready containerization |
| **Railway** | Deployment platform |
| **PostgreSQL** | Production database |
| **Lemon Squeezy** | Payment processing & subscription management |
| **Google Analytics 4** | Conditional analytics |

---

## Key Features

### Intelligent Lead Generation Pipeline

- **Google Maps Scraping** — Playwright-based with anti-detection (randomized user agents, viewports, stealth scripts, consent dialog handling) + Outscraper API fallback
- **Website Intelligence** — Detects 23+ chatbot providers (Intercom, Drift, Zendesk, Tidio...), 15+ booking platforms, CMS detection (WordPress, Squarespace, Wix...), SSL, mobile viewport, social links, and more
- **Opportunity Scoring** — Algorithm scores businesses 0-10 based on detected gaps
- **Lead Enrichment** — Owner/dentist name extraction from team pages + Hunter.io email discovery with pattern-based fallback
- **Pipeline Orchestration** — Single CLI command: scrape → analyze → enrich → build demos

### AI Chatbot Engine

- **GPT-4o-mini Powered** with per-business personalized system prompts (2000+ chars)
- **Dynamic Prompt Generation** from scraped data: services, hours, insurance, team members, FAQ, raw website content
- **Multi-Business-Type Support** — dental, legal, medical, home services, accounting, real estate
- **Intelligent Lead Capture** woven into natural conversation flow
- **After-Hours Awareness** using timezone-based context
- **Session Management** with LRU eviction (max 1000 concurrent sessions)

### Embeddable Chat Widget (`ivory.js`)

- Self-contained, drop-in JavaScript widget (~400 lines)
- Theme system (dentist, lawyer, software, medical, general) with custom colors
- Dynamic config fetching from server for live customization
- Quick reply buttons, typing indicators, proactive greeting popup
- Full-screen mobile mode with responsive design
- Accessibility: `prefers-reduced-motion`, ARIA labels

### Email Outreach System

- Personalized initial outreach with gap-based observations
- Automated 7-day follow-up sequences
- Open/click tracking via tracking pixel and redirect URLs
- Business-hours-aware sending windows (timezone-aware per US state)
- Daily sending caps with configurable limits
- Unsubscribe with HMAC-signed URLs

### Customer Self-Service Portal

- Full auth system: signup, email verification, login, forgot/reset password
- Rate limiting (5 login attempts/15 min), account lockout (10 failures → 30-min lock)
- Timing attack prevention (bcrypt always runs, even on non-existent accounts)
- Onboarding flow, dashboard, conversation viewer, lead management
- Widget customization and account settings

### SEO & Marketing

- 10 city-specific landing pages targeting local SEO
- 4 industry-specific landing pages
- 10 blog articles targeting high-intent keywords
- JSON-LD structured data throughout
- Self-hosted assets for performance

---

## Security

- **CSRF Middleware** — Origin-based validation for cookie-authenticated routes
- **Security Headers** — CSP, X-Frame-Options, HSTS, X-Content-Type-Options, Referrer-Policy, Permissions-Policy
- **Webhook Signature Verification** — HMAC SHA-256 for Lemon Squeezy webhooks
- **Production Startup Guards** — App crashes on boot if default secrets are unchanged
- **Token Versioning** — All sessions invalidated on password change
- **Rate Limiting** — Per-IP limits on login, signup, and password reset endpoints

---

## Testing

**141+ tests** covering every module:

- Config & database layer
- Scraper & website analyzer
- Chatbot engine & prompt generation
- Pipeline orchestration
- Email sender & tracking
- Authentication & authorization
- Billing & webhook handling
- Dashboard & admin routes
- Security middleware
- Widget configuration
- Blog & SEO pages
- Background scheduler

---

## Project Metrics

| Metric | Value |
|---|---|
| **Source Files** | 100+ |
| **Database Models** | 5 models, 80+ columns |
| **API Route Modules** | 11 |
| **Database Migrations** | 8 |
| **Test Count** | 141+ |
| **Chatbot Provider Detections** | 23+ |
| **CMS Detections** | 9+ |
| **Landing Pages** | 24+ |
| **Development Period** | Feb 2025 – Present (active) |

---

## My Role

I built this entire platform solo — backend, frontend, infrastructure, security, email system, payment integration, AI chatbot engine, scraping pipeline, and SEO. Every line of code, every database migration, every test.

---

## Contact

**Onur Haniffa** — [onurhaniffa.com](https://onurhaniffa.com) · [LinkedIn](https://www.linkedin.com/in/onurhaniffa/)
