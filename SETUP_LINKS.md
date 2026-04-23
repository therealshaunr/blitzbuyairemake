# Quick links & machine setup anchors

Use this file as a single pointer for automated agents or humans to bootstrap a new Ubuntu machine and fix common keyboard issues.

Raw scripts and docs (direct URLs you can curl/fetch):

- Bootstrap script (auto-installs nvm/node and deps):
  https://raw.githubusercontent.com/therealshaun/blitzbuyai/main/scripts/bootstrap.sh

- Developer quick-start checklist: (DEV_SETUP.md)
  https://raw.githubusercontent.com/therealshaun/blitzbuyai/main/DEV_SETUP.md

- Onboarding notes (what was done in this session):
  https://raw.githubusercontent.com/therealshaun/blitzbuyai/main/ONBOARDING.md

- Example env file (copy to `.env.local` and fill values):
  https://raw.githubusercontent.com/therealshaun/blitzbuyai/main/.env.example


How to use (agent/human)

1) Fetch and run the bootstrap script:

```bash
curl -fsSL https://raw.githubusercontent.com/therealshaun/blitzbuyai/main/scripts/bootstrap.sh -o bootstrap.sh
bash bootstrap.sh
```

2) Edit `.env.local` (created by the script) and fill in `VITE_SUPABASE_URL` and `VITE_SUPABASE_PUBLISHABLE_KEY`.

3) Start dev server:

```bash
npm run dev
# open http://localhost:8080
```


Quick keyboard fixes for Ubuntu (copy/paste)

If your keyboard layout, repeat rate, or missing keys are causing problems, try these commands. They apply immediately and survive until logout.

- Set layout to US (change `us` to your preferred layout code):

```bash
# temporary for current X session
setxkbmap -layout us

# GNOME persistent setting (makes 'us' primary input source)
gsettings set org.gnome.desktop.input-sources sources "[('xkb','us')]"
```

---

## Cline (Claude Code) Local AI Setup

This guide configures Cline to use local Ollama models as a Copilot alternative.

### Prerequisites

1. **Install Ollama**:
   ```bash
   brew install ollama
   ```

2. **Start Ollama**:
   ```bash
   ollama serve
   ```

3. **Pull models**:
   ```bash
   ollama pull codellama   # Code completion
   ollama pull llama3      # General chat
   ollama pull llava       # Vision (image analysis)
   ```

### Cline Configuration

Edit `~/.cline/settings.json` or use Cline's settings UI:

```json
{
  "apiProvider": "openai",
  "openaiApiKey": "not-needed",
  "openaiApiBase": "http://localhost:11434/v1",
  "openaiModelId": "llama3",
  "maxTokens": 4096,
  "temperature": 0.3
}
```

### Environment Variables

Add to `~/.zshrc`:

```bash
export OLLAMA_HOST="http://localhost:11434"
export OPENAI_API_KEY="not-needed"
export OPENAI_API_BASE="http://localhost:11434/v1"
```

### Performance Tips

| Model | Use Case | VRAM |
|-------|----------|------|
| codellama:7b | Code completion | ~8GB |
| mistral | General chat | ~4GB |
| llama3:8b | General chat | ~8GB |
| llava | Image analysis | ~8GB |
```

- Fix slow key repeat (make repeat faster):

```bash
gsettings set org.gnome.desktop.peripherals.keyboard repeat true
gsettings set org.gnome.desktop.peripherals.keyboard delay 250
gsettings set org.gnome.desktop.peripherals.keyboard repeat-interval 25
```

- If keys are swapped or wrong characters appear, try resetting XKB options:

```bash
setxkbmap -layout us -option ""  # clears XKB options
```

- For Wayland (GNOME on Wayland), use `gsettings` changes above; `setxkbmap` is X-only.

Helpful links (browser)

- GNOME input sources docs: https://help.gnome.org/users/gnome-help/stable/keyboard-layouts.html
- Ubuntu keyboard layout guide: https://help.ubuntu.com/stable/ubuntu-help/keyboard-layout.html.en


If you want, point your agent at this raw `scripts/bootstrap.sh` URL and the above keyboard commands. I can also add a one-line wrapper (e.g., `scripts/quick-ubuntu.sh`) that runs safe keyboard fixes automatically — say the word and I'll add it.
