[README.md](https://github.com/user-attachments/files/32476677/README.md)
# Life Sport

> A MERN-stack physiotherapy clinic management system, because apparently paper cards, mystery balances, and "who forgot to tell the physio?" were not a sustainable long-term strategy.

[![React](https://img.shields.io/badge/React-18-20232A?style=for-the-badge&logo=react)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-5-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/)
[![Express](https://img.shields.io/badge/Express-4-000000?style=for-the-badge&logo=express)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Socket.IO](https://img.shields.io/badge/Socket.IO-Realtime-010101?style=for-the-badge&logo=socketdotio)](https://socket.io/)

---

## Table of Contents

- [About](#about)
- [Why This Exists](#why-this-exists)
- [Core Features](#core-features)
- [User Roles](#user-roles)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Available Scripts](#available-scripts)
- [Testing](#testing)
- [Deployment](#deployment)
- [Documentation](#documentation)
- [Team Ownership](#team-ownership)

---

## About

**Life Sport** is a full-stack web application for managing the daily operations of a physical therapy center in Egypt.

It replaces paper-based patient tracking cards with a centralized platform for:

- patient registration,
- QR and manual attendance check-in,
- session balance tracking,
- company billing contracts,
- clinical notes,
- doctor visit notes,
- absence alerts,
- treatment programs,
- admin dashboards,
- physiotherapist dashboards,
- and a read-only patient portal.

So yes, it does slightly more than a spreadsheet. Shocking, really.

---

## Why This Exists

Clinics should not have to rely on:

| Traditional Problem | Life Sport Solution |
|---|---|
| Paper cards get lost, damaged, or creatively interpreted | Patient profiles live in MongoDB, where at least the database remembers things |
| Session balances are counted manually | Every valid check-in decrements balance automatically |
| Physiotherapists miss critical notes | Clinical and doctor notes surface during check-in workflows |
| Patients stop attending and nobody notices | Scheduled absence detection creates alerts |
| Company billing varies by contract | Admins define flexible company contracts and per-session pricing |
| Patients call reception just to ask basic questions | Patient portal shows balance, QR code, schedule, notes, and visit history |

The goal is simple: **accurate session tracking and visible clinical context at the exact moment it matters.**

---

## Core Features

### Patient Management

- Register patients with full profile details.
- Validate Egyptian phone numbers.
- Generate QR codes for check-in.
- Configure custom daily, weekly, and monthly session limits.
- Manage active, on-hold, and discharged patient statuses.

### Attendance Check-in

- QR code scanning through the browser camera.
- Manual patient search and check-in.
- Automatic balance decrement.
- Duplicate scan protection.
- Warning and override flow for limits or special cases.
- Offline queue for check-ins when the internet decides to participate later.

### Billing and Contracts

- Register companies and insurers.
- Create flexible contracts with custom package size and price per session.
- Preserve historical pricing by deactivating old contracts instead of mutating them.
- Calculate revenue summaries for dashboards and history views.

### Clinical Workflow

- Add clinical notes with severity levels.
- Add doctor visit notes with acknowledgement requirements.
- Block check-in progression when unread doctor notes require attention.
- Maintain treatment programs with modalities and durations.
- Show clinical context to physiotherapists during patient arrival.

### Alerts and Realtime Updates

- Low-balance alerts.
- Zero-balance refill queue.
- Absence detection based on weekly patient schedules.
- Optional SMS absence warnings through Twilio.
- Realtime dashboard updates using Socket.IO.

### Patient Portal

- Read-only patient dashboard.
- Remaining session balance.
- QR code display.
- Attendance history.
- Weekly schedule.
- Doctor notes.
- Active treatment program.
- Password change flow.

---

## User Roles

| Role | What They Can Do |
|---|---|
| Admin | Manage staff, patients, companies, contracts, balances, limits, clinical data, alerts, and dashboards |
| Receptionist | Register patients, check patients in, manage companies/contracts, and handle operational workflows |
| Physiotherapist | View patient profiles, see clinical alerts, manage treatment/clinical notes, and use physio dashboard |
| Patient | View their own read-only portal, because letting patients edit clinic records would be a bold legal experiment |

Authentication is phone-number based using Egyptian formats: `010x`, `011x`, `012x`, or `015x`.

---

## Tech Stack

### Frontend

- React 18
- Vite 5
- Tailwind CSS 3
- Redux Toolkit
- RTK Query
- React Router DOM 6
- Axios
- react-i18next
- html5-qrcode
- Socket.IO Client
- Dexie for offline queue storage

### Backend

- Node.js
- Express.js 4
- MongoDB Atlas
- Mongoose 8
- JWT authentication
- bcryptjs password hashing
- Socket.IO
- node-cron
- Helmet
- CORS
- express-rate-limit
- express-mongo-sanitize
- Twilio SMS integration

### Testing

- Jest
- Supertest
- mongodb-memory-server

---

## Architecture

```text
React + Vite Client
        |
        | Axios + JWT
        | Socket.IO Client
        v
Express API Server
        |
        | Middleware
        | - CORS
        | - Helmet
        | - Rate limiting
        | - Auth
        | - RBAC
        | - Mongo sanitization
        v
Routes
        v
Controllers
        v
Service Layer
        |
        | EventBus
        | - alert persistence
        | - realtime socket bridge
        v
Mongoose Models
        v
MongoDB Atlas
```

The project follows a strict **MVC + Service Layer** style. Routes route, controllers control, services do the real work, and nobody puts business logic in random places just to keep future maintainers emotionally unstable.

---

## Project Structure

```text
life-sport/
|-- client/
|   |-- src/
|   |   |-- components/
|   |   |-- hooks/
|   |   |-- i18n/
|   |   |-- pages/
|   |   |-- services/
|   |   `-- store/
|   |-- package.json
|   `-- vite.config.js
|
|-- server/
|   |-- src/
|   |   |-- config/
|   |   |-- controllers/
|   |   |-- events/
|   |   |-- jobs/
|   |   |-- middleware/
|   |   |-- models/
|   |   |-- routes/
|   |   |-- services/
|   |   `-- utils/
|   |-- tests/
|   |-- package.json
|   `-- index.js
|
|-- DISCUSSION_*.md
|-- PROJECT_OVERVIEW.md
|-- REQUIREMENTS.md
|-- SECURITY.md
`-- TECH_STACK.md
```

---

## Getting Started

### Prerequisites

Install the following, preferably before blaming the code:

- Node.js 18 or newer
- npm 9 or newer
- MongoDB Atlas database or local MongoDB connection string
- Git

### 1. Clone the repository

```bash
git clone https://github.com/Mosalah4351/life-sport.git
cd life-sport
```

### 2. Install dependencies

```bash
npm --prefix client install
npm --prefix server install
```

### 3. Configure environment variables

Create `server/.env`:

```env
PORT=3001
HOST=0.0.0.0
NODE_ENV=development
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret
FRONTEND_URL=http://localhost:5173
CRON_ENABLED=false
```

Create `client/.env`:

```env
VITE_API_URL=http://localhost:3001/api
```

If SMS absence alerts are required, add Twilio settings to `server/.env`:

```env
TWILIO_ACCOUNT_SID=your_twilio_account_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
TWILIO_PHONE_NUMBER=your_twilio_phone_number
```

### 4. Run the backend

```bash
npm --prefix server run dev
```

The API runs on:

```text
http://localhost:3001
```

### 5. Run the frontend

```bash
npm --prefix client run dev
```

The app runs on:

```text
http://localhost:5173
```

---

## Environment Variables

### Server

| Variable | Purpose |
|---|---|
| `PORT` | Backend server port |
| `HOST` | Backend host binding |
| `NODE_ENV` | Runtime environment |
| `MONGODB_URI` | MongoDB connection string |
| `JWT_SECRET` | JWT signing secret |
| `FRONTEND_URL` | Allowed frontend origin for CORS and Socket.IO |
| `CRON_ENABLED` | Enables or disables scheduled absence detection |
| `TWILIO_ACCOUNT_SID` | Twilio account identifier |
| `TWILIO_AUTH_TOKEN` | Twilio secret token |
| `TWILIO_PHONE_NUMBER` | Twilio sender number |

### Client

| Variable | Purpose |
|---|---|
| `VITE_API_URL` | Base API URL used by the React client |

---

## Available Scripts

### Client

```bash
npm --prefix client run dev
npm --prefix client run build
npm --prefix client run lint
npm --prefix client run preview
```

### Server

```bash
npm --prefix server start
npm --prefix server run dev
npm --prefix server test
npm --prefix server run test:unit
npm --prefix server run test:integration
```

---

## Testing

Run the backend test suite:

```bash
npm --prefix server test
```

Run a production frontend build:

```bash
npm --prefix client run build
```

Recent verification at cleanup time:

```text
Backend: 14 test suites passed, 74 tests passed
Frontend: Vite production build completed successfully
```

So yes, there are tests. Civilization has not completely collapsed.

---

## Deployment

The app is designed as a split deployment:

| Part | Platform | Reason |
|---|---|---|
| Frontend | Vercel | React/Vite static app hosting |
| Backend | Render | Persistent Node.js process for Socket.IO and cron jobs |
| Database | MongoDB Atlas | Managed MongoDB database |

Why not deploy the backend on Vercel? Because Socket.IO and cron jobs need a persistent process, and serverless functions are famously not that. They appear, do one thing, disappear, and somehow still get invited to architecture diagrams.

---

## Documentation

The repository includes detailed project documents:

| File | Purpose |
|---|---|
| `PROJECT_OVERVIEW.md` | High-level system explanation |
| `REQUIREMENTS.md` | Functional and non-functional requirements |
| `TECH_STACK.md` | Technology choices and rationale |
| `SECURITY.md` | Security decisions and mitigations |
| `FOLDER_STRUCTURE.md` | Repository structure reference |
| `AUDIT-REPORT.md` | Audit findings and cleanup notes |
| `DISCUSSION_*.md` | Viva/discussion guides by team member |

---

## Team Ownership

| Member | Area |
|---|---|
| Mohamed Salah | Team Lead, Auth, Infrastructure |
| Shahd | Backend, Patient and Attendance |
| Mariam | Backend, Alerts and Company Contracts |
| Lina | Frontend, Admin and Check-in Flows |
| Mazen | Frontend, Physio and Patient Dashboards |
| Yehia | Database, QA, DevOps |

Each member has a dedicated discussion guide at the root of the repository. This is useful when someone asks, "who wrote this?" and the room suddenly becomes very interested in the ceiling.

---

## Security Notes

- JWT-based authentication.
- Password hashing with bcryptjs.
- Role-based access control.
- Rate limiting for API and login routes.
- Helmet security headers.
- MongoDB query sanitization.
- Refresh-session revocation.
- Audit logging for sensitive staff/admin actions.
- Patient portal restricted to self-owned data.

---

## Final Note

Life Sport is built for a small physiotherapy clinic, but the code still tries to behave like a responsible adult: clear layers, explicit roles, real validation, tests, security middleware, and documentation that does not require archaeological training.

If you are replacing paper cards with software, this is the part where the software should actually be better than paper. Low bar, apparently. Still cleared.
