Developer setup & quick checklist
================================

This checklist is tuned for a fresh Ubuntu/macOS developer machine. It covers installing tooling, cloning the repo, running the dev server, and connecting to Vercel.

1) Install core tooling

 - Git, curl, build tools
   - Ubuntu: sudo apt update && sudo apt install -y git curl build-essential
   - macOS (Homebrew): brew install git curl

 - Install nvm (recommended) and Node LTS
   ```bash
   curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.6/install.sh | bash
   # then reopen shell or source nvm
   nvm install --lts
   nvm use --lts
   node -v && npm -v
   ```

 - (Optional) GitHub CLI and Vercel CLI
   ```bash
   # GitHub CLI
   sudo snap install gh --classic   # or follow platform instructions
   gh auth login

   # Vercel CLI (optional)
   npm install -g vercel
   vercel login
   ```

2) Clone and install

```bash
git clone git@github.com:therealshaun/blitzbuyai.git
cd blitzbuyai
npm install
```

3) Environment variables (local only)

Create a `.env.local` (do NOT commit). The app expects client-side VITE_* vars:

```
VITE_SUPABASE_URL=https://<your-supabase>.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=public-anon-xxx
```

4) Run dev server

```bash
npm run dev
# open http://localhost:8080
```

5) Quick quality checks

```bash
npm run lint       # eslint
npm run test       # vitest
npm run build:dev  # quick dev build
```

6) Vercel / Deploys

- Ensure your Vercel project is connected to this GitHub repo and `main` is the production branch.
- Add the same client env vars to Vercel (Preview & Production): `VITE_SUPABASE_URL`, `VITE_SUPABASE_PUBLISHABLE_KEY`.
- Push a feature branch and open a PR — Vercel will create a preview deployment for that PR.

7) Recommended workflow

- Work on a feature branch, push, create a PR. Vercel preview will let you test the change before merging to main.
- CI runs on PRs (lint + tests). Merge only when green.

If you want help wiring Vercel env vars or a vercel.json, say so and I will prepare exact commands or files.
