# Project Report: Password Strength Analyzer

## 1. Introduction

Weak and reused passwords are one of the most common causes of account
compromise. This project, **PasswordGuard**, is a full-stack web
application built to help users evaluate the strength of their passwords,
understand *why* a password is weak, receive actionable advice to improve
it, generate strong random passwords, and learn general password-security
best practices — all while never storing or transmitting the user's actual
password.

## 2. Objectives

1. Design and implement a scoring algorithm that evaluates password
   strength based on multiple security criteria.
2. Provide clear, non-generic suggestions for improving weak passwords.
3. Implement a cryptographically secure password generator.
4. Build a RESTful backend using Node.js and Express.
5. Persist anonymous analysis results using MongoDB, without ever storing
   plaintext passwords.
6. Apply secure coding practices throughout (input validation,
   environment-based secrets, no sensitive logging).

## 3. Scope

The application covers four core modules — Analyzer, Generator, Security
Tips, and About/Home — connected through a shared navigation layer, backed
by a small REST API and a MongoDB collection for anonymous analysis
history.

## 4. System Architecture

```
┌─────────────────┐        HTTPS/JSON        ┌──────────────────┐        ┌──────────────┐
│   React Frontend │  ───────────────────────▶ │  Express Backend │ ─────▶ │   MongoDB    │
│   (Vite, Port    │  (score/strength/problems  │   (Port 5000)    │        │  (Analysis   │
│    5173)          │   /suggestions ONLY —      │                  │        │  collection) │
│                   │   never the password)      │                  │        │              │
└─────────────────┘  ◀─────────────────────── └──────────────────┘ ◀───── └──────────────┘
        │
        ▼
 Password strength
 checking & generation
 run 100% client-side
```

## 5. Modules

### 5.1 Password Analyzer
Evaluates a password against: length, uppercase/lowercase/number/special
character presence, repeated-character sequences, common-password
dictionary matches, and simple keyboard/number patterns. Produces a
0-100 score, a strength label, and a list of human-readable reasons.

### 5.2 Password Suggestions
Generates technique-level advice (passphrases, mid-string character
substitution, avoiding dictionary words, using a password manager) based
on which checks failed — deliberately avoiding trivial advice such as
appending `123` or `!`.

### 5.3 Password Generator
Lets the user choose a length (8-32) and which character types to include.
Uses `window.crypto.getRandomValues()` for cryptographically secure
randomness, guarantees at least one character from each selected type, and
shuffles the result using the Fisher-Yates algorithm.

### 5.4 Security Tips
Static educational content explaining password reuse risk, password
managers, and two-factor authentication in plain language.

### 5.5 Analysis History (MongoDB)
Stores only `{ score, strength, problems[], suggestions[], createdAt }` —
the schema has no field for a password at all, making it structurally
impossible to store one by accident.

## 6. Database Design

**Collection:** `analyses`

| Field | Type | Description |
|---|---|---|
| `score` | Number (0-100) | Computed strength score |
| `strength` | String (enum) | Very Weak / Weak / Medium / Strong / Very Strong |
| `problems` | [String] | Reasons detected during analysis |
| `suggestions` | [String] | Improvement suggestions shown to the user |
| `createdAt` | Date | Timestamp, defaults to save time |

## 7. Security Considerations

- No password field exists anywhere in the database schema.
- Passwords are analyzed and generated entirely client-side; the network
  tab shows no request ever carries a raw password.
- `console.log` is never used on password values, anywhere in the codebase.
- MongoDB credentials are read from environment variables (`.env`), which
  are excluded from version control via `.gitignore`.
- All backend endpoints validate incoming data types and ranges before
  writing to the database.
- Password generation uses the Web Crypto API instead of `Math.random()`,
  which is not cryptographically secure.

## 8. Technology Justification

- **React + Vite**: fast development experience, component-based UI,
  well-suited for an interactive single-page application.
- **Express.js**: minimal, well-documented framework for building the
  small REST API this project needs.
- **MongoDB + Mongoose**: flexible document schema, straightforward to
  set up for an academic project, and a natural fit for storing simple
  analysis records.

## 9. Future Enhancements

- Expand the common-password dictionary using a larger breach-data list.
- Add user accounts to track personal (still password-free) analysis
  history over time.
- Add a password-strength meter that estimates crack time (entropy-based).
- Add automated unit tests for the scoring and generator utilities.

## 10. Conclusion

PasswordGuard demonstrates a complete, privacy-conscious full-stack
application: a React frontend that performs all sensitive computation
locally, a lightweight Express API, and a MongoDB collection that stores
only anonymized results. The project reinforces core MCA coursework
concepts — REST API design, database schema design, secure coding
practices, and modern frontend development — around a practical,
relatable security topic.
