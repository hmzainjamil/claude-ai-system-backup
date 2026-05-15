# claude-ai-system-backup

Master backup of entire HMZ Claude AI system: bin scripts, skills, agents, hooks, memory, and workflow configs.

![Backup](https://img.shields.io/badge/Backup-Complete-blue?style=flat&labelColor=555) ![Claude](https://img.shields.io/badge/Claude-AI_System-green?style=flat&labelColor=555) ![Size](https://img.shields.io/badge/Skills-500+-orange?style=flat&labelColor=555) ![License](https://img.shields.io/badge/License-MIT-yellow?style=flat&labelColor=555)

[Concepts](#-concepts) · [How It Works](#-how-it-works) · [Install](#-install) · [Usage](#-usage) · [Config](#-configuration) · [Tips](#-tips-and-tricks-12) · [Troubleshooting](#-troubleshooting) · [Architecture](#-architecture) · [Startups](#️-startups--businesses)

---

## 🧠 CONCEPTS

| Feature | Location | Description |
|---|---|---|
| Bin Scripts | `bin/` | All `~/.claude/bin/` executables: speckit, mae, tcc, etc. |
| Skills Library | `skills/` | 500+ skill SKILL.md files with prompts and configs |
| Agent Configs | `agents/` | Hermes, G0DM0D3, MAE agent definitions |
| Hook Configs | `hooks/` | PreToolUse, PostToolUse, Stop hook scripts |
| Memory Snapshots | `memory/` | Versioned MEMORY.md + individual feedback files |
| MCP Configs | `mcp/` | `.mcp.json` with all connected MCP servers |
| CLAUDE.md | `CLAUDE.md` | Global Claude Code instructions — master prompt |
| Settings | `settings/` | `settings.json` + `settings.local.json` backups |
| Workflow Configs | `workflows/` | MAE, TCC, daily automation workflow definitions |
| Backup Script | `backup.sh` | One-command full system backup to this repo |
| Restore Script | `restore.sh` | One-command restore from this repo |
| Version Manifest | `MANIFEST.json` | Checksums + timestamps for all backed-up files |

### 🔥 Hot

| Feature | Location | Description |
|---|---|---|
| restore.sh | `restore.sh` | Full system restore in <5 min on fresh Mac |
| CLAUDE.md | `CLAUDE.md` | Master prompt — most valuable single file in system |
| Skills Library | `skills/` | 500+ accumulated skills lost without this backup |
| Memory Snapshots | `memory/` | Months of accumulated context — irreplaceable |
| Hook Configs | `hooks/` | Automated behaviors re-established immediately on restore |

---

## ⚙️ HOW IT WORKS

```
~/.claude/              (live system)
    │
    ▼ (backup.sh runs daily via LaunchAgent)
claude-ai-system-backup/
    ├── bin/            ← copy of ~/.claude/bin/
    ├── skills/         ← copy of ~/.claude/skills/
    ├── agents/         ← copy of ~/.claude/agents/
    ├── hooks/          ← copy of ~/.claude/hooks/
    ├── memory/         ← copy of ~/.claude/memory/
    ├── mcp/            ← copy of ~/.mcp.json
    ├── CLAUDE.md       ← copy of ~/.claude/CLAUDE.md
    └── MANIFEST.json   ← checksums + timestamps

git add -A && git commit -m "backup: $(date +%Y-%m-%d)"
git push origin main
```

**Restore flow:**
```
git clone hmzainjamil/claude-ai-system-backup
bash restore.sh  # copies all files back to ~/.claude/
```

---

## 🚀 INSTALL

```bash
git clone https://github.com/hmzainjamil/claude-ai-system-backup
cd claude-ai-system-backup

# Make scripts executable
chmod +x backup.sh restore.sh

# Run initial backup of current system
bash backup.sh

# Schedule daily backup via LaunchAgent (macOS)
cp launchagents/com.hmz.claude-backup.plist ~/Library/LaunchAgents/
launchctl load ~/Library/LaunchAgents/com.hmz.claude-backup.plist

# Verify backup
python3 verify_manifest.py
```

---

## 📟 USAGE

```bash
# Full backup now
bash backup.sh

# Restore everything to ~/.claude/
bash restore.sh

# Restore only skills
bash restore.sh --only skills

# Restore only memory
bash restore.sh --only memory

# Restore only CLAUDE.md
bash restore.sh --only claude-md

# Verify backup integrity
python3 verify_manifest.py

# Show what changed since last backup
python3 diff_manifest.py

# List all backed-up skills
ls skills/ | wc -l

# Search backed-up memory
grep -r "keyword" memory/
```

---

## ⚙️ CONFIGURATION

| Variable | Default | Description |
|---|---|---|
| `BACKUP_SOURCE` | `~/.claude/` | Source directory to back up |
| `BACKUP_DEST` | `./` | Destination in this repo |
| `BACKUP_SCHEDULE` | `06:00` | LaunchAgent run time |
| `GIT_AUTO_PUSH` | `true` | Auto-push after backup |
| `INCLUDE_SECRETS` | `false` | Never backup `.env` files |
| `MANIFEST_ALGO` | `sha256` | Checksum algorithm |
| `MAX_MEMORY_SIZE_MB` | `50` | Alert if memory folder exceeds this |
| `BACKUP_REMOTE` | `origin` | Git remote to push to |
| `NOTIFY_ON_FAILURE` | `true` | Notify if backup fails |

---

## 💡 TIPS AND TRICKS (12)

[Backup](#tips-backup) · [Restore](#tips-restore) · [Memory](#tips-memory) · [Security](#tips-security)

<a id="tips-backup"></a>■ **Backup Strategy (3)**

| Tip | Source |
|---|---|
| Run `backup.sh` before any major Claude Code session — state can drift fast | Backup guide |
| Use `diff_manifest.py` to see what changed — pinpoints regression source | Manifest docs |
| `MANIFEST.json` checksums let you detect corrupted files before restoring them | Verify script |

<a id="tips-restore"></a>■ **Restore Strategy (3)**

| Tip | Source |
|---|---|
| Always `restore.sh --only memory` first — memory context is most valuable | Restore guide |
| `restore.sh --dry-run` previews what will be overwritten — no surprises | Restore flags |
| On fresh Mac: clone this repo first, run restore.sh — system live in 5 min | Setup guide |

<a id="tips-memory"></a>■ **Memory Management (3)**

| Tip | Source |
|---|---|
| Memory snapshots are dated — restore any point-in-time state | Memory docs |
| `grep -r "project" memory/` faster than re-reading all files | Shell tips |
| Keep memory files under 500 lines each — large files slow RAG retrieval | Memory guide |

<a id="tips-security"></a>■ **Security (3)**

| Tip | Source |
|---|---|
| `.gitignore` must include `.env`, `*.key`, `api_keys.*` — never commit secrets | Security guide |
| Use private GitHub repo for this backup — contains system prompts | GitHub docs |
| `verify_manifest.py` detects tampering — run after any remote restore | Manifest docs |

---

## 🔧 TROUBLESHOOTING

| Issue | Fix |
|---|---|
| Backup fails: permission denied | `chmod -R u+rw ~/.claude/` |
| Git push fails | Check SSH key: `ssh -T git@github.com` |
| Restore overwrites wrong files | Use `--only` flag to restore specific components |
| Manifest checksum mismatch | File corrupted — restore from previous git commit |
| LaunchAgent not running | `launchctl list | grep claude-backup` |
| Skills missing after restore | Check `ls skills/ | wc -l` matches expected count |
| CLAUDE.md not loading | Verify path: `~/.claude/CLAUDE.md` exists |

---

## 📊 ARCHITECTURE

```
claude-ai-system-backup/
├── bin/                    # ~/.claude/bin/ — all executable scripts
├── skills/                 # ~/.claude/skills/ — 500+ skill definitions
├── agents/                 # Agent YAML configs
├── hooks/                  # Claude Code hook scripts
├── memory/
│   ├── MEMORY.md           # Master memory index
│   └── *.md                # Individual memory files
├── mcp/
│   └── mcp.json            # MCP server connections
├── workflows/              # MAE/TCC workflow configs
├── settings/
│   ├── settings.json       # Claude Code settings
│   └── settings.local.json
├── launchagents/
│   └── com.hmz.claude-backup.plist
├── CLAUDE.md               # Master global instructions
├── MANIFEST.json           # Checksums + timestamps
├── backup.sh               # Full system backup
├── restore.sh              # Full system restore
├── verify_manifest.py      # Integrity verification
└── diff_manifest.py        # Change detection
```

---

## ☠️ STARTUPS / BUSINESSES

| This Repo / Feature | Replaced |
|---|---|
| Full system backup | Months of skill building lost on OS reinstall |
| One-command restore | Multi-day recovery from scratch |
| Memory snapshots | Session context lost permanently on wipe |
| CLAUDE.md backup | Recreating master prompt from memory |
| Hook config backup | Re-implementing all automation behaviors |
| Manifest verification | No way to detect system drift |
| LaunchAgent scheduling | Manual backup discipline (always fails) |
| Versioned history | No rollback if a config change breaks things |

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/claude-ai-system-backup&type=Date)](https://star-history.com/#hmzainjamil/claude-ai-system-backup&Date)

---
<div align="center">Built by <a href="https://github.com/hmzainjamil">HMZ</a> · Part of HMZ Claude AI System</div>

---

## 🔬 BACKED-UP SYSTEM COMPONENTS

| Component | Count | Size (approx) |
|---|---|---|
| Skills | 500+ | ~15 MB |
| Bin scripts | 20+ | ~500 KB |
| Memory files | 30+ | ~200 KB |
| Agent configs | 10+ | ~100 KB |
| Hook scripts | 5 | ~50 KB |
| Workflow configs | 10+ | ~200 KB |
| Prompt templates | 50+ | ~2 MB |

---

## 🔐 WHAT IS EXCLUDED FROM BACKUP

These are gitignored and never backed up:

```
.env
.env.local
*.key
api_keys.*
chroma_db/
*.sqlite
session-queue.jsonl (processed to memory/)
```

Secrets are managed separately via 1Password or macOS Keychain.

---

## 📅 BACKUP SCHEDULE

| Trigger | Action |
|---|---|
| Daily 06:00 | Full backup via LaunchAgent |
| Before major session | `bash backup.sh` (manual) |
| After installing new skill | Automatic via PostInstall hook |
| After CLAUDE.md change | Automatic via file watcher |
| Weekly | Verify manifest integrity |

---

## 🔄 VERSION HISTORY USAGE

```bash
# See all backup commits
git log --oneline

# Restore system from 7 days ago
git checkout HEAD~7 -- skills/ memory/ CLAUDE.md
bash restore.sh --from-checkout

# Compare current vs 30 days ago
git diff HEAD~30 memory/MEMORY.md
```
