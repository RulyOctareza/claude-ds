# claude-ds

Run **Claude Code** using **DeepSeek** models (R1, V4) via DeepSeek's Anthropic-compatible API endpoint.

Saves ~90% on API costs vs Anthropic direct, with no feature loss — same Claude Code CLI, different backend.

## Requirements

- [Claude Code](https://claude.ai/code) installed (`npm install -g @anthropic-ai/claude-code`)
- DeepSeek API key from [platform.deepseek.com](https://platform.deepseek.com)

## Install

```bash
# 1. Setup API key
cp key.env.example ~/.config/deepseek/key.env
chmod 600 ~/.config/deepseek/key.env
# Edit ~/.config/deepseek/key.env — paste your DeepSeek API key

# 2. Install the wrapper
cp claude-ds /usr/local/bin/claude-ds
chmod +x /usr/local/bin/claude-ds

# Or symlink instead:
# ln -sf "$PWD/claude-ds" ~/.local/bin/claude-ds
```

## Usage

```bash
claude-ds                    # Launch Claude Code via DeepSeek
claude-ds --bare             # Launch without hooks/plugins
claude-ds "some prompt"      # Run one-shot via stdin
```

## How It Works

The wrapper:

1. Reads `DEEPSEEK_API_KEY` from `~/.config/deepseek/key.env`
2. Sets env vars: `ANTHROPIC_BASE_URL` → `api.deepseek.com/anthropic`, model → `deepseek-v4-pro[1m]` / `deepseek-v4-flash`
3. Runs `claude --settings` with generated JSON config

No Anthropic login or OAuth needed — all traffic routes through DeepSeek's API.

## CI / Dev Containers

For automated environments, set these env vars directly instead of using `key.env`:

```bash
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_API_KEY="sk-..."
export ANTHROPIC_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
export CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
export CLAUDE_CODE_EFFORT_LEVEL="max"

claude --daemon  # or claude with your prompt
```

## Notes

- Model IDs: `deepseek-v4-pro[1m]` (heavy reasoning), `deepseek-v4-flash` (fast/light)
- Some Claude features (MCP server auth, Artifacts) may behave differently via DeepSeek
- See [DeepSeek docs](https://api-docs.deepseek.com/quick_start/agent_integrations/claude_code) for official integration guide
