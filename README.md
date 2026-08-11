# 🔐 PasswordGuard — Password Strength Analyzer

An MCA academic project: a full-stack web application that analyzes password
strength, suggests real improvements, generates secure passwords, and
teaches basic password security — without ever storing a user's actual
password.

## Features

1. **Password Strength Checker** — scores a password 0-100 based on length,
   character variety, repeated characters, common passwords, and simple
   patterns; shows a strength label (Very Weak → Very Strong) with reasons.
2. **Password Suggestions** — gives technique-based advice (passphrases,
   mid-string substitution, password managers) instead of lazy tips like
   "just add 123."
3. **Password Generator** — adjustable length and character-type options,
   uses the browser's cryptographically secure random generator, includes a
   Copy button.
4. **Security Tips** — plain-language explanations of password reuse,
   password managers, and two-factor authentication.
5. **Anonymous Analysis History** — optionally save just the score,
   strength, problems, and suggestions to MongoDB (never the password).

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React.js (Vite), React Router, CSS |
| Backend | Node.js, Express.js |
| Database | MongoDB, Mongoose |
| Language | JavaScript (ES Modules) |

## Project Structure

```
password-strength-analyzer/
├── frontend/
│   ├── src/
│   │   ├── api/            # fetch wrappers for the backend
│   │   ├── components/     # Navbar, Footer
│   │   ├── pages/          # Home, Analyzer, Generator, SecurityTips, About
│   │   ├── utils/          # passwordChecker, passwordSuggestions, passwordGenerator
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── .env                # VITE_API_URL
│   └── package.json
│
├── backend/
│   ├── config/db.js        # MongoDB connection
│   ├── models/Analysis.js  # Mongoose schema (no password field, ever)
│   ├── controllers/
│   ├── routes/
│   ├── server.js
│   ├── .env.example        # copy to .env and fill in MONGO_URI
│   └── package.json
│
├── TESTING.md
├── .gitignore
└── README.md
```

## Setup Instructions

### 1. Backend

```bash
cd backend
npm install
cp .env.example .env
```

Edit `backend/.env` and set your MongoDB connection string:

```
MONGO_URI=mongodb+srv://<username>:<password>@<cluster-url>/passwordguard
PORT=5000
```

Start the backend:

```bash
npm run dev
```

You should see:
```
MongoDB connected: <your-cluster-host>
Server running on port 5000
```

### 2. Frontend

In a **new terminal**:

```bash
cd frontend
npm install
npm run dev
```

Open the URL shown (usually `http://localhost:5173/`).

The frontend already has a `.env` file pointing to `http://localhost:5000/api` —
change `VITE_API_URL` there if your backend runs on a different port.

## Security Design

- The password a user types is analyzed **entirely in the browser** and is
  **never sent to the server**.
- Only the anonymous analysis result (score, strength label, problems,
  suggestions, timestamp) can be optionally saved to MongoDB.
- No password is ever printed to any console log.
- No password is ever stored in `localStorage` or `sessionStorage`.
- MongoDB credentials are kept in a `.env` file (excluded from Git via
  `.gitignore`) rather than hardcoded.
- Generated passwords use `window.crypto.getRandomValues()`, a
  cryptographically secure random source — not `Math.random()`.
- All backend input is validated before being saved to the database.

## API Reference

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/analysis/recent` | Returns the 10 most recent analysis results |
| POST | `/api/analysis` | Saves a new analysis result `{ score, strength, problems, suggestions }` |

## Testing

See [TESTING.md](./TESTING.md) for a full manual test checklist.

## Author's Note (MCA Project Context)

This project was built step-by-step as a learning exercise covering:
React component design and routing, client-side algorithms (scoring logic),
secure random generation, REST API design with Express, MongoDB schema
design with Mongoose, environment-variable-based configuration, and
privacy-conscious application architecture.
