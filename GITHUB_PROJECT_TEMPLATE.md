# RepLog - GitHub Project Board & Issues Template

**Purpose:** This template helps you set up a GitHub Project (Kanban board) and Issues to track Phase 1 development of RepLog (AI-powered workout logging app).

**How to use:**
1. Create a GitHub Project (classic or new table view)
2. Create Issues from each Epic/Task below
3. Link sub-tasks to parent issues
4. Move cards through: Backlog → In Progress → Review → Done

---

## GitHub Labels to Create

Create these labels in your repo (`Settings > Labels`):

```
Type Labels:
- type:epic (🔴 Red)
- type:feature (🔵 Blue)
- type:bug (🟠 Orange)
- type:documentation (📚 Purple)

Priority Labels:
- priority:p0 (High - blocking)
- priority:p1 (Medium - important)
- priority:p2 (Low - nice-to-have)

Status Labels:
- status:blocked (🔴 Blocked)
- status:in-progress (🟡 Yellow)
- status:review (🟠 Orange)
- status:done (🟢 Green)

Component Labels:
- component:frontend (React)
- component:backend (Node/Express)
- component:database (Supabase)
- component:ai (Claude API)
```

---

## Milestones

Create these milestones (in `Issues > Milestones`):

```
Milestone 1: MVP Phase 1 (Due: 4 weeks from start)
  - Core workflow: Auth → Muscle Select → AI Feedback → Logging
  
Milestone 2: Phase 2 (Due: Later)
  - History & Progress Charts
```

---

## Epic 1: RepLog - Project Setup & Infrastructure

**Epic Title:** `[EPIC] RepLog - Project Setup & Infrastructure`  
**Label:** `type:epic`, `priority:p0`, `component:frontend`, `component:backend`  
**Milestone:** RepLog MVP Phase 1  

**Description:**
Initialize RepLog project structure for both frontend and backend. Set up Supabase, environment variables, and CI/CD.

---

### Issue 1.1: Initialize React Frontend Project with Vite
**Title:** `[TASK] Initialize React Frontend with Vite`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Create a new React 18 project using Vite with:
- React 18
- Vite build tool
- Tailwind CSS
- Redux Toolkit
- React Hook Form
- Axios
- React Router

Acceptance Criteria:
- [ ] Project created with `npm create vite@latest`
- [ ] Tailwind CSS configured
- [ ] Redux store initialized (empty slices)
- [ ] Folder structure: src/components, src/pages, src/store, src/utils
- [ ] Vite dev server runs on localhost:5173
- [ ] `.env.example` created with template variables
- [ ] Pushed to GitHub

Time Estimate: 1.5 hours
```

**Sub-tasks:**
```
- [ ] Run `npm create vite@latest gym-app -- --template react`
- [ ] Install dependencies: tailwind, redux-toolkit, react-hook-form, axios, react-router-dom
- [ ] Configure Tailwind (tailwind.config.js, postcss.config.js)
- [ ] Create folder structure (components/, pages/, store/, utils/)
- [ ] Create Redux store (index.js, slices/)
- [ ] Create .env.example file
- [ ] Test dev server: `npm run dev`
- [ ] Commit and push to GitHub
```

---

### Issue 1.2: Initialize Node.js Backend with Express
**Title:** `[TASK] Initialize Node.js Backend with Express`  
**Label:** `type:feature`, `priority:p0`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Create a new Node.js + Express backend with:
- Express 4
- Dotenv for environment variables
- CORS for frontend communication
- Basic error handling middleware

Acceptance Criteria:
- [ ] Backend project initialized with npm
- [ ] Express server runs on localhost:3001
- [ ] CORS configured to allow frontend (localhost:5173)
- [ ] Folder structure: routes/, middleware/, controllers/, models/, db/
- [ ] `.env.example` created
- [ ] Server responds to GET /health with 200
- [ ] Pushed to GitHub

Time Estimate: 1.5 hours
```

**Sub-tasks:**
```
- [ ] Create project folder: mkdir gym-app-backend && cd gym-app-backend
- [ ] npm init -y
- [ ] Install: express, dotenv, cors, pg (PostgreSQL client)
- [ ] Create server.js (basic Express setup)
- [ ] Configure CORS middleware
- [ ] Create folder structure (routes/, controllers/, db/, middleware/)
- [ ] Create GET /health endpoint
- [ ] Create .env.example
- [ ] Test with curl: curl http://localhost:3001/health
- [ ] Commit and push to GitHub
```

