# StudyMind AI — System Documentation

**Version:** 1.0 MVP  
**Type:** Web-based AI Study Coordinator  
**Stack:** HTML · CSS · Vanilla JS · Claude API (Anthropic)

---

## 1. Project Overview

StudyMind AI is a web-based intelligent study assistant designed to help students learn more effectively through four specialized interaction modes: tutoring, quizzing, study planning, and concept explanation. It is powered by Anthropic's Claude AI and requires no backend — the entire application runs in a single HTML file.

### Target Users
- Secondary school and university students
- Self-learners preparing for exams
- Anyone who needs a personal, always-available tutor

---

## 2. Core Features (MVP)

| Feature | Description |
|---|---|
| **Tutor Mode** | Step-by-step explanations, concept walkthroughs, problem solving |
| **Quiz Mode** | AI-generated multiple-choice and short-answer questions with feedback |
| **Planner Mode** | Personalized study schedules based on subjects and exam dates |
| **Explainer Mode** | Simplified breakdowns using analogies and real-world examples |
| **Subject Focus** | Pin conversations to a specific subject (Maths, Physics, Biology, etc.) |
| **Quick Actions** | One-click prompts for session summaries, practice questions, key takeaways |
| **Session Stats** | Live tracking of message count, active mode, subject, and session duration |
| **Conversation Memory** | Full chat history sent with each request for contextual responses |

---

## 3. System Architecture

```
┌─────────────────────────────────────────┐
│              Browser (Client)           │
│                                         │
│  ┌─────────┐  ┌──────────┐  ┌────────┐ │
│  │ Sidebar │  │   Chat   │  │ Input  │ │
│  │ (modes, │  │  Window  │  │  Bar   │ │
│  │subjects)│  │(messages)│  │        │ │
│  └─────────┘  └──────────┘  └────────┘ │
│                    │                    │
│              JS State Manager           │
│           (history, mode, subject)      │
│                    │                    │
└────────────────────┼────────────────────┘
                     │ HTTPS POST
                     ▼
          ┌─────────────────────┐
          │   Anthropic API     │
          │  /v1/messages       │
          │  claude-sonnet-4    │
          └─────────────────────┘
```

### How it works
1. User selects a mode and optional subject from the sidebar
2. User types a message or clicks a quick action
3. The app builds a request: system prompt (based on mode) + full conversation history + new message
4. Request sent to Anthropic's Claude API
5. Response rendered as formatted markdown in the chat window
6. Response added to history for context in future turns

---

## 4. File Structure

```
studybot/
└── index.html        ← Entire application (HTML + CSS + JS in one file)
```

No build tools, no dependencies, no backend required.

---

## 5. Setup & Running

### Prerequisites
- A modern web browser (Chrome, Firefox, Edge, Safari)
- An Anthropic API key — get one at https://console.anthropic.com

### Running locally
```bash
# Option 1: Open directly
open index.html

# Option 2: Serve locally (recommended to avoid CORS issues)
python3 -m http.server 8080
# Then visit http://localhost:8080
```

### API Key
The app calls the Anthropic API directly from the browser. For the MVP, the API key is handled by the Anthropic proxy included in the artifact environment. For production deployment, you will need to route requests through your own backend to protect the key.

---

## 6. Modes — Detailed Behaviour

### 📚 Tutor Mode
The default mode. Claude acts as a patient, adaptive tutor.
- Explains concepts with examples and analogies
- Walks through problems step by step
- Checks for understanding at the end of each explanation
- Adapts vocabulary to student level

### ❓ Quiz Mode
Tests the student's knowledge interactively.
- Generates one question at a time by default
- Supports multiple-choice (A/B/C/D) and short-answer formats
- Gives detailed feedback after each answer
- Tracks performance across the session

### 📅 Planner Mode
Creates structured study plans.
- Builds daily/weekly revision schedules
- Factors in subjects, difficulty, and available time
- Includes breaks and practice test slots
- Outputs plans as formatted tables

### 💡 Explainer Mode
Simplifies complex concepts.
- Follows the structure: What it is → Why it matters → How it works → Example
- Always includes a real-world analogy
- Introduces technical terms after plain-language explanation
- Ends with a memorable one-line summary

---

## 7. Upgrade Roadmap

### v1.1 — Persistence
- Save chat history to localStorage
- Resume previous sessions
- Export conversations as PDF or text

### v1.2 — User Accounts
- Flask/Node.js backend with user login
- PostgreSQL database for storing sessions and progress
- Dashboard showing study streaks and topics covered

### v2.0 — Production Features
- File upload: student uploads notes/textbooks for AI to study from
- Flashcard generation from conversation
- Progress tracking with charts
- Exam countdown timer with smart reminders
- Multiple language support

### v2.5 — Monetisation Ready
- Subscription tiers (Free: 20 messages/day, Pro: unlimited)
- School/institution licensing
- Teacher dashboard to monitor student progress
- Custom branding (white-label)

### v3.0 — Full Platform
- Mobile app (React Native)
- Voice interaction (speech-to-text + text-to-speech)
- Collaborative study rooms
- Integration with Google Classroom / Moodle

---

## 8. Tech Stack Details

| Layer | Technology | Purpose |
|---|---|---|
| UI | HTML5 + CSS3 | Structure and styling |
| Logic | Vanilla JavaScript | State management, API calls, rendering |
| AI | Anthropic Claude Sonnet 4 | All intelligence and responses |
| Fonts | Google Fonts | Plus Jakarta Sans + Fira Code |
| Hosting (future) | Vercel / Netlify / Railway | Static or full-stack deployment |

---

## 9. API Usage

Each message sends:
```json
{
  "model": "claude-sonnet-4-20250514",
  "max_tokens": 1000,
  "system": "<mode-specific system prompt>",
  "messages": [
    { "role": "user", "content": "..." },
    { "role": "assistant", "content": "..." },
    ...full history...
  ]
}
```

The system prompt changes dynamically based on the selected mode and subject, giving Claude a different personality and set of instructions for each mode.

---

## 10. Known Limitations (MVP)

| Limitation | Future Fix |
|---|---|
| No data persistence (chat lost on refresh) | Add localStorage or backend DB |
| API key exposed in browser | Route through backend proxy |
| No user accounts | Add auth (Flask-Login / Supabase) |
| No file/image upload | Add document parsing in v2 |
| English only | Add i18n support in v2.5 |

---

## 11. Potential Commercial Value

StudyMind AI targets a large, growing market:

- The global e-learning market is projected to exceed $400B by 2026
- AI tutoring tools are among the fastest-growing EdTech categories
- Schools and universities actively seek affordable AI-powered study tools
- Individual students pay $20–$50/month for tutoring apps (Chegg, Quizlet, Khan Academy)

**Monetisation paths:**
- B2C subscription (students pay monthly)
- B2B licensing to schools and tutoring centres
- Freemium with premium mode unlocks
- White-label solution sold to EdTech companies
