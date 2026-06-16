# 🤖 Automation Portfolio

> Real-world automation workflows built with n8n, REST APIs, and JavaScript.
> Built by **Elkay** — Cybersecurity Student & Automation Engineer.

---

## 📁 Projects

| # | Project | Tools | Status |
|---|---------|-------|--------|
| 01 | [Event Registration & Analytics System](#-project-1--event-registration--attendance-analytics-system) | n8n, REST API, JavaScript | ✅ Complete |
| 02 | [Daily Weather Alert Bot](#-project-2--daily-weather-alert-bot) | n8n, OpenWeatherMap, Gmail, Telegram | ✅ Complete |
| 03 | [Smart Applicant Response System](#-project-3--smart-applicant-response-system) | n8n, Google Sheets, Gmail, Switch Node | ✅ Complete |
| 04 | Form → Email Pipeline | n8n, Webhooks, Google Sheets | 🔨 Coming Soon |

---

## 📌 Project 1 — Event Registration & Attendance Analytics System

### Problem
Organizations running multiple events struggle with manual participant processing, duplicate registrations, and generating attendance reports. This takes hours of manual effort per event.

### Solution
A fully automated n8n workflow that handles the entire registration lifecycle — from data retrieval to report generation — with zero manual effort.

### Workflow Architecture
```
HTTP Request (Fetch 100 participants)
      ↓
Code Node (Transform + Assign Events + Inject Duplicates)
      ↓
Aggregate (Collect all records)
      ↓
Code Node (Detect & Flag Duplicates)
      ↓
Code Node (Simulate Attendance)
      ↓
Code Node (Build Analytics)
      ↓
Code Node (Final Report)
```

### Key Features
- Fetches 100 real participant records from a live REST API
- Round-robin event assignment across 3 events (AI Workshop, Career Development Seminar, Startup Innovation Summit)
- Duplicate detection using `email + event` composite key — same person, different events is allowed
- Attendance simulation with 70% probability model (realistic industry benchmark)
- Auto-generated analytics report including:
  - Total vs valid vs duplicate registrations
  - Per-event attendance breakdown
  - No-show rates
  - Event leaderboard ranked by attendance rate

### Design Decisions
| Decision | Justification |
|----------|--------------|
| Round-robin assignment | Ensures even, deterministic distribution across all events |
| Flag duplicates (not delete) | Preserves audit trail for fraud detection and debugging |
| 70% attendance rate | Industry benchmark for professional events |
| email + event duplicate key | Same person can attend different events legitimately |

### Sample Report Output
```json
{
  "reportTitle": "Event Registration & Attendance Analytics Report",
  "registrationStatistics": {
    "totalRegistrations": 115,
    "validRegistrations": 100,
    "duplicateRegistrations": 15
  },
  "attendanceStatistics": {
    "totalAttended": 71,
    "totalAbsent": 29,
    "attendanceRate": "71.0%",
    "noShowRate": "29.0%"
  },
  "leaderboard": [
    { "rank": 1, "event": "Startup Innovation Summit", "attendanceRate": "75.8%" },
    { "rank": 2, "event": "AI Workshop", "attendanceRate": "70.6%" },
    { "rank": 3, "event": "Career Development Seminar", "attendanceRate": "66.7%" }
  ]
}
```

### n8n Concepts Demonstrated
✅ HTTP Request · ✅ Split Out · ✅ Merge · ✅ Aggregate · ✅ Looping · ✅ Conditional Logic · ✅ Data Transformation

---

## 🌤️ Project 2 — Daily Weather Alert Bot

### Problem
Manually checking weather every morning for multiple cities is repetitive and easy to forget — especially before leaving the house.

### Solution
A fully automated weather bot that fetches live weather data for Kaduna and Zaria every morning at 7AM and delivers formatted alerts via Gmail and Telegram to multiple recipients — no human effort required after setup.

### Workflow Architecture
```
Schedule Trigger (7AM daily)
      ↓                    ↓
HTTP Request (Kaduna)   HTTP Request (Zaria)
      ↓                    ↓
      └──────→ Merge ←─────┘
                  ↓
         Code (Format Report)
                  ↓              ↓
            Gmail (Email)    Code (Recipients)
                                  ↓
                           Telegram (Broadcast)
```

### Key Features
- Fetches real-time weather from OpenWeatherMap API for 2 cities simultaneously
- Rain detection — alerts users when Rain, Drizzle, or Thunderstorm is detected
- Formatted weather report including temperature, feels like, humidity, wind speed, and condition
- Sends email report via Gmail automatically
- Broadcasts to multiple Telegram recipients via bot
- Fully scheduled — zero human input after activation

### Sample Alert Output
```
Good morning! 🌅

Here is your daily weather report for Monday, 15 June 2026.

━━━━━━━━━━━━━━━━━━━━━━
📍 Kaduna
🌡️ Temperature: 28°C (Feels like 31°C)
💧 Humidity: 74%
🌤️ Condition: light rain
💨 Wind Speed: 3.2 m/s
☔ RAIN EXPECTED — carry an umbrella!

━━━━━━━━━━━━━━━━━━━━━━
📍 Zaria
🌡️ Temperature: 27°C (Feels like 30°C)
💧 Humidity: 68%
🌤️ Condition: partly cloudy
💨 Wind Speed: 2.8 m/s
☀️ No rain expected today.
━━━━━━━━━━━━━━━━━━━━━━

Stay prepared and have a great day!
— Your Weather Bot 🤖
```

### n8n Concepts Demonstrated
✅ Schedule Trigger · ✅ HTTP Request · ✅ Merge · ✅ Code Node · ✅ Gmail · ✅ Telegram · ✅ Multi-recipient Broadcasting

---

## 📬 Project 3 — Smart Applicant Response System

### Problem
Most organizations send the same generic email to every applicant regardless of their skill level. This makes communication feel impersonal and ineffective — beginners need encouragement, intermediates need challenge, and advanced learners should be treated as collaborators.

### Solution
A fully automated n8n workflow that reads applicant data from Google Sheets, detects their skill level using a Switch node, and sends a fully personalized email response — then updates the sheet with the result automatically.

### Workflow Architecture
```
Google Sheets Trigger (new row added)
      ↓
Set Node (clean & prepare data)
      ↓
Switch Node (route by skill level)
      ↓              ↓               ↓
Wait 3min       Wait 3min       Wait 3min
      ↓              ↓               ↓
Gmail            Gmail           Gmail
(Beginner)   (Intermediate)   (Advanced)
      ↓              ↓               ↓
      └──────────────┴───────────────┘
                     ↓
         Google Sheets Update Row
         (Status = Email Sent)
```

### Key Features
- Triggers automatically when a new applicant row is added to Google Sheets
- Extracts and cleans applicant data using a Set node (first name, email, level, commitment hours, city)
- Routes applicants into 3 branches using a Switch node based on automation skill level
- Sends fully personalized emails per branch:
  - **Beginner** — friendly & encouraging tone, WhatsApp group link, next steps
  - **Intermediate** — direct & challenging tone, real-world automation expectations
  - **Advanced** — professional & opportunity-focused, collaboration & API/AI mention
- 3-minute Wait node before each email — simulates real-world email pacing
- Updates Google Sheet after sending with Status, Level Category, and Timestamp

### Routing Logic
| Input Value | Route |
|-------------|-------|
| Beginner, No experience | Beginner Branch |
| Intermediate, Some experience | Intermediate Branch |
| Advanced, Expert, Professional | Advanced Branch |

### Design Decisions
| Decision | Justification |
|----------|--------------|
| Switch node over IF node | Handles 3+ conditions cleanly in one node |
| 3-minute Wait node | Simulates email pacing, reduces system overload |
| Google Sheets as data source | No-code friendly, easy for non-technical admins to manage |
| Update row after sending | Creates an audit trail and prevents duplicate sends |
| Personalized subject lines | Increases open rates — generic subjects get ignored |

### Email Subject Lines
```
Beginner:     "Welcome to n8n Automation — Your Journey Starts Here"
Intermediate: "You're In — Let's Build Real Automation Skills"
Advanced:     "Selected: Advanced Track in n8n Automation Cohort"
```

### n8n Concepts Demonstrated
✅ Google Sheets Trigger · ✅ Set Node · ✅ Switch Node · ✅ Wait Node · ✅ Gmail · ✅ Google Sheets Update · ✅ Conditional Routing · ✅ Data Personalization

---

## 🧰 Tools & Technologies

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation platform |
| **REST APIs** | External data integration |
| **JavaScript** | Data transformation & logic |
| **OpenWeatherMap API** | Live weather data |
| **Gmail** | Email delivery |
| **Telegram Bot API** | Instant messaging & broadcasting |
| **Google Sheets** | Data source & result tracking |

---

## 👤 About Me

I'm a cybersecurity student passionate about automation and building systems that solve real problems. I combine cybersecurity knowledge with automation skills to create smart, efficient workflows.

📫 Connect with me on [LinkedIn](https://linkedin.com/in/muhammad-khamis-armaya-u-6180153a7)


---

*More projects coming soon — follow to stay updated!* ⭐

