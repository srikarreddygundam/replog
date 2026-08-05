# RepLog - GitHub Repository Setup Guide

**Goal:** Push all design documents to GitHub so you have version control + portfolio showcase

---

## Step 1: Create GitHub Repository

### 1.1 Go to GitHub
- Open https://github.com
- Sign in with your account
- Click **"+"** icon (top right) → **"New repository"**

### 1.2 Repository Settings
Fill in these fields:

| Field | Value |
|-------|-------|
| Repository name | `replog` or `replog-app` |
| Description | "AI-powered workout logging app with intelligent exercise selection" |
| Visibility | **Public** (shows on your portfolio) |
| Initialize with README | ✅ Check this |
| .gitignore | Select **Node** (handles node_modules) |
| License | **MIT** (good for portfolios) |

### 1.3 Create Repository
Click **"Create repository"** button.

**You'll see:**
```
https://github.com/YOUR_USERNAME/replog
```

Copy this URL — you'll need it soon.

---

## Step 2: Clone Repository to Your Computer

### 2.1 Open Terminal/Command Prompt

Navigate to your Projects folder:
```bash
cd /Users/srikarreddygundam/Projects
```

(This is where you already are based on your `pwd` output)

### 2.2 Clone the Repository

Replace `YOUR_USERNAME` with your actual GitHub username:

```bash
git clone https://github.com/YOUR_USERNAME/replog.git
```

**What this does:**
- Downloads the empty repo from GitHub
- Creates a `replog/` folder on your computer
- Sets up git to track changes

**You'll see:**
```
Cloning into 'replog'...
remote: Enumerating objects: 3, done.
...
```

### 2.3 Navigate into the folder

```bash
cd replog
```

---

## Step 3: Add Design Documents

### 3.1 Copy the design documents

You have 3 documents that need to go into your repo:

1. `GYM_APP_DESIGN_DOC.md` → rename to `DESIGN_DOC.md`
2. `GITHUB_PROJECT_TEMPLATE.md` → keep as is
3. `DESIGN_REVIEW_CHANGES.md` → keep as is

**Copy these 3 files** from `/home/claude/` to your `/Users/srikarreddygundam/Projects/replog/` folder.

Or use command line:
```bash
# From inside replog folder:
cp ~/DESIGN_DOC.md .
cp ~/GITHUB_PROJECT_TEMPLATE.md .
cp ~/DESIGN_REVIEW_CHANGES.md .
```

(Adjust paths if needed)

### 3.2 Verify files are there

```bash
ls -la
```

You should see:
```
README.md
DESIGN_DOC.md
GITHUB_PROJECT_TEMPLATE.md
DESIGN_REVIEW_CHANGES.md
.gitignore
.git/
```

---

## Step 4: Update README.md

### 4.1 Edit README.md

Open `README.md` in your code editor and replace all content with:

```markdown
# RepLog

**An AI-powered workout logging app that provides intelligent exercise selection, real-time feedback, and progress tracking.**

## 📋 What is RepLog?

RepLog combines:
- 💪 **Smart Workout Logging** - Log exercises with weight, reps, sets
- 🤖 **AI Feedback** - Powered by Claude API for intelligent workout analysis
- 📊 **Exercise Selection** - AI recommends exercise order (compound → isolation)
- 📅 **Workout History** - Track workouts by date
- 📈 **Progress Analytics** - See weight progression over time (Phase 2)

## 🎯 Tech Stack

- **Frontend:** React 18 + Vite + Redux + Tailwind CSS
- **Backend:** Node.js + Express
- **Database:** Supabase (PostgreSQL)
- **AI:** Anthropic Claude API
- **Deployment:** Vercel (frontend) + Railway (backend)
- **Cost:** 100% Free tier (forever)

## 📚 Documentation

- **[DESIGN_DOC.md](DESIGN_DOC.md)** - Complete product requirements, system architecture, API endpoints
- **[GITHUB_PROJECT_TEMPLATE.md](GITHUB_PROJECT_TEMPLATE.md)** - Development roadmap with 24 issues & sprint planning
- **[DESIGN_REVIEW_CHANGES.md](DESIGN_REVIEW_CHANGES.md)** - Design review feedback & changes

## 🚀 Development Status

**Phase 1 (MVP):** In Development
- User authentication (signup/login)
- Workout logging workflow
- AI feedback system
- Exercise ordering
- Deployment ready

**Phase 2:** Planned
- Workout history & calendar view
- Progress charts
- Body metrics tracking

**Phase 3:** Future
- Mobile native app
- Social features
- Advanced analytics

## 📖 How to Use These Docs

1. **Start here:** [DESIGN_DOC.md](DESIGN_DOC.md)
   - Product vision, user stories, system architecture
   - Complete database schema & API specifications
   - Screen designs & flow diagrams

2. **For development:** [GITHUB_PROJECT_TEMPLATE.md](GITHUB_PROJECT_TEMPLATE.md)
   - 6 Epics with detailed tasks & sub-tasks
   - Time estimates (50-60 hours for Phase 1)
   - GitHub project board setup guide

3. **Design decisions:** [DESIGN_REVIEW_CHANGES.md](DESIGN_REVIEW_CHANGES.md)
   - Summary of design review feedback
   - All approved changes with rationale
   - Technology choices explained

## 🛠️ Getting Started

```bash
# Frontend setup
npm create vite@latest replog-frontend -- --template react
cd replog-frontend
npm install

