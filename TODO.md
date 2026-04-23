Repository TODO (short)
=======================

1) Verify Vercel configuration
   - Ensure project is linked to this GitHub repo
   - Ensure Preview Deployments are enabled
   - Add env vars: VITE_SUPABASE_URL and VITE_SUPABASE_PUBLISHABLE_KEY (Preview & Production)

2) Validate CI runs on PRs
   - Create a feature branch, open a PR and confirm `.github/workflows/ci.yml` runs

3) Local dev
   - Use `scripts/bootstrap.sh` on your new Ubuntu machine to install Node and deps
   - Create `.env.local` from `.env.example` and run `npm run dev`

4) Integrate preserved local commits (optional)
   - If you want previous local work recovered, I can search and restore any dangling commits and open a PR from them.

5) Future enhancements (nice-to-have)
   - Add Vercel preview badge to README
   - Add lint/fix CI job that auto-formats code (prettier)
   - Add e2e / integration tests for critical flows