---

### Issue 1.3: Setup Supabase Project & Database
**Title:** `[TASK] Setup Supabase & Seed Database`  
**Label:** `type:feature`, `priority:p0`, `component:database`  
**Milestone:** MVP Phase 1  

**Description:**
```
Create Supabase project and initialize database with:
- Users table
- Exercises table
- Workout logs table
- AI feedback logs table
- RLS policies
- Pre-seeded exercise library

Acceptance Criteria:
- [ ] Supabase project created
- [ ] All 4 tables created (schema from design doc)
- [ ] RLS policies configured
- [ ] Exercise library seeded (50+ exercises across 7 muscle groups)
- [ ] Connection string obtained
- [ ] Backend can connect and query

Time Estimate: 2 hours
```

**Sub-tasks:**
```
- [ ] Sign up for Supabase (free tier)
- [ ] Create new project (region: us-east-1 or closest to you)
- [ ] Run SQL migrations (create tables)
- [ ] Create users table with correct schema
- [ ] Create exercises table with correct schema
- [ ] Create workout_logs table
- [ ] Create ai_feedback_logs table
- [ ] Enable RLS on all tables
- [ ] Create RLS policies (users can only see own data)
- [ ] Seed exercises table (Chest, Back, Shoulders, Biceps, Triceps, Abs, Legs)
- [ ] Copy connection string & store in .env
- [ ] Test connection from backend with simple query
- [ ] Document connection process in README
```

---

### Issue 1.4: Setup Environment Variables & Secrets
**Title:** `[TASK] Configure Environment Variables & Secrets`  
**Label:** `type:feature`, `priority:p0`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Ensure all secrets are properly managed:

Acceptance Criteria:
- [ ] Frontend .env configured (API URL)
- [ ] Backend .env configured (DB, API keys, JWT secret)
- [ ] .env files in .gitignore (never commit secrets)
- [ ] .env.example files in both projects (for reference)
- [ ] README documents which env variables are needed
- [ ] No hardcoded API keys in code

Time Estimate: 30 minutes
```

**Sub-tasks:**
```
- [ ] Frontend: Create .env.local with VITE_BACKEND_URL=http://localhost:3001
- [ ] Backend: Create .env with DATABASE_URL, ANTHROPIC_API_KEY, JWT_SECRET, NODE_ENV
- [ ] Add .env and .env.local to .gitignore
- [ ] Create .env.example files
- [ ] Update README with env variable instructions
- [ ] Test: Backend can read env variables (console.log in server.js)
```

---

## Epic 2: Authentication System

**Epic Title:** `[EPIC] User Authentication (Signup/Login)`  
**Label:** `type:epic`, `priority:p0`, `component:frontend`, `component:backend`  
**Milestone:** MVP Phase 1  

---

### Issue 2.1: Create Signup API Endpoint
**Title:** `[TASK] Create POST /auth/signup Backend Endpoint`  
**Label:** `type:feature`, `priority:p0`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build signup endpoint that:
- Validates email & password format
- Hashes password (use bcrypt)
- Stores user in Supabase
- Returns JWT token

Acceptance Criteria:
- [ ] POST /auth/signup accepts { email, password, name, age, fitness_level }
- [ ] Returns 201 with user + auth_token on success
- [ ] Returns 400 with error message on invalid input
- [ ] Returns 409 if email already exists
- [ ] Password hashed before storage
- [ ] Endpoint tested with curl/Postman

Time Estimate: 2 hours
```

**Sub-tasks:**
```
- [ ] Install bcryptjs: npm install bcryptjs
- [ ] Create routes/auth.js
- [ ] Create controllers/authController.js
- [ ] Write signup logic (validation, hash, insert, generate token)
- [ ] Test with Postman: valid signup
- [ ] Test error case: duplicate email
- [ ] Test error case: invalid email
- [ ] Test error case: short password
```

---

### Issue 2.2: Create Login API Endpoint
**Title:** `[TASK] Create POST /auth/login Backend Endpoint`  
**Label:** `type:feature`, `priority:p0`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build login endpoint that:
- Finds user by email
- Compares password hash
- Returns JWT token

