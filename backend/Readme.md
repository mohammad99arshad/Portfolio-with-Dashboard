# Backend — README

Minimal backend for the Portfolio project. This document explains how to set up, run and safely manage the repository (especially environment secrets) and how to add backend subfolders to git one-by-one.

## Contents
- `server.js` — application entry point
- `app.js` — Express app and middleware (imported by `server.js`)
- `config/` — environment file(s) (should not be committed)
- `controller/`, `routes/`, `models/`, `middlewares/`, `utils/`, `database/` — application code

## Prerequisites
- Node.js 18+ (or the version you use) and npm
- MongoDB connection string if you run locally

## Install
From the `backend/` directory:

```bash
npm install
```

## Environment
Place secrets in `backend/config/config.env` (this file must never be committed). Example variables used by this project:

- `PORT` — server port (example: `5000`)
- `MONGO_URI` — MongoDB connection string
- `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`
- `JWT_SECRET_KEY`, `JWT_EXPIRES`, `COOKIE_EXPIRE`
- `SMTP_HOST`, `SMTP_PORT`, `SMTP_SERVICE`, `SMTP_MAIL`, `SMTP_PASSWORD`

Security note: `backend/config/config.env` was intentionally removed from the repo and is listed in `.gitignore`. Keep it out of source control.

## Scripts

- `npm run start` — start the server with Node
- `npm run dev` — start with `nodemon` (development)

## Run locally

```bash
# from repository root
cd backend
npm install
# ensure `backend/config/config.env` exists with required variables
npm run dev
```

## Git workflow — add backend content one-by-one

This repository ignores `backend/*` by default to avoid leaking secrets. To add backend folders or files one at a time and keep other files ignored, follow either approach:

1) Quick add a single file (force-add, no .gitignore changes):

```bash
# from repo root
git add -f backend/server.js
git commit -m "Add backend/server.js"
git push
```

2) Preferred: whitelist a specific folder/file in `.gitignore` then add normally.

```bash
# add a negation to allow full folder contents (example: controller)
printf '%s\n' '!/backend/controller/' '!/backend/controller/**' >> .gitignore
git add .gitignore
git commit -m "Whitelist backend/controller in .gitignore"
# then add the folder normally
git add backend/controller
git commit -m "Add backend/controller"
git push
```

3) To stop tracking a sensitive file that accidentally got committed (e.g. `config.env`):

```bash
git rm --cached backend/config/config.env
printf '%s\n' '/backend/config/config.env' >> .gitignore
git add .gitignore
git commit -m "Stop tracking backend/config/config.env and ignore it"
git push
```

## Notes
- Do not commit `backend/config/config.env` or other secret files. They should remain local and listed in `.gitignore`.
- `node_modules/` is always skipped; do not commit it.
- If you want me to add specific backend folders now (one commit per folder), tell me the order and I'll add them and push individually.

---
If anything here should be adjusted for your workflow (different port, extra env vars, or a different add-order), tell me which change and I'll update this README and push it.
