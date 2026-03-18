请往下翻页查看中文说明

---

# Changelog

All notable changes to this skill will be documented in this file.

---

## [1.3.0] - 2026-03-18 (Security & Privacy Fix)

### Security
- **Removed hardcoded credentials**: Eliminated hardcoded DingTalk recipient ID from install script and documentation
- **Removed hardcoded model**: Eliminated fixed model reference from install script
- **Automatic configuration**: Installer now infers `--model`, `--channel`, and `--to` from the user's current OpenClaw configuration instead of using hardcoded values
- **Privacy protection**: No personal identifiers or specific model names are baked into the skill package

### Changed
- `install.sh`: Removed explicit `--model`, `--channel`, `--to`, and `--best-effort-deliver` parameters; now relies on OpenClaw defaults
- `README.md`: Updated task creation section to reflect automatic parameter inference
- All documentation: No longer exposes user-specific IDs or models

### Note
This is a critical security update. All users should upgrade to 1.3.0 to prevent credential leakage.

---

## [1.2.6] - 2026-03-18 (Version Unification)

### Changed
- Unified all version numbers across local, GitHub, and ClawHub to 1.2.6
- Consolidated all prior documentation refinements into single version

### Note
This release contains no functional changes. It is a version synchronization point. All features are as in v1.2.2.

---

## [1.2.5] - 2026-03-18 (Documentation Cleanup)

### Changed
- Removed intermediate version entries (1.2.3, 1.2.4) from CHANGELOG to avoid confusion
- Kept only essential version history (1.2.2 as final feature release)

### Fixed
- Ensured CHANGELOG structure follows rule: newest at top, oldest at bottom
- Verified bilingual separation across all documents

### Note
No functional changes. Documentation only.

---

## [1.2.4] - 2026-03-18 (Documentation Fix)

### Added
- Separated English and Chinese documentation correctly (CHANGELOG now has distinct sections)
- Version & release protocol added to AGENTS.md

### Changed
- CHANGELOG ordering: newest versions at top, oldest at bottom
- Documentation structure: clear English-first then Chinese-after-separator

### Fixed
- Fixed duplicate/incorrect version entries in CHANGELOG
- Ensured bilingual consistency across README and CHANGELOG

### Note
No functional changes. Documentation refinement only.

---

## [1.2.3] - 2026-03-18 (Documentation Update)

### Added
- Separated English and Chinese documentation in README and CHANGELOG for improved readability
- "Please scroll down for Chinese" notice at the top of each document
- Comprehensive version & release protocol added to AGENTS.md

### Changed
- Documentation structure: English content first, then full Chinese translation after separator
- README and CHANGELOG now fully bilingual with clear section separation

### Fixed
- None (documentation only)

### Note
This version was an intermediate documentation update. All changes are non-functional.

---

## [1.2.2] - 2026-03-18 (Final Feature Release)

### Added
- State persistence & checkpoint resilience - Each agent task maintains `.compression_state.json` to resume from interruptions
- Deduplication - Checks target files before appending to avoid duplicate entries from same daily note
- Remaining notes reporting - Summary includes count of old notes still pending for future runs
- Enhanced error handling - Individual note failures don't stop the entire task; errors logged and continue
- Moved-file marking - Processed notes moved to `memory/processed/` directory for clear separation
- Domain-specific extraction guidelines - Each task includes DOMAIN_CONTEXT to tailor extraction (general, HR/work, parenting, renovation)
- Pre-check validation - Script verifies agents list, workspace existence, and memory directory before registration
- Auto-discovery of all agents via `openclaw agents list --json`
- Staggered weekly scheduling (Sundays, 30-minute intervals starting 03:00)
- Workspace isolation - each agent compresses its own memory files
- Basic extraction of preferences, decisions, and personal information
- Markdown date headers for all appended entries
- Summary notifications via DingTalk connector
- Uninstall script to remove all `per_agent_compression_*` tasks
- Comprehensive README with troubleshooting guide

### Changed
- Task naming - Changed from `peragent_compression_` to `per_agent_compression_` for better readability
- Timeout increased - From 300s to 1200s to accommodate larger note sets
- Message payload enriched - Detailed execution plan with specific file paths, state structure, and date header format (`### [YYYY-MM-DD]`)
- Delivery mode - Uses `--best-effort-deliver` to ensure notifications are attempted even if partial failures occur

