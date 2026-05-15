# claude-ai-system-backup
Emergency backup of the full HMZ Claude AI system — restore everything on a new machine in minutes

![Backup](https://img.shields.io/badge/Backup-Auto_Daily-brightgreen?style=flat&labelColor=555)
![Claude](https://img.shields.io/badge/Claude-Code-cc785c?style=flat&labelColor=555)
![macOS](https://img.shields.io/badge/macOS-Apple_Silicon-black?style=flat&labelColor=555&logo=apple)
![Scripts](https://img.shields.io/badge/Scripts-80+-blue?style=flat&labelColor=555)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat&labelColor=555)

[Concepts](#-concepts) · [How It Works](#️-how-it-works) · [Restore](#-restore-on-new-machine) · [What's Backed Up](#-whats-backed-up) · [Tips](#-tips-and-tricks-8) · [Startups](#️-startups--businesses)

---

## 🧠 CONCEPTS

| Feature | Location | Description |
|---------|----------|-------------|
| [**Daily Sync**](../claude-ai-system/scripts/daily-sync-repos.sh) | `daily-sync-repos.sh` | 11 PM LaunchAgent — syncs all new files to GitHub |
| [**Fresh Install**](../claude-ai-system/scripts/fresh-machine-install.sh) | `fresh-machine-install.sh` | 1-command restore on any new Mac |
| [**80+ Bin Scripts**](../claude-ai-system/bin/) | `claude-ai-system/bin/` | All automation scripts version-controlled |
| [**17 Skills**](../claude-ai-system/skills/) | `claude-ai-system/skills/` | All Claude skills backed up |
| [**52 Repos**](../claude-ai-system/repos/) | `REPOS_MANIFEST.md` | Every HMZ repo catalogued with install commands |
| [**LaunchAgents**](launchagents/) | `launchagents/` | All macOS daemon plists backed up |

### 🔥 Hot

| Feature | Location | Description |
|---------|----------|-------------|
| [**One-command restore**](scripts/fresh-machine-install.sh) | `fresh-machine-install.sh` | Restores entire system including Homebrew, Ollama, skills |
| [**Auto-backup 11 PM**](../library/launchagents/) | LaunchAgent | Never manually push — everything auto-syncs nightly |
| [**52 repos manifest**](repos/REPOS_MANIFEST.md) | `REPOS_MANIFEST.md` | Complete catalogue with `gh repo clone` commands |

---

## ⚙️ HOW IT WORKS

```
11:00 PM — com.hmz.daily-repo-sync fires
         ↓
daily-sync-repos.sh runs:
  1. Scan ~/.claude/bin/ → copy new/updated files to claude-ai-system/bin/
  2. Scan ~/.claude/skills/ → copy to claude-ai-system/skills/
  3. Update REPOS_MANIFEST.md with all 52+ repos
  4. Detect new local dirs with READMEs → auto-create GitHub repos
  5. Git commit + push claude-ai-system
         ↓
Everything on GitHub → safe from machine failure
```

---

## 🔄 RESTORE ON NEW MACHINE

```bash
# Step 1: Install prerequisites (5 min)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git gh node python3 ollama

# Step 2: Auth GitHub
gh auth login

# Step 3: One-command restore (10 min)
bash <(curl -fsSL https://raw.githubusercontent.com/hmzainjamil/claude-ai-system/main/scripts/fresh-machine-install.sh)

# Step 4: Restore API keys
# Edit ~/.claude/tier0.env with your keys
```

**Total restore time: ~15 minutes**

---

## 📦 WHAT'S BACKED UP

| Category | Count | Location |
|---|---|---|
| Bin scripts | 80+ | `claude-ai-system/bin/` |
| Claude skills | 17 | `claude-ai-system/skills/` |
| Agent definitions | 210 | `claude-ai-system/agents/` |
| GitHub repos | 52 | `REPOS_MANIFEST.md` |
| LaunchAgents | 14 | Manifest in `launchagents/` |
| Workflows | 8,159 | n8n workflow JSONs |

**NOT backed up (manual):** API keys · Browser cookies · Local databases

---

## 💡 TIPS AND TRICKS (8)

[restore](#tips-restore) · [backup](#tips-backup) · [verify](#tips-verify) · [keys](#tips-keys)

<a id="tips-restore"></a>■ **Restore (2)**

| Tip | Source |
|-----|--------|
| Test fresh install monthly on a VM — verify it actually works before you need it | [HMZ](https://github.com/hmzainjamil) |
| Keep `tier0.env` in 1Password — only thing not on GitHub | [HMZ](https://github.com/hmzainjamil) |

<a id="tips-backup"></a>■ **Backup (2)**

| Tip | Source |
|-----|--------|
| Daily sync at 11 PM — if you work past midnight, trigger manually: `bash ~/.claude/bin/daily-sync-repos.sh` | [HMZ](https://github.com/hmzainjamil) |
| Check sync log: `tail -20 /tmp/daily-repo-sync.log` — verify last night's push | [DigiMinds](https://github.com/hmzainjamil) |

<a id="tips-verify"></a>■ **Verify (2)**

| Tip | Source |
|-----|--------|
| `gh repo list hmzainjamil --limit 100 \| wc -l` — verify repo count after sync | [HMZ](https://github.com/hmzainjamil) |
| `ls ~/.claude/bin \| wc -l` on new machine after restore — should match source | [HMZ](https://github.com/hmzainjamil) |

<a id="tips-keys"></a>■ **API Keys (2)**

| Tip | Source |
|-----|--------|
| Never commit tier0.env — it's in .gitignore, verify before every push | [HMZ](https://github.com/hmzainjamil) |
| Rotate keys after any security incident — revoke + regenerate all Tier 0 keys | [HMZ](https://github.com/hmzainjamil) |

---

## ☠️ STARTUPS / BUSINESSES

| This Repo / Feature | Replaced |
|-|-|
| **Daily auto-backup** | [Dropbox](https://dropbox.com), [iCloud](https://icloud.com), [Backblaze](https://backblaze.com) |
| **One-command restore** | [Ansible playbooks](https://ansible.com), [Chef](https://chef.io), [Puppet](https://puppet.com) |
| **Repo manifest** | [Confluence](https://atlassian.com/confluence), [Notion](https://notion.so) system docs |
| **Version-controlled scripts** | [Dotfiles managers](https://dotfiles.github.io) — GNU Stow, Chezmoi |

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/claude-ai-system-backup&type=Date)](https://star-history.com/#hmzainjamil/claude-ai-system-backup&Date)

---

<div align="center">
Built by <a href="https://github.com/hmzainjamil">HMZ</a> · Everything backed up, nothing lost
</div>