Acceptance Criteria:
- [ ] POST /auth/login accepts { email, password }
- [ ] Returns 200 with user + auth_token on success
- [ ] Returns 401 if credentials invalid
- [ ] Tested with Postman

Time Estimate: 1 hour
```

**Sub-tasks:**
```
- [ ] Write login logic in authController.js
- [ ] Test valid login
- [ ] Test invalid email
- [ ] Test wrong password
```

---

### Issue 2.3: Create Signup Page (React)
**Title:** `[TASK] Create Signup UI Component`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build signup form with:
- Name, Age, Email, Password inputs
- Fitness level radio buttons (Beginner/Non-Beginner)
- Form validation
- API call to /auth/signup
- Redirect to home on success

Acceptance Criteria:
- [ ] Form renders with all fields
- [ ] Validation shows errors (empty, invalid email)
- [ ] Password >= 8 characters enforced
- [ ] Fitness level is required (default none)
- [ ] Submit calls POST /auth/signup
- [ ] Token stored in localStorage
- [ ] Redirects to /home on success
- [ ] Error messages displayed

Time Estimate: 2 hours
```

**Sub-tasks:**
```
- [ ] Create pages/SignUp.jsx
- [ ] Use React Hook Form for form state
- [ ] Add input fields: name, age, email, password
- [ ] Add radio buttons: fitness level
- [ ] Add client-side validation
- [ ] Create API call function (utils/api.js)
- [ ] Handle API errors
- [ ] Store token in localStorage
- [ ] Use React Router to redirect
- [ ] Add Tailwind styling
- [ ] Test on localhost:5173
```

---

### Issue 2.4: Create Login Page (React)
**Title:** `[TASK] Create Login UI Component`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build login form with:
- Email & password inputs
- Submit button
- API call to /auth/login
- Redirect to home on success
- Link to signup

Acceptance Criteria:
- [ ] Form renders
- [ ] Validation working
- [ ] Submit calls POST /auth/login
- [ ] Token stored on success
- [ ] Redirects to /home
- [ ] Error messages shown

Time Estimate: 1.5 hours
```

**Sub-tasks:**
```
- [ ] Create pages/Login.jsx
- [ ] Add email & password inputs
- [ ] Add submit button
- [ ] Implement API call
- [ ] Add error handling
- [ ] Store token in localStorage
- [ ] Redirect to /home
- [ ] Add link to signup page
```

---

### Issue 2.5: Create Auth Context & Protected Routes
**Title:** `[TASK] Setup Auth Context & Route Protection`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Setup authentication context to:
- Store current user & token globally
- Provide ProtectedRoute component
- Prevent access to /home without auth
- Add logout functionality

Acceptance Criteria:
- [ ] Auth context created
- [ ] ProtectedRoute component prevents unauthorized access
- [ ] Logout clears token from localStorage
- [ ] User info available throughout app
- [ ] Refresh doesn't lose auth state

Time Estimate: 1.5 hours
```

**Sub-tasks:**
```
- [ ] Create context/AuthContext.jsx
- [ ] Create useAuth hook
- [ ] Create ProtectedRoute component
- [ ] Add logout function
- [ ] Wrap app with AuthProvider
- [ ] Setup route hierarchy (/login, /signup, /home)
- [ ] Test auth flow end-to-end
```

---

## Epic 3: Home Screen & Muscle Group Selection

**Epic Title:** `[EPIC] Home Screen & Muscle Group Selection`  
**Label:** `type:epic`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

---

### Issue 3.1: Create Home Screen Layout
**Title:** `[TASK] Create Home Screen UI`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build home screen with:
- Horizontal date navigation banner
- Today highlighted
- Muscle group button grid
- Next button

Acceptance Criteria:
- [ ] Date banner shows current date (today highlighted)
- [ ] Can swipe/click left-right for date navigation
- [ ] Muscle groups displayed: Chest, Back, Shoulders, Biceps, Triceps, Abs, Legs
- [ ] User can select 1-2 muscle groups
- [ ] Next button enabled only with 1-2 selections
- [ ] Responsive on mobile

