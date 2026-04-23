Agent context & runbook for continuing work on this repo
======================================================

Purpose
-------
This file collects the essential context, actions performed, and next steps so another automated agent (or a human acting as an agent) can pick up work immediately on a fresh machine or CI runner.

Important: do NOT expect any secrets in this file. Secrets (Supabase keys, Vercel tokens) are NOT stored here.

What was done (high level)
---------------------------
- Created and merged `.github/copilot-instructions.md` (guidance for AI coding agents).
- Added CI for PRs: `.github/workflows/ci.yml` (runs lint & tests).
- Added developer docs: `DEV_SETUP.md`, `ONBOARDING.md`, `MACHINE_COMMANDS.txt`, `SETUP_LINKS.md`.
- Added automation helpers:
  - `scripts/bootstrap.sh` — bootstraps a new machine (installs nvm/node, installs dependencies, copies `.env.example` -> `.env.local`).
  - `scripts/update-vercel-envs.sh` — updates Vercel project env vars using Vercel API.
- Security cleanup:
  - A tracked `.env` containing `VITE_SUPABASE_URL` and `VITE_SUPABASE_PUBLISHABLE_KEY` was removed from the repo and then purged from git history.
  - A backup branch `backup-before-filter` was pushed to origin before history rewrite (if recovery needed).
  - The repo history was force-updated; collaborators must re-clone.

Key files and where to find them
--------------------------------
- Bootstrapping and commands:
  - `scripts/bootstrap.sh` (curl+run to bootstrap)
  - `MACHINE_COMMANDS.txt` (copyable commands for new machines)
  - `DEV_SETUP.md` (human-oriented setup checklist)
  - `SETUP_LINKS.md` (raw URLs for bootstrap and quick fixes)

- Vercel / env update helper:
  - `scripts/update-vercel-envs.sh` — requires env vars `VERCEL_TOKEN`, `VERCEL_PROJECT_ID`, `NEW_SUPABASE_URL`, `NEW_SUPABASE_KEY` when run.

- Cleanup documentation:
  - `ISSUE_CLEANUP.md` — summary of the cleanup and next steps for collaborators.

How to pick up work (recommended sequence)
-----------------------------------------
1) Re-clone the repo (must re-clone because history was rewritten):
   git clone git@github.com:therealshaun/blitzbuyai.git
   cd blitzbuyai

2) Bootstrap the machine (run as the local user):
   bash scripts/bootstrap.sh
   # edit .env.local and add VITE_SUPABASE_URL and VITE_SUPABASE_PUBLISHABLE_KEY (do NOT commit)

3) Start the dev server and verify the app runs:
   npm run dev
   # visit http://localhost:8080

4) Rotate Supabase publishable key in the Supabase dashboard now (this is critical):
   - Supabase → Project → Settings → API → Keys → rotate anon/public key

5) Update Vercel environment variables with the new key (two options):
   - Manual: Vercel dashboard → Project → Settings → Environment Variables (add/update `VITE_SUPABASE_PUBLISHABLE_KEY` and `VITE_SUPABASE_URL` for Preview & Production)
   - Scripted: run `scripts/update-vercel-envs.sh` with these env vars set:
     VERCEL_TOKEN, VERCEL_PROJECT_ID, NEW_SUPABASE_URL, NEW_SUPABASE_KEY

6) Validate CI on a PR:
   - Create a small feature branch, push it, open a PR and confirm actions in `.github/workflows/ci.yml` run.

7) If you need preserved local commits (previous work):
   - The backup branch is `backup-before-filter` on origin. Inspect it and cherry-pick commits you want into a new branch.

Agent instructions: what to automate
-----------------------------------
- On first run on a fresh VM, an agent should:
  1. Clone this repo.
  2. Run `bash scripts/bootstrap.sh` to install Node + dependencies.
  3. Prompt the human (or a secrets manager) to provide `VITE_SUPABASE_URL` and `VITE_SUPABASE_PUBLISHABLE_KEY` (write to `.env.local`).
  4. Start dev server `npm run dev` and run smoke tests (or run `npm run test`).
  5. If asked to update production envs, use `scripts/update-vercel-envs.sh` (requires Vercel token & project id) — do NOT store tokens in repo.

- For PR automation: ensure the CI workflow is triggered on PRs. If tests fail, post the failing logs to the PR thread.

Security & operational notes
----------------------------
- Secrets must never be committed. Use `.env.local` for local development and Vercel environment variables for deployments.
- The historical `.env` exposure was removed, but a rotate of the Supabase publishable key is required to ensure no leak persists.
- After the history rewrite, all collaborators must re-clone. Avoid force-pulling.

Troubleshooting quick hits
--------------------------
- If `npm` is not installed, `scripts/bootstrap.sh` installs nvm and Node LTS. If the environment blocks installs, follow `DEV_SETUP.md` manual steps.
- If Vercel API calls fail, verify `VERCEL_TOKEN` has the right scopes (Project > Environment Variable management) and check `VERCEL_PROJECT_ID`.
- If preview deployments don't appear for PRs, check Vercel Git integration and branch settings (preview deployments enabled) and that Vercel has the env vars set.

Where to find logs and status
-----------------------------
- CI logs: GitHub Actions (PR -> Checks)
- Vercel previews: PR page (Vercel bot comment) or Vercel dashboard
- Local dev: terminal output from `npm run dev`

If you (the agent) need further action
-------------------------------------
- Paste any failing terminal logs here (first ~120 lines). I will analyze and produce precise fixes.
- If you need me to run privileged API calls (Vercel), run `scripts/update-vercel-envs.sh` locally with secure env vars rather than pasting tokens into chat.

---
End of context.
