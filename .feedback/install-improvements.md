# Installation Improvements (User Feedback)

**Date**: 2026-03-19 03:53 UTC

## Feature Request

During installation, **prompt user for delivery preferences**:

1. **channel**: Which channel to use for task announcements? (default: last/dingtalk-connector)
2. **account**: Which bot/account should send the message? (default: default account)
3. **recipient**: Which user/group should receive the task summaries? (default: main user ID)

### Rationale
- Different users may want reports sent to different channels (Telegram, WeCom, etc.)
- Multi-account setups may prefer a specific bot for consolidated reports
- Avoid hardcoded values; make installer interactive/inferred

### Implementation Ideas
- Interactive prompts if CLI detected (tty)
- Or accept CLI args: `--channel`, `--account`, `--to`
- Fallback to auto-detection (current behavior) if not specified

---

*This file tracks user-suggested improvements for future versions.*