Time Estimate: 2 hours
```

**Sub-tasks:**
```
- [ ] Create pages/Home.jsx
- [ ] Create DateBanner component
- [ ] Create MuscleGroupSelector component
- [ ] Add date navigation logic (previous/next day)
- [ ] Implement multi-select logic (max 2)
- [ ] Add Tailwind styling
- [ ] Make responsive for mobile
- [ ] Test date navigation
- [ ] Test muscle group selection
```

---

### Issue 3.2: Create Date Navigation Logic
**Title:** `[TASK] Implement Date Navigation`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Implement date navigation that:
- Shows current + adjacent dates
- Allows clicking/swiping
- Updates selected date state
- Formats dates properly

Acceptance Criteria:
- [ ] Previous/next buttons work
- [ ] Date format: "Mon, Jul 30"
- [ ] Today has different styling
- [ ] Clicking date updates page view
- [ ] No date boundary issues

Time Estimate: 1 hour
```

**Sub-tasks:**
```
- [ ] Write date utility functions
- [ ] Handle date state in Redux or React Context
- [ ] Add left/right navigation handlers
- [ ] Format date display
- [ ] Highlight today
- [ ] Test date arithmetic (edge cases: month end, year boundary)
```

---

### Issue 3.3: Add Muscle Group Selection State Management
**Title:** `[TASK] Setup Muscle Group Selection State`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Manage selected muscle groups in Redux:

Acceptance Criteria:
- [ ] Redux slice for workoutState created
- [ ] selectedMuscleGroups tracked
- [ ] Can select/deselect (toggle)
- [ ] Max 2 selection enforced
- [ ] State persists across navigation

Time Estimate: 45 minutes
```

**Sub-tasks:**
```
- [ ] Create store/slices/workoutSlice.js
- [ ] Add reducers: setMuscleGroups, toggleMuscleGroup
- [ ] Connect component to Redux
- [ ] Test multi-select logic
```

---

## Epic 4: Exercise Variations & AI Feedback

**Epic Title:** `[EPIC] Exercise Variations & AI Feedback System`  
**Label:** `type:epic`, `priority:p0`, `component:frontend`, `component:backend`, `component:ai`  
**Milestone:** MVP Phase 1  

---

### Issue 4.1: Get Exercises by Muscle Group API
**Title:** `[TASK] Create GET /exercises API Endpoint`  
**Label:** `type:feature`, `priority:p0`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build endpoint that:
- Accepts query params: ?muscle_groups=shoulders,triceps
- Returns exercises sorted by effectiveness_rank
- Groups by muscle group

Acceptance Criteria:
- [ ] GET /exercises?muscle_groups=shoulders,triceps returns exercises
- [ ] Sorted by effectiveness_rank (1 = best)
- [ ] Includes: id, name, type, effectiveness_rank, equipment, form_instructions
- [ ] Tested with Postman

Time Estimate: 1 hour
```

**Sub-tasks:**
```
- [ ] Write query in controllers/exerciseController.js
- [ ] Create routes/exercises.js
- [ ] Test with Postman
- [ ] Verify sorting by rank
```

---

### Issue 4.2: Create Exercise Variations Screen (UI)
**Title:** `[TASK] Create Exercise Variations Selection UI`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build screen to:
- Display exercises grouped by muscle
- Show effectiveness rank
- Allow multi-select of exercises
- Show "Next" button

Acceptance Criteria:
- [ ] Fetches exercises from backend
- [ ] Grouped by muscle group
- [ ] Sorted by effectiveness
- [ ] User can select/deselect
- [ ] At least 1 exercise required to proceed
- [ ] Loading state shown while fetching

Time Estimate: 2 hours
```

**Sub-tasks:**
```
- [ ] Create pages/ExerciseVariations.jsx
- [ ] Fetch exercises from GET /exercises endpoint
- [ ] Display exercises grouped by muscle
- [ ] Implement checkbox selection
- [ ] Add loading/error states
- [ ] Add Tailwind styling
- [ ] Connect to Redux state
- [ ] Test selection logic
```

---

### Issue 4.3: Build AI Feedback API Endpoint (Claude Integration)
**Title:** `[TASK] Create POST /ai/feedback Endpoint with Claude API`  
**Label:** `type:feature`, `priority:p0`, `component:backend`, `component:ai`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build endpoint that:
- Accepts selected exercises + fitness level
- Calls Claude API for feedback
- Returns rating (great/good/can_improve) + explanation
- Logs feedback to DB

Acceptance Criteria:
- [ ] POST /ai/feedback accepts selected exercises & fitness level
- [ ] Calls Claude API with proper prompt
- [ ] Returns feedback object with rating & text
- [ ] Feedback saved to ai_feedback_logs table
- [ ] Beginner-specific rules applied
- [ ] Error handling if Claude API fails

Time Estimate: 2.5 hours
```