# Backend setup
mkdir replog-backend
cd replog-backend
npm init -y
npm install express dotenv cors pg
```

Detailed setup instructions coming in DESIGN_DOC.md.

## 👤 Author

Built as a portfolio project combining:
- Full-stack development (React + Node.js)
- Database design (PostgreSQL)
- AI integration (Claude API)
- System architecture & design

## 📝 License

MIT - Feel free to use, modify, and learn from this code.

---

**Status:** Design & Architecture Complete ✅  
**Next:** Phase 1 Development (Task 1: Project Setup)
```

### 4.2 Save the file

Save `README.md` in your editor.

---

## Step 5: Commit & Push to GitHub

### 5.1 Check what's changed

```bash
git status
```

You should see something like:
```
On branch main

Changes not staged for commit:
  modified:   README.md
  
Untracked files:
  DESIGN_DOC.md
  GITHUB_PROJECT_TEMPLATE.md
  DESIGN_REVIEW_CHANGES.md
```

### 5.2 Add all files to staging

```bash
git add .
```

This tells git "I want to commit all these changes."

### 5.3 Create a commit

```bash
git commit -m "Add design docs and project structure for RepLog

- Added DESIGN_DOC.md (product requirements & system design)
- Added GITHUB_PROJECT_TEMPLATE.md (development roadmap)
- Added DESIGN_REVIEW_CHANGES.md (design review feedback)
- Updated README with project overview"
```

**What you'll see:**
```
[main 1a2b3c4] Add design docs and project structure for RepLog
 4 files changed, 5000 insertions(+)
 create mode 100644 DESIGN_DOC.md
 create mode 100644 GITHUB_PROJECT_TEMPLATE.md
 create mode 100644 DESIGN_REVIEW_CHANGES.md
```

### 5.4 Push to GitHub

```bash
git push origin main
```

**What you'll see:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 1.23 KiB | 1.23 MiB/s, done.
...
To github.com:YOUR_USERNAME/replog.git
   abc123..1a2b3c4  main -> main
```

---

## ✅ Verify on GitHub

### 6.1 Check GitHub.com

- Go to https://github.com/YOUR_USERNAME/replog
- You should see:
  - ✅ README.md with project description
  - ✅ DESIGN_DOC.md
  - ✅ GITHUB_PROJECT_TEMPLATE.md
  - ✅ DESIGN_REVIEW_CHANGES.md

### 6.2 Your repo is now public!

Anyone can see your design work, architecture decisions, and planning — **excellent for interviews!**

---

## 📋 Next Steps

After pushing to GitHub:

1. ✅ Design docs are version-controlled
2. ✅ Your portfolio shows serious planning & architecture skills
3. 🚀 Ready to start **Sub-Task 1.1: Create React Frontend**

---

## Troubleshooting

### "fatal: not a git repository"
**Solution:** Make sure you're inside the `replog` folder:
```bash
cd /Users/srikarreddygundam/Projects/replog
git status
```

### "Permission denied" when pushing
**Solution:** GitHub uses SSH or HTTPS. Follow GitHub's guide:
- Go to GitHub Settings → SSH keys (if using SSH)
- Or use HTTPS with personal access token

### "Authentication failed"
**Solution:** Use a GitHub Personal Access Token:
1. GitHub Settings → Developer Settings → Personal Access Tokens
2. Create new token with `repo` scope
3. Use token as password when git asks

---

**You're almost there!** After pushing these docs, we move to Sub-Task 1.1! 🚀
