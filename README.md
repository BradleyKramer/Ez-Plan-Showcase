# Easy-Plan — AI-Powered Travel Itinerary Planner

A full-stack travel planning application that combines curated destination templates with AI-generated itineraries, real-time budget tracking, and a comprehensive admin control center — built for travelers who want structure without the hassle.

> **Live Demo:** [ezplan.lovable.app](https://ezplan.lovable.app)

> **Recorded Demo:** ![Demo](demo/demo.gif) Coming Soon!
 
---

## Overview

Easy-Plan lets users browse 15+ curated destinations, select or customize pre-built itineraries, or generate entirely new ones using AI. Every trip includes day-by-day scheduling with drag-and-drop reordering, per-activity cost estimates, lodging selection, and budget tracking. The platform supports 20 languages, multiple currencies, and provides a conversational AI trip assistant for real-time travel advice.

On the backend, an admin dashboard surfaces analytics on user behavior, trip trends, API health, and community feedback — all protected by role-based access control.

---

## Core Features

### Trip Planning
- **Curated Destination Templates** — Pre-built itineraries for Paris, Tokyo, New York, Barcelona, and 11 other destinations with region/duration filtering
- **AI Itinerary Generator** — Generates structured day-by-day plans via Gemini, scoped by destination, duration, budget, and interests
- **Drag-and-Drop Customization** — Reorder activities, adjust times, swap events, and add custom activities with `@dnd-kit`
- **Budget Tracker** — Per-activity cost ranges, lodging estimates, and real-time budget utilization with visual progress indicators
- **Trip Lifecycle Management** — Trips flow through `planning → booked → completed` with persistent state in the database

### AI Capabilities
- **Trip Assistant Chatbot** — Streaming conversational AI (Gemini 3 Flash) for destination recommendations, packing tips, and itinerary advice
- **Smart Itinerary Generation** — Structured tool-calling that returns typed day/activity/cost data, not free text
- **Feedback Summarizer** — AI-generated summaries of user feedback with staleness detection and cached persistence

### User Experience
- **Multi-Language Support** — 20 languages with cached translations via a dedicated edge function
- **Currency Conversion** — Real-time exchange rates with persistent caching
- **Guided Walkthrough** — First-login onboarding tour (appears once per user, every login for testers)
- **Community Feedback System** — Users submit bugs/feature requests with media attachments; public status tracking

### Authentication & Authorization
- **Email/Password Auth** — Signup with email verification, password reset flow
- **Role-Based Access Control** — `admin`, `tester`, and `user` roles stored in a dedicated `user_roles` table
- **RLS-Protected Data** — Row-level security policies on all user-facing tables
- **Account Deletion** — Self-service with 30-day grace period and admin reinstatement capability

---

## System Architecture

```
├── Frontend (React + Vite + TypeScript)
│   ├── Pages: Home, Itineraries, Trip Details, My Trips, Profile, Admin, Feedback
│   ├── Providers: Currency, Translation, Query (TanStack), Tooltip
│   └── State: React Query for server state, local state for UI
│
├── Backend (Lovable Cloud / Supabase)
│   ├── Database: 16 tables with RLS policies
│   ├── Edge Functions:
│   │   ├── generate-itinerary    — AI itinerary generation with tool-calling
│   │   ├── trip-assistant        — Streaming chat completions
│   │   ├── translate-ui          — Batch UI string translation
│   │   ├── currency-rates        — Exchange rate fetching + caching
│   │   ├── summarize-feedback    — AI feedback analysis
│   │   ├── api-gateway           — Centralized API routing
│   │   ├── delete-account        — User self-service deletion
│   │   ├── admin-delete-user     — Admin account management
│   │   ├── reinstate-account     — Recover deleted accounts
│   │   ├── get-signed-url        — Secure file access
│   │   └── record-daily-metrics  — Scheduled analytics snapshots
│   │
│   └── Auth: Email/password with role-based access
│
└── AI Layer (Lovable AI Gateway)
    ├── Model: google/gemini-3-flash-preview
    ├── Streaming responses for chat
    └── Structured tool-calling for itinerary generation
```

---

## Admin Dashboard

The admin panel is organized into seven functional tabs:

| Tab | Capabilities |
|-----|-------------|
| **Analytics** | Destination heatmaps, booking funnels, cohort retention, peak planning patterns, budget utilization, CSV export |
| **Events** | CRUD for destination activities with auto-generated short IDs, cost ranges, and duration |
| **Accounts** | User management, flag system, trip oversight, dual-admin deletion verification |
| **Destinations** | Metadata editing, image management (primary + secondary), region assignment |
| **Feedback** | Status tracking, admin notes, AI-powered summarization with staleness detection |
| **Plans** | Internal roadmap with Kanban board, sticky to-dos, and prompt cost tracking |
| **Tester Notes** | Communication channel between testers and admins with rich text and media |

---

## Analytics & Data

- **API Usage Logging** — Every AI call logs feature name, response time, success/failure, and metadata to `api_usage_logs`
- **Credit Cost Tracking** — Daily snapshots of development credit usage with historical charting
- **Trip Analytics** — Average trip duration, budget utilization, itinerary type breakdown (template vs AI vs custom)
- **Review Analytics** — Per-destination ratings, tag co-occurrence matrices, rating distributions
- **User Retention Funnel** — Registered → Created Trip → Booked → Completed conversion tracking
- **Feature Kill Switches** — Global and per-feature API toggles with custom disabled messages

---

## Screenshots

### Core Experience

| Home Page | Example Destination | My Trips | Itinerary Customization |
|-----------|---------------------|----------|--------------------------|
| ![Home](images/home.png) | ![Destination](images/destination.png) | ![My Trips](images/my-trips.png) | ![Itinerary](images/itinerary.png) |

---

### 🤖 AI Features

| AI Itinerary Generation | AI Feedback Summary |
|------------------------|-------------------|
| ![AI 1](images/ai-1.png) | ![AI 2](images/ai-2.png) |

---

### Admin Dashboard

| Admin Tab | Admin Analytics |
|-----------|----------------|
| ![Admin 1](images/admin-1.png) | ![Admin 2](images/admin-2.png) |

---

### User & Feedback

| Profile | Settings | Help | Feedback |
|---------|----------|------|----------|
| ![Profile](images/profile.png) | ![Settings](images/settings.png) | ![Help](images/help.png) | ![Feedback](images/feedback.png) |

## License

This project is proprietary. All rights reserved.
