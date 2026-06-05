# Nihongo Kerja — Japan Recruitment Platform

A dual-sided recruitment management app for Indonesian workers going to Japan.

---

## Features

### Student side
- **Login / Registration** — name, contact details, DOB, blood type, medical notes
- **Progress timeline** — 11-stage pipeline with live status per stage
- **Interview simulation** — 8 Japanese workplace questions, AI-scored via Claude API
- **Document uploads** — upload PDFs/images for each parallel stage (4–7)
- **Profile** — personal info, performance scores, document checklist

### Admin / Tester side
- **Student list** — overview of all students with progress bars
- **Stage management** — unlock, mark done, revert, add notes and scores per student
- **File viewer** — see documents uploaded by students
- **Pipeline overview** — bar chart of completion across all students

---

## Stage pipeline

| # | Stage | Type |
|---|-------|------|
| 1 | Interview Simulation | Sequential |
| 2 | Actual Interview | Sequential (unlocks after 1) |
| 3 | Working Contract Approval | Sequential (unlocks after 2) |
| 4 | Contract Issuing | **Parallel** (unlocks after 3) |
| 5 | Additional Documents | **Parallel** (unlocks after 3) |
| 6 | MCU Medical Test | **Parallel** (unlocks after 3) |
| 7 | Passport | **Parallel** (unlocks after 3) |
| 8 | COE (Work Authorization) | Gate (locked until 4–7 all done) |
| 9 | e-KTKLN Issuing | Gate |
| 10 | Visa | Gate |
| 11 | Dispatchment | Gate |

---

## Getting started

### Prerequisites
- Node.js 16+
- npm or yarn

### Install and run
```bash
npm install
npm start
```

The app opens at `http://localhost:3000`.

### Demo credentials
| Role | Email | Password |
|------|-------|----------|
| Admin | admin@nihongo.id | admin2024 |
| Student (demo 1) | budi@email.com | pass123 |
| Student (demo 2) | siti@email.com | pass123 |

---

## Project structure

```
src/
├── App.js                    # Root — auth state, DB, routing
├── index.js
├── styles.css                # All global styles
├── data/
│   ├── stages.js             # Stage definitions, parallel keys, document specs
│   ├── questions.js          # Interview questions (JP/romaji/EN)
│   └── mockDb.js             # Demo students and admin account
├── utils/
│   ├── stageLogic.js         # canUnlockStage, unlockNext, getOverallProgress
│   └── claudeApi.js          # evaluateInterviewAnswer (Claude API call)
└── components/
    ├── LoginScreen.js         # Sign-in + registration form
    ├── AdminScreen.js         # Admin dashboard + student detail
    ├── StudentScreen.js       # Tab shell for student views
    ├── StudentProgress.js     # Timeline view
    ├── InterviewSim.js        # Interview simulation flow
    ├── DocumentUploads.js     # File upload UI for parallel stages
    └── StudentProfile.js      # Personal info + checklist
```

---

## Notes

- **Database**: in-memory React state (no backend). Replace `INITIAL_STUDENTS` in `mockDb.js` and the state in `App.js` with real API calls to persist data.
- **Claude API**: the interview evaluator calls `api.anthropic.com` directly. In production, proxy this through your own backend to keep API keys secure.
- **File uploads**: files are stored as base64 `dataUrl` in state (in-memory). For production use cloud storage (S3, GCS, etc.) and store only the URL.
- **Mobile**: the app is designed for 430px max-width and works in any mobile browser. For true native apps, migrate to React Native with these same components.