### Fixed
- State file path - Now properly defined as `{workspace}/memory/.compression_state.json`
- Processed directory - Explicitly created as `{workspace}/memory/processed/`
- Target sections - Clear append locations: USER.md (`## Personal Info / Preferences`), IDENTITY.md (`## Notes`), SOUL.md (`## Principles`/`## Boundaries`), MEMORY.md (`## Key Learnings`)

### Documentation Improvements (Continuous)
- **Bilingual documentation**: README and CHANGELOG fully separated into English (first) and Chinese (after separator) sections for readability
- **Scroll notice**: "请往下翻页查看中文说明" at the top of each document
- **Version & release protocol**: Added to AGENTS.md to enforce proper changelog ordering and release workflow
- **CHANGELOG structure**: Newest versions at top, oldest at bottom; clear separation between English and Chinese entries

### Known Issues
- CLI message length limit: `openclaw cron add --message` truncates messages > ~1KB. Workaround: use concise template. For fully detailed instructions, manually edit the task post-install using `openclaw cron edit --message`.
- No per-agent install filter: Skill auto-discovers all agents; cannot limit to a single agent via flag. To test one agent, either edit `install.sh` or manually create that agent's task after uninstall.
- Requires `self-improve-agent` for full automation (optional).
- Memory/processed/ directory must be writable (standard permissions suffice).
- No dry-run mode for testing (future enhancement).
- No performance optimizations (caching, indexing) - acceptable for typical workloads.

### Tested
- ✅ Fresh install on clean system (no pre-existing per-agent tasks)
- ✅ Reinstall over existing tasks (skips duplicates)
- ✅ Uninstall removes all skill-created tasks
- ✅ Task payload includes all expected fields (state file, processed dir, domain context)
- ✅ Gateway logs show no errors during installation
- ✅ Daily notes (2026-03-18) recorded full session for future compression
- ✅ Documentation rendering validated in Markdown viewers
- ✅ Bilingual separation and links verified

---

## [1.1.0] - 2026-03-18 (Initial Public Release)

### Added
- Auto-discovery of all agents via `openclaw agents list --json`
- Staggered weekly scheduling (Sundays, 30-minute intervals starting 03:00)
- Workspace isolation - each agent compresses its own memory files
- Basic extraction of preferences, decisions, and personal information
- Markdown date headers for all appended entries
- Summary notifications via DingTalk connector
- Uninstall script to remove all `per_agent_compression_*` tasks
- Comprehensive README with troubleshooting guide

---

## Upgrade Notes

### From 1.1.0 to 1.2.2
1. Run `./uninstall.sh` to remove old tasks
2. Replace skill directory with v1.2.2
3. Run `./install.sh` to register tasks
4. Existing `.compression_state.json` files will be preserved (backward compatible)

### From 1.2.2 to 1.3.0 (Security Fix)
This is a critical security update that removes hardcoded credentials and model references. Replace all skill files with v1.3.0 and re-run `./install.sh` to recreate tasks with safe defaults.

### Fresh Install
Simply run `./install.sh` after placing the skill in `/root/.openclaw/workspace/skills/`.

---

## Version Comparison

| Feature | 1.1.0 | 1.2.2 | 1.2.3-1.2.6 | 1.3.0 |
|---------|-------|-------|-------------|-------|
| Auto-discovery | ✅ | ✅ | ✅ | ✅ |
| State persistence | ❌ | ✅ | ✅ | ✅ |
| Deduplication | ❌ | ✅ | ✅ | ✅ |
| Domain filtering | ❌ | ✅ | ✅ | ✅ |
| Moved-file marking | ❌ | ✅ | ✅ | ✅ |
| Bilingual docs | ❌ | ✅ | ✅ | ✅ |
| Test artifacts | ❌ | ✅ | ✅ | ✅ |
| Production readiness label | ❌ | ✅ | ✅ | ✅ |
| CLI length workaround | ❌ | ✅ | ✅ | ✅ |
| No hardcoded credentials | ❌ | ❌ | ❌ | ✅ |

---

**Note**
This skill is actively iterated and tested. While core functionality is stable, edge cases (e.g., message length limits) may require manual intervention. Check CHANGELOG for latest updates.
