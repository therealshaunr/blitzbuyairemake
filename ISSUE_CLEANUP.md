Title: Remove exposed .env and rotate Supabase key — cleanup performed

Summary
-------
I removed a tracked `.env` file from the repository and purged it from Git history. A backup branch `backup-before-filter` was created containing the previous history.

What happened
--------------
- `.env` was removed from the index and `.gitignore` was updated.
- Then the repository history was rewritten to remove `.env` from all past commits and the cleaned history was force-pushed to `main`.
- A backup branch `backup-before-filter` was pushed to `origin` containing the pre-filter state.

Required actions for all collaborators
-------------------------------------
1. Re-clone the repository (recommended):

```bash
cd ~
rm -rf blitzbuyai
git clone git@github.com:therealshaun/blitzbuyai.git
cd blitzbuyai
```

2. Rotate the Supabase publishable key immediately via the Supabase dashboard (Settings → API → Keys).

3. Update Vercel environment variables for the project (Preview & Production) with the new key and the existing VITE_SUPABASE_URL.
   - Option A (manual): Update via Vercel web UI.
   - Option B (scripted): run `scripts/update-vercel-envs.sh` with `VERCEL_TOKEN` and `VERCEL_PROJECT_ID` environment variables.

Example (scripted):

```bash
export VERCEL_TOKEN="<your-vercel-token>"
export VERCEL_PROJECT_ID="<your-project-id>"
export NEW_SUPABASE_URL="https://your.supabase.co"
export NEW_SUPABASE_KEY="public-..."
./scripts/update-vercel-envs.sh
```

Notes and rollback
------------------
- The backup branch `backup-before-filter` is available at `origin/backup-before-filter` if you need to recover anything.
- Rewriting history is disruptive: any local branches or forks must be updated. The easiest recovery is re-cloning.

If you want me to perform updates to Vercel (using an API token) or create an issue on GitHub summarizing this, tell me and I will proceed.
