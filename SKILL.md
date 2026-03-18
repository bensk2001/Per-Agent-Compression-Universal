# Per-Agent Memory Compression (Universal)

> Zero-config memory consolidation for multi-agent OpenClaw deployments

## Summary

This skill automatically sets up weekly memory compression tasks for all agents in your OpenClaw deployment. Each agent compresses its own daily notes into long-term memory files (USER.md, IDENTITY.md, SOUL.md, MEMORY.md) with domain-specific extraction.

No manual configuration required—just run the installer and it auto-discovers all agents and creates staggered cron tasks.

## Features

- ✅ Auto-discovery of all agents via `openclaw agents list --json`
- ✅ Workspace isolation (each agent compresses its own memory)
- ✅ State persistence with checkpoint resilience (`.compression_state.json`)
- ✅ Deduplication to avoid double-processing
- ✅ Domain-tailored extraction (general, HR, parenting, decoration)
- ✅ Summary notifications via DingTalk connector
- ✅ Staggered weekly schedule (Sundays 03:00-04:30)
- ✅ Configurable via `alert_rules.json`, `topic_keywords.json`, `user_map.json`

## Installation

```bash
# Copy the skill to your skills directory
cp -r per-agent-compression-universal /root/.openclaw/skills/

# Run the installer from the skill directory
cd /root/.openclaw/skills/per-agent-compression-universal
./install.sh
```

The installer will:
- Verify your agent list
- Create a cron task for each discovered agent
- Set up necessary directories and state files
- Validate memory paths

**No parameters needed**—all settings inferred from your OpenClaw configuration.

## Uninstallation

```bash
./uninstall.sh
```

Removes all `per_agent_compression_*` cron tasks created by this skill.

## Configuration

### Agent-Specific Behavior

Each agent's task uses a tailored `DOMAIN_CONTEXT`:
- `main`: General-purpose extraction (USER.md, IDENTITY.md, SOUL.md, MEMORY.md)
- `hrbp`: Focus on work-related preferences, decisions, HR policies
- `parenting`: Parenting themes (sleep, feeding, health, development)
- `decoration`: Renovation-related insights (design, materials, quotes)

### Rule Files

Placed in each agent's workspace `shared_memory/`:

- `topic_keywords.json` – Keyword-to-topic mapping for categorization
- `alert_rules.json` – Alert frequency and limits
- `alert_state.json` – Cooldown tracking (auto-maintained)
- `user_map.json` – Platform user ID to person nicknames

These files follow the same format as the shared memory system described in `PROMPT_FULL_SHARED_MEMORY.md`.

### Timeouts

Default task timeout: 1200s (20 minutes). Adjust via `openclaw cron edit <id> --timeout-seconds <seconds>` if you have many notes.

## Limitations

- Requires `self-improve-agent` for full automation (optional).
- CLI message length limit may require manual `cron edit --message` for fully detailed payloads.
- No per-agent install filter—auto-discovers all agents. To limit, edit `install.sh`.
- Not yet optimized for extremely large note sets (>500 daily notes per run).

## Support

See `README.md` for detailed architecture, troubleshooting, and upgrade notes.

## License

MIT-0

## Version

1.3.2 (2026-03-18)
