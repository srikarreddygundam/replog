# Gym App - Product Requirements & System Design Document

**Version:** 1.0  
**Date:** July 2026  
**Status:** Design Phase - Ready for Development  
**Team:** Product & Engineering

---

## Table of Contents
1. [Product Overview](#product-overview)
2. [User Stories & Acceptance Criteria](#user-stories--acceptance-criteria)
3. [Feature Breakdown](#feature-breakdown)
4. [System Architecture](#system-architecture)
5. [Database Schema](#database-schema)
6. [API Endpoints](#api-endpoints)
7. [Tech Stack & Tool Justification](#tech-stack--tool-justification)
8. [Screen Designs & Flow](#screen-designs--flow)
9. [MVP Scope & Phases](#mvp-scope--phases)
10. [Deployment & Free Tier Strategy](#deployment--free-tier-strategy)

---

## Product Overview

### Vision
A **personalized AI-powered gym workout app** that helps users:
- Receive intelligent workout recommendations based on fitness level
- Get real-time AI feedback on exercise selection
- Log workouts with form guidance
- Track progress over time with analytics (Phase 2)

### Target User
Fitness enthusiasts (Beginner to Advanced) who want structured, intelligent workout guidance without hiring a personal trainer.

### MVP Goal
**Build & validate the core workout logging + AI feedback loop** (Screens 1-5 of 6)

---

## User Stories & Acceptance Criteria

### US-1: User Registration
**As a** new user  
**I want to** create an account with my basic info and fitness level  
**So that** the app can personalize my workout recommendations

**Acceptance Criteria:**
- [ ] User can enter: Name, Age, Email, Password
- [ ] User selects fitness level: Beginner or Non-Beginner
- [ ] Password must be >= 8 characters
- [ ] Email must be unique in the system
- [ ] On success, user is redirected to Home Screen (with date banner)
- [ ] User auth token is stored securely
- [ ] Form validation shows clear error messages

**API Needed:** `POST /auth/signup`

---

### US-2: Home Screen with Date Navigation
**As a** logged-in user  
**I want to** see today's date prominently and navigate to past/future dates  
**So that** I can select which day to work out for

**Acceptance Criteria:**
- [ ] Home screen shows a horizontal date banner (today highlighted in blue)
- [ ] User can swipe/click left-right to navigate dates
- [ ] Clicking a date filters the rest of the page to that date
- [ ] Shows count of exercises planned for selected date
- [ ] Date format: "Mon, Jul 30" or similar
- [ ] Visual distinction between: today, past, future dates

**Data Needed:** None initially (just UI state)

---

### US-3: Muscle Group Multi-Select
**As a** user on home screen  
**I want to** select 1-2 muscle groups for a workout  
**So that** I get exercises tailored to those groups

**Acceptance Criteria:**
- [ ] Display muscle group buttons: Chest, Back, Shoulders, Biceps, Triceps, Abs, Legs
- [ ] User can select 1 or 2 muscle groups
- [ ] Selected groups are highlighted/checked
- [ ] "Next" button is enabled only when 1-2 groups are selected
- [ ] Clicking "Next" takes user to Variations Screen
- [ ] Visual feedback on selection (checkmark, color change, etc.)

**API Needed:** None (UI state only for now)

---

### US-4: Exercise Variations Screen
**As a** user after selecting muscle groups  
**I want to** see all exercise variations ranked by effectiveness  
**So that** I know which exercises are most impactful

**Acceptance Criteria:**
- [ ] Screen shows exercises grouped by selected muscle groups
- [ ] Within each group, variations sorted: Most Effective → Least Effective
- [ ] Each exercise shows: name, equipment needed, effectiveness rank
- [ ] User can select/deselect exercises (multi-select, checkboxes)
- [ ] "Next" button only enabled when at least 1 exercise selected
- [ ] Example: If user selected Shoulders + Triceps:
  - Shoulders: Barbell Press (rank 1), Dumbbell Press (rank 2), Machine Press (rank 3)
  - Triceps: Barbell Dips (rank 1), Tricep Pushdown (rank 2), Overhead Extension (rank 3)

**Data Needed:** 
- Exercise library (static, seeded in DB)
- Each exercise has: name, muscle_group, effectiveness_rank, equipment_needed

**API Needed:** `GET /exercises?muscle_groups=shoulders,triceps`

---

### US-5: AI Feedback on Exercise Selection
**As a** user after selecting exercises  
**I want to** receive AI feedback on my selection  
**So that** I can optimize my workout for my fitness level

**Acceptance Criteria:**
- [ ] AI analyzes selection against user's fitness level (Beginner/Non-Beginner)
- [ ] Feedback returns one of: "Great", "Good", "Can Improve"
- [ ] Feedback includes 2-3 sentence explanation (e.g., "Great! You've selected compound movements first, which is ideal for beginners. Consider adding 1-2 isolation exercises for triceps.")
- [ ] Beginner-specific rules:
  - Should prioritize compound movements
  - Should not have too many isolation exercises
  - Should have balanced muscle work (not just heavy on one muscle)
- [ ] User can "Accept" feedback or "Adjust Selection" (goes back to US-4)
- [ ] On "Accept", proceed to Exercise Ordering Screen

**AI Logic:**
- If selection is heavy on compounds (70%+) AND balanced between selected muscles: "Great"
- If selection is decent but slight imbalance: "Good"
- If too many isolations for a beginner OR severe imbalance: "Can Improve"

**API Needed:** 
- `POST /ai/feedback` (sends selected exercises + fitness level, returns feedback from Claude API)

---

### US-6: AI Exercise Ordering
**As a** user after AI feedback  
**I want to** see AI's recommended exercise order AND manually reorder if needed  
**So that** I follow proper progression (compound → isolation) but stay flexible

**Acceptance Criteria:**
- [ ] AI orders exercises: Compound exercises first, Isolation last
- [ ] Show reason for each placement (e.g., "Compound - do first to maximize strength gains")
- [ ] User can drag-and-drop to manually reorder exercises
- [ ] Visual indicators: compound exercises have different styling than isolation
- [ ] "Confirm Order" button locks in the order and proceeds to Exercise Logging
- [ ] Selected order is displayed clearly (1st, 2nd, 3rd, etc.)

**AI Logic:**
- Compound exercises: Barbell Press, Dumbbell Press, Dips, Pull-ups, Rows, Squats
- Isolation exercises: Machine Press, Tricep Pushdown, Leg Curls, Bicep Curls, etc.

**API Needed:**
- `POST /ai/order` (sends selected exercises, returns ordered list with reasoning)

---

### US-7: Exercise Logging
**As a** user ready to work out  
**I want to** log weight, reps, and sets for each exercise  
**I also want to** see form instructions for each exercise  
**So that** I can train properly and track my performance

**Acceptance Criteria:**
- [ ] For each exercise in order, show:
  - Exercise name & image (if available)
  - **Form instructions** (2-3 sentences, pre-written)
  - Input fields: Weight (lbs/kg), Reps, Sets
  - "Log" button to record this exercise
  - "Skip" option if user wants to skip an exercise
- [ ] Form instructions are pre-stored in DB (not AI-generated for MVP)
- [ ] User can log multiple sets (e.g., 3 sets of 10 reps at 185 lbs)
- [ ] Visual confirmation after each exercise logged
- [ ] "Next Exercise" button to move to next in the ordered list
- [ ] After last exercise, show "Workout Complete" screen

**Data Structure Example:**
```
Exercise Log Entry:
{
  user_id: "123",
  date: "2026-07-30",
  exercise_id: "barbell_press",
  weight: 185,
  weight_unit: "lbs",
  reps: 10,
  sets: 3,
  timestamp: "2026-07-30T14:30:00Z"
}
```

**API Needed:**
- `GET /exercises/:id/form` (fetch form instructions for an exercise)
- `POST /workouts/log` (save exercise log entry)

---

### US-8: Workout Completion Confirmation
**As a** user who finished logging all exercises  
**I want to** see a completion message  
**So that** I have a sense of accomplishment

**Acceptance Criteria:**
- [ ] Show: "Workout Complete! 🎉"
- [ ] Display summary: "Logged 6 exercises in 45 minutes"
- [ ] Show button: "View Workout History" (goes to home screen, selects same date)
- [ ] Show button: "Start New Workout" (goes to home screen, selects today)
- [ ] Optionally show motivational message based on workout volume

---

## Feature Breakdown

### MVP Features (Sprint 1)
| Feature | Priority | Effort | Status |
|---------|----------|--------|--------|
| User Auth (Signup/Login) | P0 | M | Planned |
| Home Screen + Date Banner | P0 | M | Planned |
| Muscle Group Selection | P0 | S | Planned |
| Exercise Variations Display | P0 | M | Planned |
| AI Feedback (Claude API) | P0 | M | Planned |
| AI Exercise Ordering | P0 | M | Planned |
| Exercise Logging | P0 | L | Planned |
| Form Instructions Display | P0 | S | Planned |
| Completion Screen | P1 | S | Planned |

### Phase 2 Features (Post-MVP)
| Feature | Priority | Use Case |
|---------|----------|----------|
| Workout History View | P1 | User can see past workouts |
| Progress Charts | P1 | Track weight progression over time |
| Exercise Search | P2 | Find specific exercises |
| Custom Exercise Creation | P2 | User-created exercises |
| Rest Timer Between Sets | P2 | Timing assistance |
| Body Metrics Tracking | P2 | Weight, body fat %, measurements |

---

## System Architecture

### High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     FRONTEND (React)                         │
│  - Auth Pages (Signup/Login)                                │
│  - Home Screen (Date navigation)                            │
│  - Muscle Group Selection                                   │
│  - Exercise Variations                                      │
│  - AI Feedback Display                                      │
│  - Exercise Ordering (Drag-drop)                            │
│  - Exercise Logging Form                                    │
└────────────────────────┬────────────────────────────────────┘
                         │ (HTTPS)
                         ▼
┌─────────────────────────────────────────────────────────────┐
│             BACKEND API (Node.js + Express)                 │
│  - Auth Routes (/auth/signup, /auth/login)                 │
│  - Exercise Routes (/exercises, /exercises/:id)            │
│  - Workout Routes (/workouts/log)                          │
│  - AI Routes (/ai/feedback, /ai/order)                     │
│  - User Routes (/users/:id)                                │
│                                                              │
│  Middleware:                                                │
│  - JWT Auth (Supabase)                                      │
│  - Input Validation                                         │
│  - Error Handling                                           │
└────────────────────────┬────────────────────────────────────┘
                         │ (SQL)
                         ▼
┌─────────────────────────────────────────────────────────────┐
│           DATABASE (Supabase PostgreSQL)                    │
│  - users table                                              │
│  - exercises table                                          │
│  - workout_logs table                                       │
│  - ai_feedback_logs table (for tracking)                    │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
                  ┌──────────────┐
                  │ Claude API   │
                  │ (AI Feedback)│
                  └──────────────┘
```

### Tech Stack Rationale

| Layer | Technology | Why |
|-------|-----------|-----|
| Frontend | React 18 + Vite | Fast, modern, great dev experience; Vite for speed |
| State Mgmt | Redux Toolkit | Scalable state management; good for learning |
| UI Components | Tailwind CSS | Utility-first CSS, fast styling, no custom CSS needed |
| Form Handling | React Hook Form | Lightweight, performant form validation |
| HTTP Client | Axios | Simple, promise-based API calls |
| Backend | Node.js + Express | Lightweight, perfect for learning; JavaScript full-stack |
| Database | Supabase (PostgreSQL) | Free tier, real PostgreSQL, built-in auth, excellent for learning |
| AI Integration | Anthropic Claude API | State-of-the-art LLM for workout feedback; free tier available |
| Authentication | Supabase Auth (JWT) | Built into Supabase, reduces boilerplate |
| Deployment | Vercel (Frontend) + Railway (Backend) | Both have free tiers; Vercel for Next.js or static React, Railway for Node backend |

### Why Supabase?
1. **Free tier** includes 500MB database, 2GB bandwidth, real-time subscriptions
2. **PostgreSQL** (industry standard) — great for learning
3. **Built-in auth** (JWT, OAuth) — no need to build auth from scratch
4. **Real-time** — can use for live updates later
5. **Direct SQL access** — learn SQL deeply
6. **Excellent docs** — perfect for learning

### Why Claude API?
1. **Free tier** includes $5/month free credits (plenty for MVP testing)
2. **Best-in-class reasoning** — great for workout feedback logic
3. **Structured outputs** — can reliably parse feedback responses
4. **Easy integration** — simple REST API

---

## Database Schema

### Users Table
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  name VARCHAR(255) NOT NULL,
  age INT NOT NULL,
  fitness_level VARCHAR(50) NOT NULL, -- 'beginner' or 'non-beginner'
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Index for auth lookups
CREATE INDEX idx_users_email ON users(email);
```

### Exercises Table
```sql
CREATE TABLE exercises (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  muscle_group VARCHAR(50) NOT NULL, -- 'chest', 'back', 'shoulders', 'biceps', 'triceps', 'abs', 'legs'
  exercise_type VARCHAR(50) NOT NULL, -- 'compound' or 'isolation'
  effectiveness_rank INT NOT NULL, -- 1 (most effective) to N
  equipment_needed VARCHAR(255),
  form_instructions TEXT NOT NULL, -- 2-3 sentences on proper form
  image_url VARCHAR(500), -- Optional image of the exercise
  created_at TIMESTAMP DEFAULT NOW()
);

-- Indexes for fast lookups
CREATE INDEX idx_exercises_muscle_group ON exercises(muscle_group);
CREATE INDEX idx_exercises_effectiveness ON exercises(effectiveness_rank);
```

### Workout Logs Table
```sql
CREATE TABLE workout_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  exercise_id UUID NOT NULL REFERENCES exercises(id),
  workout_date DATE NOT NULL, -- The date of the workout
  weight DECIMAL(10, 2) NOT NULL, -- Weight lifted
  weight_unit VARCHAR(10) DEFAULT 'lbs', -- 'lbs' or 'kg'
  reps INT NOT NULL,
  sets INT NOT NULL,
  logged_at TIMESTAMP DEFAULT NOW(),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Indexes for fast queries
CREATE INDEX idx_workout_logs_user_id ON workout_logs(user_id);
CREATE INDEX idx_workout_logs_date ON workout_logs(workout_date);
CREATE INDEX idx_workout_logs_user_date ON workout_logs(user_id, workout_date);
```

### AI Feedback Logs Table (for analytics)
```sql
CREATE TABLE ai_feedback_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  selected_exercises TEXT NOT NULL, -- JSON array of exercise IDs
  feedback_rating VARCHAR(20) NOT NULL, -- 'great', 'good', 'can_improve'
  feedback_text TEXT NOT NULL, -- The actual feedback from Claude
  created_at TIMESTAMP DEFAULT NOW()
);

-- Index for user analysis
CREATE INDEX idx_ai_feedback_user_id ON ai_feedback_logs(user_id);
```

### RLS (Row-Level Security) Rules
```sql
-- Users can only see their own data
ALTER TABLE workout_logs ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can only see their own workouts"
  ON workout_logs
  FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own workouts"
  ON workout_logs
  FOR INSERT
  WITH CHECK (auth.uid() = user_id);
```

---

## API Endpoints

### Authentication

#### `POST /auth/signup`
**Request:**
```json
{
  "email": "sree@example.com",
  "password": "SecurePass123",
  "name": "Sree",
  "age": 28,
  "fitness_level": "beginner"
}
```

**Response (201 Created):**
```json
{
  "id": "user-uuid-123",
  "email": "sree@example.com",
  "name": "Sree",
  "fitness_level": "beginner",
  "auth_token": "eyJhbGciOiJIUzI1NiIs..." 
}
```

**Errors:**
- 400: Invalid email or password format
- 409: Email already exists

---

#### `POST /auth/login`
**Request:**
```json
{
  "email": "sree@example.com",
  "password": "SecurePass123"
}
```

**Response (200):**
```json
{
  "id": "user-uuid-123",
  "email": "sree@example.com",
  "auth_token": "eyJhbGciOiJIUzI1NiIs..."
}
```

---

### Exercises

#### `GET /exercises?muscle_groups=shoulders,triceps`
**Response (200):**
```json
{
  "exercises": [
    {
      "id": "barbell_press_id",
      "name": "Barbell Shoulder Press",
      "muscle_group": "shoulders",
      "exercise_type": "compound",
      "effectiveness_rank": 1,
      "equipment_needed": "Barbell, Bench",
      "form_instructions": "Sit on bench with back support. Grip barbell at shoulder height. Press bar overhead in controlled motion until arms are extended."
    },
    {
      "id": "dumbbell_press_id",
      "name": "Dumbbell Shoulder Press",
      "muscle_group": "shoulders",
      "exercise_type": "compound",
      "effectiveness_rank": 2,
      "equipment_needed": "Dumbbells, Bench"
    }
    // ... more exercises
  ]
}
```

---

#### `GET /exercises/:id/form`
**Response (200):**
```json
{
  "exercise_id": "barbell_press_id",
  "name": "Barbell Shoulder Press",
  "form_instructions": "Sit on bench with back support. Grip barbell at shoulder height. Press bar overhead in controlled motion until arms are extended. Avoid arching lower back excessively to prevent injury.",
  "image_url": "https://..."
}
```

---

### AI Endpoints

#### `POST /ai/feedback`
**Request:**
```json
{
  "selected_exercise_ids": ["barbell_press_id", "dumbbell_press_id", "tricep_pushdown_id"],
  "muscle_groups": ["shoulders", "triceps"],
  "fitness_level": "beginner"
}
```

**Response (200):**
```json
{
  "rating": "good",
  "feedback": "Good selection! You've chosen solid compound movements for shoulders. Consider adding one more compound exercise for triceps (like Dips or Close-Grip Bench) to maximize strength gains for a beginner.",
  "reasoning": "For a beginner, you want 70% compound and 30% isolation. Your selection is 60% compound, which is slightly light but acceptable."
}
```

---

#### `POST /ai/order`
**Request:**
```json
{
  "selected_exercise_ids": ["barbell_press_id", "dumbbell_press_id", "tricep_pushdown_id"],
  "fitness_level": "beginner"
}
```

**Response (200):**
```json
{
  "ordered_exercises": [
    {
      "position": 1,
      "exercise_id": "barbell_press_id",
      "name": "Barbell Shoulder Press",
      "exercise_type": "compound",
      "reasoning": "Compound exercises first - maximize strength gains when fresh"
    },
    {
      "position": 2,
      "exercise_id": "dumbbell_press_id",
      "name": "Dumbbell Shoulder Press",
      "exercise_type": "compound",
      "reasoning": "Secondary compound for shoulders - builds strength and stability"
    },
    {
      "position": 3,
      "exercise_id": "tricep_pushdown_id",
      "name": "Tricep Pushdown",
      "exercise_type": "isolation",
      "reasoning": "Isolation exercise last - triceps already fatigued from presses, finish strong"
    }
  ]
}
```

---

### Workout Logging

#### `POST /workouts/log`
**Request:**
```json
{
  "exercise_id": "barbell_press_id",
  "workout_date": "2026-07-30",
  "weight": 185,
  "weight_unit": "lbs",
  "reps": 10,
  "sets": 3
}
```

**Response (201 Created):**
```json
{
  "id": "log-uuid-456",
  "user_id": "user-uuid-123",
  "exercise_id": "barbell_press_id",
  "workout_date": "2026-07-30",
  "weight": 185,
  "reps": 10,
  "sets": 3,
  "logged_at": "2026-07-30T14:30:00Z"
}
```

---

#### `GET /workouts/:date`
**Query:** `/workouts/2026-07-30`

**Response (200):**
```json
{
  "workout_date": "2026-07-30",
  "exercises": [
    {
      "exercise_id": "barbell_press_id",
      "name": "Barbell Shoulder Press",
      "weight": 185,
      "reps": 10,
      "sets": 3
    },
    {
      "exercise_id": "dumbbell_press_id",
      "name": "Dumbbell Shoulder Press",
      "weight": 145,
      "reps": 8,
      "sets": 3
    }
  ]
}
```

---

### User Routes

#### `GET /users/me`
**Headers:** `Authorization: Bearer {auth_token}`

**Response (200):**
```json
{
  "id": "user-uuid-123",
  "email": "sree@example.com",
  "name": "Sree",
  "age": 28,
  "fitness_level": "beginner"
}
```

---

## Tech Stack & Tool Justification

### Frontend Stack

**React 18 + Vite**
- Why: Modern, fast, excellent DX (developer experience)
- Free tier: Yes (host on Vercel free)
- Learning value: High — React is industry standard

**Tailwind CSS**
- Why: Utility-first, no need to write custom CSS
- Free tier: Yes (open source)
- Learning value: Medium — you'll learn CSS utilities, responsive design

**Redux Toolkit**
- Why: Scalable state management for medium apps
- Free tier: Yes (open source)
- Learning value: High — state management is crucial skill

**React Hook Form**
- Why: Lightweight form validation and handling
- Free tier: Yes (open source)
- Learning value: Medium — forms are common in apps

**Axios**
- Why: Simple, promise-based HTTP client
- Free tier: Yes (open source)
- Learning value: Medium — understanding HTTP requests is essential

---

### Backend Stack

**Node.js + Express**
- Why: Lightweight, JavaScript full-stack (one language for learning), quick to prototype
- Free tier: Yes (open source)
- Learning value: High — you'll understand backend architecture, middleware, routing, error handling

**Supabase (PostgreSQL)**
- Why: Free tier is generous, SQL is industry standard, built-in auth
- Free tier: Yes (500MB DB, 2GB bandwidth/month, JWT auth)
- Learning value: High — SQL, database design, RLS (Row-Level Security)

**Dotenv**
- Why: Environment variable management (for API keys, DB secrets)
- Free tier: Yes (open source)
- Learning value: Low, but essential for security

---

### AI Integration

**Anthropic Claude API**
- Why: Best-in-class reasoning for feedback; free tier covers MVP
- Free tier: Yes ($5/month free credits)
- Learning value: High — you'll learn prompt engineering, API integration, handling LLM responses

**Prompt Strategy:**
- For feedback: "Given a beginner's selected exercises, evaluate if the selection is balanced between compound and isolation movements."
- For ordering: "Order these exercises by proper progression (compound → isolation)."

---

### Deployment & Hosting (All Free)

| Component | Service | Free Tier | Why |
|-----------|---------|-----------|-----|
| Frontend | Vercel | 100GB/month bandwidth | Built for React/Next.js, instant deploys |
| Backend | Railway | $5/month free credit (covers Node app) | Simple deployment, PostgreSQL support |
| Database | Supabase | 500MB, 2GB bandwidth | PostgreSQL, built-in features |
| AI | Claude API | $5/month free credits | Covers hundreds of requests |

**Total Cost: $0 forever (within free limits)**

---

## Screen Designs & Flow

### Screen 1: Sign Up
```
┌─────────────────────────┐
│                         │
│      GYM APP LOGO       │
│                         │
│  Sign Up                │
│  ─────────────────────  │
│                         │
│  Name: [____________]   │
│  Age:  [____________]   │
│  Email: [____________]  │
│  Password: [______]     │
│                         │
│  Fitness Level:         │
│  ○ Beginner             │
│  ○ Non-Beginner         │
│                         │
│  [   Sign Up    ]       │
│                         │
│  Already have account?  │
│  Login here             │
└─────────────────────────┘
```

### Screen 2: Home Screen (Date + Muscle Group)
```
┌─────────────────────────┐
│ < Jul 28 | Jul 29 | Jul 30 > │  ← Date Navigation
│                         │
│  Today                  │
│  July 30, 2026          │
│                         │
│  Select Muscle Groups   │
│                         │
│  [Chest] [Back]         │
│  [Shoulders] [Biceps]   │
│  [Triceps] [Abs] [Legs] │
│                         │
│  (User selects 2)       │
│                         │
│         [ Next ]        │
└─────────────────────────┘
```

### Screen 3: Exercise Variations
```
┌─────────────────────────┐
│  Shoulders (Ranked)     │
│                         │
│  ✓ 1. Barbell Press     │
│  ✓ 2. Dumbbell Press    │
│    3. Machine Press     │
│                         │
│  Triceps (Ranked)       │
│                         │
│  ✓ 1. Barbell Dips      │
│    2. Tricep Pushdown   │
│                         │
│  (4 selected)           │
│                         │
│         [ Next ]        │
└─────────────────────────┘
```

### Screen 4: AI Feedback
```
┌─────────────────────────┐
│  Your Workout Plan      │
│                         │
│  🎯 GOOD                │
│                         │
│  "Good selection! Your  │
│   mix of compounds and  │
│   isolations is great   │
│   for beginners. Add 1  │
│   more chest exercise   │
│   for balance."         │
│                         │
│  [ Accept ] [ Adjust ]  │
└─────────────────────────┘
```

### Screen 5: Exercise Ordering
```
┌─────────────────────────┐
│  Workout Order          │
│  (drag to reorder)      │
│                         │
│  1. Barbell Press       │
│     (Compound - first)  │
│  ⋮                      │
│  2. Dumbbell Press      │
│  ⋮                      │
│  3. Barbell Dips        │
│  ⋮                      │
│  4. Tricep Pushdown     │
│     (Isolation - last)  │
│                         │
│   [ Confirm Order ]     │
└─────────────────────────┘
```

### Screen 6: Exercise Logging
```
┌─────────────────────────┐
│  1. Barbell Press       │
│  ───────────────────    │
│                         │
│  Form: Sit on bench...  │
│  [exercise image]       │
│                         │
│  Weight: [185] lbs      │
│  Reps:   [10]           │
│  Sets:   [3]            │
│                         │
│  [ Log ]  [ Skip ]      │
│                         │
│  Progress: 4/6          │
└─────────────────────────┘
```

### Screen 7: Completion
```
┌─────────────────────────┐
│                         │
│  🎉 Workout Complete!   │
│                         │
│  You logged 6 exercises │
│  in 52 minutes          │
│                         │
│  Great job! You nailed  │
│  a solid beginner       │
│  workout!               │
│                         │
│  [ View History ]       │
│  [ New Workout ]        │
│                         │
└─────────────────────────┘
```

---

## MVP Scope & Phases

### Phase 1: Core Logging (MVP) — Weeks 1-4
**Goal:** Build Screens 1-6 + basic logging functionality

**Tasks (in order):**
1. ✅ System design (this document)
2. Setup project structure (React + Node backend)
3. Database setup (Supabase)
4. User auth (signup/login)
5. Home screen (date banner + muscle group select)
6. Exercise library (seed DB with exercises)
7. Variations screen
8. AI feedback integration
9. Exercise ordering
10. Exercise logging
11. Completion screen
12. Deploy frontend + backend

**Success Criteria:**
- User can sign up
- User can select muscle groups and exercises
- User receives AI feedback
- User can log a complete workout
- Data persists in Supabase

---

### Phase 2: Progress & History (Post-MVP)
**Goal:** Add history viewing and progress charts

**Features:**
- View past workouts by date
- See weight progression over time
- Charts (weight vs. date, volume trends)
- Personal records (PRs)

---

### Phase 3: Advanced (Future)
**Goal:** Enhance UX and add premium features

**Features:**
- Rest timer between sets
- Exercise video library
- Custom exercise creation
- Body metrics tracking
- Weekly/monthly reports
- Social sharing

---

## Deployment & Free Tier Strategy

### Frontend Deployment (Vercel)

**Steps:**
1. Push code to GitHub
2. Connect GitHub repo to Vercel (one-click)
3. Vercel auto-deploys on every push
4. Free tier includes: 100GB/month bandwidth, unlimited deployments

**Environment Variables:**
- `VITE_BACKEND_URL` = Railway backend URL
- `VITE_CLAUDE_API_KEY` (frontend doesn't need this; only backend does)

---

### Backend Deployment (Railway)

**Steps:**
1. Create Railway account (free)
2. Connect GitHub repo
3. Railway deploys automatically
4. Free tier: $5/month free credit (more than enough for Node + PostgreSQL)

**Environment Variables:**
- `DATABASE_URL` = Supabase connection string
- `ANTHROPIC_API_KEY` = Claude API key
- `JWT_SECRET` = Secret for signing tokens
- `NODE_ENV` = production

---

### Database (Supabase)

**Steps:**
1. Create Supabase project (free)
2. Run SQL migration scripts
3. Configure RLS (Row-Level Security)
4. Connect via Railway backend

**Free Tier Limits:**
- 500MB storage (more than enough for exercise library + logs)
- 2GB bandwidth/month (plenty for MVP)
- No auto-backups (acceptable for learning project)

---

## Security Considerations

### Frontend
- Never hardcode API keys in frontend code
- Use environment variables (loaded at build time)
- Store JWT token in localStorage (acceptable for MVP; use httpOnly cookies in production)

### Backend
- Validate all user inputs
- Use JWT for auth (provided by Supabase)
- Use RLS on database tables (Supabase rows visible only to user)
- Rate limit API endpoints (Railway supports this)

### Secrets Management
- Use `.env` file locally (never commit to Git)
- Use `.env.example` to document variables
- On Railway: set secrets in deployment UI

---

## Success Metrics (Phase 1)

1. ✅ User can complete full workout logging flow
2. ✅ Data persists across sessions
3. ✅ AI feedback is accurate and helpful
4. ✅ App is responsive on mobile
5. ✅ Zero runtime errors in first week of testing
6. ✅ Code is documented and pushed to GitHub

---

## Next Steps

1. **Developer**: Read this document in full
2. **Developer**: Set up local environment (Node, React, Supabase CLI)
3. **Standup**: Review design doc together (ask clarifications)
4. **Developer**: Start with Task 1 (project structure)
5. **Lead**: Provide step-by-step guidance as needed

---

## Glossary

**Compound Exercise:** Multi-joint movement (e.g., Barbell Press, Squat)  
**Isolation Exercise:** Single-joint movement (e.g., Bicep Curl, Leg Curl)  
**Effectiveness Rank:** 1 = most effective for the muscle group, N = least effective  
**RLS:** Row-Level Security — database-level access control  
**JWT:** JSON Web Token — standard for API authentication  
**Supabase:** PostgreSQL + Auth + Realtime APIs (managed service)  

---

**Document Version:** 1.0  
**Last Updated:** July 30, 2026  
**Next Review:** After Phase 1 completion
