# 🧙 GPE Developer Agent

Custom agent instructions for Global Portfolio Engine ETF & Crypto project.

## 🎯 Role

You are an expert full-stack developer specializing in:
- JavaScript/TypeScript (Vanilla JS, no frameworks)
- Portfolio Management Systems
- Financial Calculations & Trading Strategies
- Browser-based Applications (IndexedDB, localStorage)
- Telegram Bot Integration
- GitHub Actions & CI/CD

## 📁 Project Structure

```
global-portfolio-engine-etf-crypto/
├── index.html          # Main application (single-file app)
├── .clinerules/       # Project configuration
│   ├── rule-token-optimize.md
│   ├── workflow.md
│   ├── memory-bank/
│   │   └── progress.md
│   └── developer-agent.md  # This file
├── AGENTS.md          # Global agent instructions (RTK)
└── README.md
```

## 🔧 Development Commands

### Git Workflow
```bash
# Always use rtk prefix for token optimization
rtk git status
rtk git add .
rtk git commit -m "<type>: <description>"
rtk git push

# Commit types: feat, fix, refactor, docs, chore
```

### GitHub Project Board Commands
```bash
# View all project items
gh project item-list 2 --owner godsid

# View specific item
gh project item-view <item-id> --owner godsid
```

### GitHub Issue Workflow
```bash
# After completing a task:
# 1. Update GitHub Issue status to "In Review"
# 2. Move issue to "In Review" column on Project Board

# Using GitHub API (if gh CLI not available):
curl -s "https://api.github.com/repos/godsid/global-portfolio-engine-etf-crypto/issues?state=all"
curl -X PATCH "https://api.github.com/repos/godsid/global-portfolio-engine-etf-crypto/issues/4" \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <token>" \
  -d '{"state":"closed","labels":["done"]}'
```

### Version Bumping (SemVer)
```bash
# PATCH: Bug fixes
# MINOR: New features
# MAJOR: Breaking changes
```

### Testing
```bash
# Open in browser
start index.html
```

## 🏗️ Architecture Guidelines

### Data Storage
- **IndexedDB**: Large data (snapshots, historical data)
- **localStorage**: Small settings, tokens (encrypted)
- **File Export (.gpe)**: Portfolio backup/restore

### Key Classes/Managers
| Class | Purpose |
|-------|---------|
| `PortfolioManager` | Core portfolio operations |
| `PriceEngine` | Fetch & cache prices |
| `StrategyEngine` | DCA, VH, GAP strategies |
| `SnapshotManager` | Daily NAV snapshots |
| `TelegramScheduler` | Scheduled reports |
| `DailyReportGenerator` | P&L reports |
| `RebalanceTargetManager` | Target allocation |
| `CryptoUtils` | Encryption (AES-GCM with PBKDF2) |

### Coding Standards
1. **No external dependencies** - Vanilla JS only
2. **Single HTML file** - All code in index.html
3. **IndexedDB over localStorage** - For data >5MB
4. **Encrypt sensitive data** - Tokens, keys
5. **Error handling** - Try-catch with user feedback

## 🔒 Security Rules

1. Never commit: API keys, tokens, passwords
2. Encrypt sensitive data before localStorage
3. Use HTTPS for all API calls
4. Validate all user inputs

## 📊 Task Priorities

| Priority | Symbol | Description |
|----------|--------|-------------|
| High | 🔴 | Critical bugs, security issues |
| Medium | 🟡 | New features, improvements |
| Low | 🟢 | Nice-to-have, cleanup |

## 📋 GitHub Project Board

**Project:** @godsid's Portfolio Tracking  
**URL:** https://github.com/users/godsid/projects/2

### Current Tasks (from Project Board)
| # | Title | Type | Repository |
|---|-------|------|------------|
| 1 | Telegram Bot Testing | Issue | global-portfolio-engine-etf-crypto |
| 2 | Push Notification for Portfolio Alerts | Issue | global-portfolio-engine-etf-crypto |
| 3 | Snapshot cleanup old data | Issue | global-portfolio-engine-etf-crypto |
| 4 | Replace XOR with Web Crypto API (AES-GCM) | Issue | global-portfolio-engine-etf-crypto |

## 🚀 Quick Start

1. Read `.clinerules/memory-bank/progress.md` for current status
2. Check open GitHub issues: `rtk gh issue list`
3. Check Project Board: `gh project item-list 2 --owner godsid`
4. Implement features following workflow rules
5. Test in browser (`start index.html`)
6. Commit with proper prefix

## 💡 Tips

- Use `rtk` prefix for all commands (60-90% token savings)
- Break tasks into small PRs
- Update progress.md after each milestone
- Use markdown for documentation
