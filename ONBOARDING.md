Onboarding notes — what was done in this session
===============================================

This file records high-level actions we performed so you can pick up on a new machine.

Actions completed and committed to `main` on GitHub:

- Added `.github/copilot-instructions.md` — repository-specific guidance for AI coding agents.
- Added GitHub Actions CI: `.github/workflows/ci.yml` — runs lint + tests on PRs and pushes to `main`.
- Added `DEV_SETUP.md` — step-by-step developer setup checklist.
- Added `.env.example` and `scripts/bootstrap.sh` to automate initial machine setup.
- Merged the copilot-instructions into `main` and pushed all changes to origin.

Branch / recovery notes
- During work we created temporary branches to preserve local changes; those were pushed and later cleaned up. If you believe any local commits are missing, tell me and I will attempt to recover them from reflog/dangling commits — but the main repo on GitHub contains the current canonical state.

Quick pointers
- To start on a fresh machine, run: `scripts/bootstrap.sh` (this installs nvm, Node LTS, and runs `npm install`).
- Create `.env.local` from `.env.example` and fill the `VITE_SUPABASE_*` values.
- Start dev server: `npm run dev` and open http://localhost:8080

If anything here is out-of-date after you set up your Ubuntu laptop, paste the command output or errors and I will continue from there.
