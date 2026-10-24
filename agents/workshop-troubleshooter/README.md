# TShoot — Autonomous Technical Troubleshooter Agent

> An autonomous, security-hardened multi-agent co-pilot built with the Google Agent Development Kit (ADK) that diagnoses and resolves hands-on technical roadblocks during live Google Meet and Microsoft Teams workshops.

---

## Overview

### Problem Statement & Intended Users
- **Problem**: During hands-on technical workshops, hackathons, and virtual training calls (Google Meet, Microsoft Teams), attendees frequently encounter roadblocks—such as Google Account 403 login failures, OAuth `redirect_uri_mismatch`, port conflicts (`WinError 10048`), corporate proxy blocks, Python deprecations, or invalid API keys. Instructors presenting cannot stop the live session to individually debug 50+ participants in chat.
- **Intended Users**: Workshop attendees, virtual lab participants, instructors, teaching assistants (TAs), and community organizers (e.g., GDG events).

### Key Features
- **Visual Screenshot Inspector**: Drag & drop, browse, or paste (`Ctrl+V`) a screenshot of where you are stuck. TShoot automatically parses image dimensions, detects error context, and runs autonomous triage.
- **Multi-Agent Collaborative Architecture**:
  - **Auth Specialist**: Diagnoses Google Account 403, OAuth redirect URI mismatch, and credential anomalies.
  - **Network Specialist**: Pings `accounts.google.com`, GenAI endpoints, and PyPI to detect corporate proxy/firewall intercepts.
  - **Web Search Specialist**: For the "Others" category or rare errors, searches developer forums (StackOverflow, GitHub Issues, official docs) and provides verified fixes with source links.
  - **Deep Investigator Specialist**: Multi-tier deep diagnostic engine (Tier 1: cache purge/virtualenv rebuild, Tier 2: dependency flags, Tier 3: socket mitigation) with consent-based progressive resolution.
  - **Remediation Specialist**: Generates platform-specific (Windows PowerShell, macOS, Linux) shell commands.
- **Interactive Resolution Feedback Loop**: Prompts attendees *"Did this solve your issue?"* and loops with progressive deep investigation until the issue is solved.
- **Chat Escalation Protocol**: One-click formatted message generation for Google Meet / Microsoft Teams chat when human host action is required.
- **OWASP Enterprise Security**:
  - Automatic secret redactor scrubs API keys from logs and inputs.
  - OWASP XSS defense & HTML entity sanitization.
  - In-memory sliding-window IP rate limiting (40 req/min).
  - Parameterized SQLite persistence (zero SQL injection).
  - Debug mode strictly turned OFF (`debug=False`).

---

## Technical Stack

- **Language & Runtime**: Python 3.10+ (tested on Python 3.12)
- **Agent Framework**: **Google Agent Development Kit (ADK)** (`google-adk`), using the **COLLAB Multi-Agent Pattern** (Coordinator + Specialized Sub-Agents)
- **Web & API Framework**: FastAPI (production hardened), Uvicorn, Pydantic v2
- **Image Processing**: Pillow (PIL)
- **Storage**: Parameterized SQLite (`sqlite3`) for persistent tickets and audit logs

---

## Architecture & Data Flow

```
[Attendee Error / Screenshot / Query]
               │
               ▼
   [OWASP Security Middleware] ───► [Secret Scrubber & XSS Sanitizer] ───► [Rate Limiter]
               │
               ▼
   [TShoot Multi-Agent Coordinator]
         │
         ├───► [Auth Specialist] ──────────────► diagnose_login_issue()
         ├───► [Network Specialist] ───────────► check_service_connectivity()
         ├───► [Web Search Specialist] ────────► search_internet_solutions()
         ├───► [Deep Investigator Specialist] ─► deep_troubleshoot_investigation()
         ├───► [Remediation Specialist] ───────► get_safe_remediation_commands()
         └───► [Escalation Specialist] ────────► create_instructor_ticket()
               │
               ▼
[Step-by-Step Remediation Plan with Code Snippets & Clickable Sources]
```

---

## Prerequisites

- Python 3.10+
- `pip` package manager
- Modern Web Browser (Chrome, Edge, Firefox, Safari)

---

## Installation & Setup

1. **Navigate to the agent directory:**
   ```bash
   cd agents/workshop-troubleshooter
   ```

2. **Set up a virtual environment and install dependencies:**
   ```bash
   # Windows (PowerShell):
   python -m venv .venv
   .venv\Scripts\activate
   pip install -r requirements.txt

   # Linux / macOS:
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

3. **Configure Environment Variables:**
   ```bash
   cp .env.example .env
   ```

---

## Running TShoot

Start the TShoot server from the agent directory:
```bash
python -m uvicorn app:app --host 127.0.0.1 --port 8000
```
Open **`http://127.0.0.1:8000`** in your browser to access the interactive portal.

---

## Quality & Security Audit

Verify against the 8 OWASP security benchmarks:
```bash
python security_audit.py
```
Score: **100% (Grade: A+)** with 8/8 security benchmarks passing.

---

## Contributing

This agent is packaged according to the community contribution guidelines:
- Dedicated self-contained folder: `agents/workshop-troubleshooter/`
- Zero tracked secrets, clean `.gitignore`, `.env.example`, and full reproducibility.