**Sub-tasks:**
```
- [ ] Sign up for Anthropic API (get $5 free credits)
- [ ] Install @anthropic-ai/sdk: npm install @anthropic-ai/sdk
- [ ] Store ANTHROPIC_API_KEY in .env
- [ ] Write Claude prompt for workout feedback
- [ ] Implement API call in controller
- [ ] Parse Claude response
- [ ] Map response to rating: great/good/can_improve
- [ ] Save feedback to DB
- [ ] Test with Postman (various exercise selections)
- [ ] Test beginner vs non-beginner responses
```

---

### Issue 4.4: Create AI Feedback Display Screen (UI)
**Title:** `[TASK] Create AI Feedback Display UI`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build screen to:
- Call /ai/feedback endpoint
- Display feedback with rating badge
- Show explanation text
- Allow "Accept" or "Adjust Selection"

Acceptance Criteria:
- [ ] Calls POST /ai/feedback
- [ ] Shows loading state
- [ ] Displays rating: Great/Good/Can Improve with colors
- [ ] Shows 2-3 sentence explanation
- [ ] "Accept" button proceeds to Exercise Ordering
- [ ] "Adjust" button goes back to Variations

Time Estimate: 1.5 hours
```

**Sub-tasks:**
```
- [ ] Create pages/AIFeedback.jsx
- [ ] Add useEffect to call /ai/feedback API
- [ ] Create Feedback component with rating badge
- [ ] Style by rating (green, yellow, orange)
- [ ] Add Accept/Adjust buttons
- [ ] Handle errors gracefully
- [ ] Test end-to-end
```

---

### Issue 4.5: Build AI Exercise Ordering API
**Title:** `[TASK] Create POST /ai/order Endpoint`  
**Label:** `type:feature`, `priority:p0`, `component:backend`, `component:ai`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build endpoint that:
- Accepts selected exercises
- Calls Claude to order by proper progression
- Returns ordered list with reasoning

Acceptance Criteria:
- [ ] POST /ai/order accepts exercise list
- [ ] Returns: compound exercises first, isolation last
- [ ] Each exercise has position + reasoning
- [ ] Tested with various exercise combos

Time Estimate: 2 hours
```

**Sub-tasks:**
```
- [ ] Write Claude prompt for exercise ordering
- [ ] Implement API call
- [ ] Parse response into ordered array
- [ ] Assign compound/isolation labels
- [ ] Add reasoning for each position
- [ ] Test with Postman
- [ ] Verify compound-first rule
```

---

### Issue 4.6: Create Exercise Ordering Screen (UI)
**Title:** `[TASK] Create Exercise Ordering & Reordering UI`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build screen to:
- Display AI's recommended order
- Allow drag-and-drop reordering
- Show exercise type (compound/isolation)
- Proceed to logging on confirm

Acceptance Criteria:
- [ ] Fetches ordered exercises from /ai/order
- [ ] Shows as numbered list
- [ ] Drag-and-drop reordering works
- [ ] Visual diff for compound vs isolation
- [ ] "Confirm Order" locks in and proceeds
- [ ] Reordered list saved to Redux state

Time Estimate: 2.5 hours
```

**Sub-tasks:**
```
- [ ] Create pages/ExerciseOrdering.jsx
- [ ] Call POST /ai/order endpoint
- [ ] Display exercises with position numbers
- [ ] Implement drag-and-drop (use react-beautiful-dnd or native HTML5)
- [ ] Show compound/isolation indicator
- [ ] Add reasoning text for each
- [ ] Handle reordering state
- [ ] Save final order to Redux
- [ ] Add Tailwind styling
- [ ] Test drag-and-drop
```

---

## Epic 5: Exercise Logging

**Epic Title:** `[EPIC] Exercise Logging & Workout Completion`  
**Label:** `type:epic`, `priority:p0`, `component:frontend`, `component:backend`  
**Milestone:** MVP Phase 1  

---

### Issue 5.1: Create Exercise Logging API
**Title:** `[TASK] Create POST /workouts/log Endpoint`  
**Label:** `type:feature`, `priority:p0`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build endpoint to save exercise logs:

Acceptance Criteria:
- [ ] POST /workouts/log accepts: exercise_id, date, weight, reps, sets
- [ ] Returns 201 on success with log entry
- [ ] Saves to workout_logs table
- [ ] Only logged-in user can save their data
- [ ] Validates weight/reps/sets are positive numbers

Time Estimate: 1 hour
```

