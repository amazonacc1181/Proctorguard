# ProctorGuard V3 — CBT + proctoring API

V3 is being introduced alongside the working V2 static prototype. The existing `index.html` is intentionally unchanged. This branch adds a backend API foundation for 180-question NEET-style exams, question-bank imports, session answer persistence, advisory monitoring events, analytics, and reports.

## Architecture and status

- **Existing V2:** remains the GitHub Pages landing/practice app; it is not overwritten by this branch.
- **V3 API:** Node.js 20+, Express, SQLite, JWT, bcrypt, Helmet, CORS allow-list and rate limiting.
- **Exam rules:** publishing requires exactly 180 questions split Physics 45, Chemistry 45, Biology 90. Questions are imported from a JSON bank; the API does not invent or label placeholder questions as official NEET content.
- **CBT:** server-created attempts, per-answer save, review flag, +4/−1/0 scoring, final answer review and persisted report.
- **Admin:** candidate account creation, exam authoring/import/publish, live/recent sessions, event timeline, dashboard, subject/chapter analytics, CSV export.
- **AI-assisted monitoring:** existing V2 browser face checks remain local. V3 API accepts only typed advisory event metadata; do not upload face images, face descriptors, or raw webcam frames. Monitoring events must not automatically disqualify candidates.
- **Important:** this branch contains the API foundation, not a fully wired V3 browser UI or a live hosted API yet. GitHub Pages cannot run Node or SQLite. Deploy this API to a Node-capable host with persistent disk, then configure a V3 client with its HTTPS API origin before enabling shared sessions. Do not merge this branch to `main` until CI and deployment configuration are reviewed.

## Run locally

Requires Node.js 20+ and npm.

```bash
npm install
cp .env.example .env
```

Generate a strong JWT secret and admin password hash, then set them in `.env`. One way to generate the hash:

```bash
node -e "require('bcryptjs').hash('replace-with-a-long-unique-password',12).then(console.log)"
```

Set `ADMIN_EMAIL`, `ADMIN_PASSWORD_HASH`, and a random `JWT_SECRET` (32+ characters). Start:

```bash
npm start
```

Check `GET http://localhost:3000/api/health`. The admin account is provisioned on first successful login using the configured email and hash. SQLite data is stored under `data/` by default; use a persistent volume in hosting.

## Import a 180-question bank

Create a draft exam as admin, then send `POST /api/admin/exams/:id/questions/import` with JSON:

```json
{
  "questions": [
    {
      "subject": "Physics",
      "chapter": "Kinematics",
      "difficulty": "medium",
      "prompt": "Question text",
      "options": ["Option A", "Option B", "Option C", "Option D"],
      "answerIndex": 2,
      "explanation": "Why option C is correct."
    }
  ]
}
```

The sample above illustrates the schema only; it is not an actual exam question. Import all 180 reviewed items. `answerIndex` is zero-based. Publish with `POST /api/admin/exams/:id/publish` only after exact subject counts pass validation.

## Main endpoints

- `POST /api/auth/login`, `GET /api/me`
- `GET /api/exams`, `POST /api/admin/exams`
- `POST /api/admin/exams/:id/questions/import`, `POST /api/admin/exams/:id/publish`
- `POST /api/exams/:id/sessions`, `PUT /api/sessions/:id/answers`, `POST /api/sessions/:id/events`, `POST /api/sessions/:id/submit`
- `GET /api/reports/:id`
- `GET /api/admin/dashboard`, `GET /api/admin/analytics`, `GET /api/admin/sessions/:id/events`, `GET /api/admin/reports/export.csv`

All except `GET /api/health` and login require a bearer token. Admin routes require the admin role. Reports include answer keys, so candidates can only access their own report and only after submission.

## Tests and security notes

Run `npm test`. Before real deployment also run dependency scanning, API integration tests, backup/restore tests, HTTPS and CORS verification, and a privacy/accessibility review. Configure CORS to the exact frontend origin(s), protect the database volume, use strong unique secrets, rotate credentials, and back up the SQLite file. For multiple concurrent server replicas, replace SQLite with a managed database. Never store passwords in plaintext. The prototype does not implement liveness detection, and heuristic face matching is not proof of identity or misconduct.
