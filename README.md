# skill-sync
[![GitHub release](https://img.shields.io/github/v/release/wan-huiyan/skill-sync)](https://github.com/wan-huiyan/skill-sync/releases) [![Claude Code](https://img.shields.io/badge/Claude_Code-skill-orange)](https://claude.com/claude-code) [![license](https://img.shields.io/github/license/wan-huiyan/skill-sync)](LICENSE) [![last commit](https://img.shields.io/github/last-commit/wan-huiyan/skill-sync)](https://github.com/wan-huiyan/skill-sync/commits)

After improving skills locally across multiple sessions, pushing changes back to GitHub means cloning, diffing, and copying files for every skill. This skill automates that for your entire published portfolio at once.

## Quick Start

```
You: /skill-sync
Claude: Scanning 18 registered skills...

Skill                    Repo                              Type      Status
---
causal-impact-campaign   wan-huiyan/causal-impact-campaign  authored  IN SYNC
permutation-validation   wan-huiyan/permutation-validation  authored  DIRTY (SKILL.md)
field-notes              wan-huiyan/field-notes             fork      IN SYNC

You: /skill-sync push permutation-validation
Claude: Cloning wan-huiyan/permutation-validation...
        Copying SKILL.md...
        Committed and pushed: "docs: sync permutation-validation from local skill updates"
```

## Installation

**Claude Code:**
```bash
git clone https://github.com/wan-huiyan/skill-sync.git ~/.claude/skills/skill-sync
```

**Cursor:**
```bash
git clone https://github.com/wan-huiyan/skill-sync.git ~/.cursor/skills/skill-sync
```

## What You Get

| Command | What it does |
|---|---|
| `/skill-sync` | Scan all registered skills, show sync status table |
| `/skill-sync init` | Populate registry by matching `gh repo list` against local `~/.claude/skills/` |
| `/skill-sync push` | Push all dirty authored skills to their GitHub repos |
| `/skill-sync push <name>` | Push a single skill |

## How It Works

1. **Registry** (`~/.claude/skill-sync-registry.json`) maps local skill directories to GitHub repos
2. **Init** scans your GitHub repos via `gh repo list`, matches against installed skills, detects forks vs authored
3. **Status** clones each repo to `/tmp`, diffs tracked files against local copies
4. **Push** copies local files to cloned repo, commits, pushes, cleans up

## Companion Skills

- **[publish-skill](https://github.com/wan-huiyan/publish-skill)** — First-time publishing (repo creation, README, screenshots, awesome-list). After publishing, run `/skill-sync init` to register.
- **[schliff](https://github.com/Zandereins/schliff)** — Score and improve skill quality before syncing.
- **[skill-anonymizer](https://github.com/wan-huiyan/skill-anonymizer)** — Scan for client-identifying data before pushing. New content added locally may introduce client names, amounts, or domain-specific details.
- **[data-provenance-verifier](https://github.com/wan-huiyan/data-provenance-verifier)** — Verify any new data files have provenance documentation before pushing.

## Key Design Decisions

- **Registry over auto-detection**: A JSON registry is explicit and editable, vs scanning `.git/config` or `plugin.json` which may be stale or missing
- **Clone-copy-push over local git**: Skills installed via `git clone` may have dirty working trees, detached HEADs, or stale remotes. Cloning fresh to `/tmp` avoids all git state issues
- **Authored vs fork distinction**: Forks get a warning ("consider opening an upstream PR") instead of silent pushes that diverge from upstream
- **Version bump checklist absorbed from publish-skill**: Keeps all update-related guidance in one place instead of split across two skills

## Limitations

- Requires `gh` CLI authenticated with push access to your repos
- Does not auto-detect new skills — run `/skill-sync init` after using `/publish-skill`
- Does not handle merge conflicts — if the remote has changes not in your local copy, push will fail and you'll need manual resolution
- Registry is per-machine — not synced across devices

## Dependencies

- **Required:** `gh` CLI (GitHub CLI) — for repo listing, cloning, pushing
- **Required:** `git` — for clone, diff, commit, push operations
- **Optional:** `jq` — for registry JSON manipulation (falls back to Python)

<details>
<summary>Quality Checklist</summary>

- [x] Detects all skills with matching GitHub repos
- [x] Distinguishes authored vs forked skills
- [x] Shows per-file diff status (not just "dirty")
- [x] Copies to both root and `skills/{name}/` paths in repo
- [x] Includes version bump checklist for major updates
- [x] Warns about upstream PRs for forked skills
- [x] Cleans up `/tmp` clones after operations
</details>

## Related Skills

- [publish-skill](https://github.com/wan-huiyan/publish-skill) — First-time skill publishing (skill-sync is its companion for ongoing maintenance)
- [schliff](https://github.com/Zandereins/schliff) — Score and improve skill quality before syncing
- [skill-anonymizer](https://github.com/wan-huiyan/skill-anonymizer) — Scan for client-identifying data before pushing updates
- [data-provenance-verifier](https://github.com/wan-huiyan/data-provenance-verifier) — Verify data files have provenance documentation

## Version History

| Version | Date | Changes |
|---|---|---|
| 1.0.0 | 2026-04-02 | Initial release: init, status, push commands. Registry format. Version bump checklist absorbed from publish-skill. |

## License

MIT