**Sub-tasks:**
```
- [ ] Create routes/workouts.js
- [ ] Implement POST /workouts/log controller
- [ ] Add input validation
- [ ] Save to workout_logs table
- [ ] Add JWT auth middleware
- [ ] Test with Postman
```

---

### Issue 5.2: Create Exercise Form Instructions API
**Title:** `[TASK] Create GET /exercises/:id/form Endpoint`  
**Label:** `type:feature`, `priority:p0`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build endpoint to fetch form instructions:

Acceptance Criteria:
- [ ] GET /exercises/:id/form returns form instructions
- [ ] Includes: exercise_id, name, form_instructions, image_url
- [ ] 2-3 sentence instructions pre-stored in DB

Time Estimate: 30 minutes
```

**Sub-tasks:**
```
- [ ] Create controller method to fetch exercise details
- [ ] Route to /exercises/:id/form
- [ ] Test with Postman
```

---

### Issue 5.3: Create Exercise Logging UI
**Title:** `[TASK] Create Exercise Logging Form Component`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build form to log exercises:

Acceptance Criteria:
- [ ] Shows exercise name & form instructions
- [ ] Input fields: weight, reps, sets
- [ ] Weight unit selector (lbs/kg)
- [ ] "Log" button saves to backend
- [ ] "Skip" button allows skipping exercise
- [ ] Loading state while saving
- [ ] Confirmation message
- [ ] "Next Exercise" button to proceed

Time Estimate: 2.5 hours
```

**Sub-tasks:**
```
- [ ] Create pages/ExerciseLogging.jsx
- [ ] Create ExerciseLogForm component
- [ ] Fetch form instructions from GET /exercises/:id/form
- [ ] Add input fields: weight, reps, sets, weight_unit
- [ ] Implement form submission (POST /workouts/log)
- [ ] Handle loading/error states
- [ ] Show confirmation after each log
- [ ] Add next exercise navigation
- [ ] Add skip functionality
- [ ] Style with Tailwind
- [ ] Test end-to-end logging
```

---

### Issue 5.4: Create Workout Completion Screen
**Title:** `[TASK] Create Workout Complete Confirmation UI`  
**Label:** `type:feature`, `priority:p0`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Build completion screen:

Acceptance Criteria:
- [ ] Shows "Workout Complete!" message
- [ ] Displays summary (exercise count, time)
- [ ] "View Workout History" button (redirects to home, same date)
- [ ] "Start New Workout" button (redirects to home, today)
- [ ] Motivational message

Time Estimate: 1 hour
```

**Sub-tasks:**
```
- [ ] Create pages/WorkoutComplete.jsx
- [ ] Add completion message & animation
- [ ] Calculate and display exercise count
- [ ] Add motivational message
- [ ] Create buttons with navigation
- [ ] Style with Tailwind
- [ ] Test routing
```

---

## Epic 6: Deployment & Documentation

**Epic Title:** `[EPIC] Deployment & Final Documentation`  
**Label:** `type:epic`, `priority:p1`, `component:frontend`, `component:backend`  
**Milestone:** MVP Phase 1  

---

### Issue 6.1: Deploy Frontend to Vercel
**Title:** `[TASK] Deploy React App to Vercel`  
**Label:** `type:feature`, `priority:p1`, `component:frontend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Deploy frontend:

Acceptance Criteria:
- [ ] App deployed to Vercel
- [ ] Environment variables configured
- [ ] Works against production backend
- [ ] HTTPS enabled
- [ ] Custom domain optional but nice-to-have

Time Estimate: 1 hour
```

**Sub-tasks:**
```
- [ ] Push to GitHub
- [ ] Sign up for Vercel
- [ ] Connect GitHub repo to Vercel
- [ ] Set environment variables (VITE_BACKEND_URL)
- [ ] Deploy
- [ ] Test deployed app
- [ ] Verify API calls working
```

---

### Issue 6.2: Deploy Backend to Railway
**Title:** `[TASK] Deploy Node Backend to Railway`  
**Label:** `type:feature`, `priority:p1`, `component:backend`  
**Milestone:** MVP Phase 1  

**Description:**
```
Deploy backend:

Acceptance Criteria:
- [ ] Backend deployed to Railway
- [ ] Database connected
- [ ] Environment variables configured
- [ ] API endpoints working in production
- [ ] CORS configured for Vercel domain

Time Estimate: 1 hour
```

**Sub-tasks:**
```
- [ ] Push to GitHub
- [ ] Sign up for Railway
- [ ] Connect GitHub repo
- [ ] Set environment variables
- [ ] Link to Supabase database
- [ ] Deploy
- [ ] Test endpoints with curl/Postman
- [ ] Update frontend API URL
```

---

### Issue 6.3: Create Comprehensive README
**Title:** `[TASK] Write Project README & Setup Docs`  
**Label:** `type:documentation`, `priority:p1`  
**Milestone:** MVP Phase 1  

**Description:**
```
Document project:

Acceptance Criteria:
- [ ] README explains project purpose
- [ ] Local setup instructions (both frontend & backend)
- [ ] Environment variables documented
- [ ] API endpoint reference
- [ ] Deployment instructions
- [ ] How to seed database
- [ ] Screenshots of key screens
- [ ] Known limitations/future work

Time Estimate: 2 hours
```

**Sub-tasks:**
```
- [ ] Write project overview section
- [ ] Write local setup (step-by-step)
- [ ] Document env variables
- [ ] Add API endpoint list with examples
- [ ] Add database setup instructions
- [ ] Document Supabase setup
- [ ] Add deployment guide
- [ ] Add screenshots (key screens)
- [ ] Add Future Work section
- [ ] Add Contributing guidelines
```

---

### Issue 6.4: Testing & Bug Fixes
**Title:** `[TASK] End-to-End Testing & Bug Fixes`  
**Label:** `type:feature`, `priority:p1`  
**Milestone:** MVP Phase 1  

**Description:**
```
Test full workflow:

Acceptance Criteria:
- [ ] Can sign up and create account
- [ ] Can log in with valid credentials
- [ ] Can select muscle groups
- [ ] Can receive AI feedback
- [ ] Can reorder exercises
- [ ] Can log full workout
- [ ] Can see completion screen
- [ ] No console errors
- [ ] Responsive on mobile
- [ ] Works in Chrome, Firefox, Safari

Time Estimate: 3 hours
```

**Sub-tasks:**
```
- [ ] Full signup flow test
- [ ] Full login flow test
- [ ] Muscle selection edge cases
- [ ] Exercise selection validation
- [ ] AI feedback accuracy check
- [ ] Exercise reordering test
- [ ] Logging validation (negative numbers, etc)
- [ ] Mobile responsiveness check
- [ ] Cross-browser testing
- [ ] Fix bugs found
- [ ] Verify no console errors
```

---

## Priority & Effort Summary

| Epic | Priority | Effort | Status |
|------|----------|--------|--------|
| Project Setup | P0 | 5h | Planned |
| Authentication | P0 | 7.5h | Planned |
| Home Screen | P0 | 4.5h | Planned |
| Exercise Variations & AI | P0 | 11.5h | Planned |
| Exercise Logging | P0 | 6.5h | Planned |
| Deployment & Docs | P1 | 8h | Planned |
| **TOTAL** | | **~43 hours** | |

**Estimate:** 1-1.5 weeks for full-time development  
**Realistic:** 2-3 weeks with 10-15 hrs/week

---

## How to Use This Template

### Step 1: Create GitHub Project
1. Go to repo → Projects tab
2. Create new project (Kanban view recommended)
3. Add columns: Backlog, In Progress, In Review, Done

### Step 2: Create Issues
1. Go to Issues
2. For each Epic, create a new issue with the template above
3. Add appropriate labels (type, priority, component)
4. Set milestone: MVP Phase 1

### Step 3: Create Sub-tasks
1. In each issue, use "Add a task list" (markdown)
2. Add sub-task checkboxes
3. Check them off as you complete

### Step 4: Track Progress
1. Move cards across Kanban columns
2. Update estimates as you work
3. Link pull requests to issues
4. Close issues when done (auto-closes linked PRs)

### Step 5: Weekly Review
Every Friday:
- Check % of Phase 1 complete
- Identify blockers
- Adjust timeline if needed
- Plan next week's priorities

---

**Template Version:** 1.0  
**Last Updated:** July 30, 2026
