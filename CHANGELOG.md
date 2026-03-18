请往下翻页查看中文说明

---

# Changelog

All notable changes to this skill will be documented in this file.

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

### Known Issues
- Same as v1.2.2; no new issues

### Tested
- ✅ Documentation rendering tested in Markdown viewers
- ✅ All bilingual links and references verified

---

## [1.2.2] - 2026-03-18 (Final Testing & Optimization)

### Added
- Full bilingual documentation (README now separates English and Chinese sections for readability)
- Testing artifacts section with verified scenarios and manual verification steps
- Production readiness checklist with clear status indicators

### Changed
- Installation approach: Simplified from two-step (add + edit) to single-step with concise message template (~1200 chars) to avoid CLI length limits reliably
- Message content: Now includes all essential execution logic in a single line with `\n` escapes; full details maintained in README for reference
- Task discovery: Skill now creates 5 tasks (hrbp, parenting, decoration, memory_master, main) automatically; manual pre-filtering not supported
- Error handling: Installer provides clearer feedback when tasks already exist or when edit operations fail

### Fixed
- cron edit command confusion: Resolved earlier misunderstanding about `cron update` (non-existent); confirmed correct command is `openclaw cron edit --message`
- Over-deletion issue: Testing procedure refined to avoid removing unrelated pre-existing tasks; uninstall script only removes skill-created tasks
- Heredoc quoting problems: Eliminated by using single-line message with escaped newlines, improving script reliability

### Known Issues
- CLI length limit persists: `openclaw cron add --message` truncates messages > ~1KB. Workaround: use concise template. For fully detailed messages, manual `cron edit --message` after install is required.
- No agent filtering: Skill auto-discovers all agents; cannot limit to a single agent via flag. To test one agent, either edit the skill's `install.sh` or manually create that agent's task after uninstall.
- Self-improve-agent dependency: The skill itself does not require it, but system-level learning relies on separate `self-improve-agent` skill (optional).

### Tested
- ✅ Uninstall + reinstall flow works without leaving orphaned tasks
- ✅ All 5 tasks created with correct schedule (staggered Sundays 03:00-05:00 Shanghai)
- ✅ Task payload includes state tracking, dedupe, domain context, moved-file marking
- ✅ No errors in gateway logs during installation
- ✅ Daily notes (2026-03-18) capture complete session history

---

## [1.2.1] - 2026-03-18 (Two-Step Message Injection Attempt)

### Added
- Two-step message injection attempt: Due to CLI length limits, installer first creates task with short message, then attempts to update with full details via `cron edit`
- Improved error reporting: Installer now warns if full message update fails

### Changed
- Installer script: Added state validation and better error handling

### Fixed
- Command confusion: Early misstatement about `cron update` corrected to `cron edit`

---

## [1.2.0] - 2026-03-18

### Added
- State persistence & checkpoint resilience - Each agent task maintains `.compression_state.json` to resume from interruptions
- Deduplication - Checks target files before appending to avoid duplicate entries from same daily note
- Remaining notes reporting - Summary includes count of old notes still pending for future runs
- Enhanced error handling - Individual note failures don't stop the entire task; errors logged and continue
- Moved-file marking - Processed notes moved to `memory/processed/` directory for clear separation
- Domain-specific extraction guidelines - Each task includes DOMAIN_CONTEXT to tailor extraction (general, HR/work, parenting, renovation)
- Pre-check validation - Script verifies agents list, workspace existence, and memory directory before registration

### Changed
- Task naming - Changed from `peragent_compression_` to `per_agent_compression_` for better readability
- Timeout increased - From 300s to 1200s to accommodate larger note sets
- Message payload enriched - Detailed execution plan with specific file paths, state structure, and date header format (`### [YYYY-MM-DD]`)
- Delivery mode - Uses `--best-effort-deliver` to ensure notifications are attempted even if partial failures occur

### Fixed
- State file path - Now properly defined as `{workspace}/memory/.compression_state.json`
- Processed directory - Explicitly created as `{workspace}/memory/processed/`
- Target sections - Clear append locations: USER.md (`## Personal Info / Preferences`), IDENTITY.md (`## Notes`), SOUL.md (`## Principles`/`## Boundaries`), MEMORY.md (`## Key Learnings`)

### Known Issues
- No dry-run mode for testing (future enhancement)
- No performance optimizations (caching, indexing) - acceptable for typical workloads

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

### From 1.1.0/1.2.0 to 1.2.2
1. Run `./uninstall.sh` to remove old tasks
2. Replace skill directory with v1.2.2
3. Run `./install.sh` to register tasks (now with two-step message injection)
4. Existing `.compression_state.json` files will be preserved (backward compatible)

### From 1.2.2 to 1.2.3 (Documentation Update)
No functional changes. Simply update the skill files to the latest version.

### Fresh Install
Simply run `./install.sh` after placing the skill in `/root/.openclaw/workspace/skills/`.

---

## Version Comparison

| Feature | 1.1.0 | 1.2.0 | 1.2.1 | 1.2.2 | 1.2.3 |
|---------|-------|-------|-------|-------|-------|
| Auto-discovery | ✅ | ✅ | ✅ | ✅ | ✅ |
| State persistence | ❌ | ✅ | ✅ | ✅ | ✅ |
| Deduplication | ❌ | ✅ | ✅ | ✅ | ✅ |
| Domain filtering | ❌ | ✅ | ✅ | ✅ | ✅ |
| Moved-file marking | ❌ | ✅ | ✅ | ✅ | ✅ |
| Bilingual docs | ❌ | ❌ | ❌ | ✅ | ✅ |
| Test artifacts | ❌ | ❌ | ❌ | ✅ | ✅ |
| Production readiness label | ❌ | ❌ | ❌ | ✅ | ✅ |
| CLI length workaround | ❌ | ❌ | ⚠️ two-step attempt | ✅ concise template | ✅ concise template |

---

**Note**
This skill is actively iterated and tested. While core functionality is stable, edge cases (e.g., message length limits) may require manual intervention. Check CHANGELOG for latest updates.

---

================================================================================

以下是完整的中文更新日志

================================================================================

---

# 更新日志

本技能的所有重要变更都会记录在此文件中。

---

## [1.2.3] - 2026-03-18（文档更新）

### 新增
- **分离式双语文档** - README 和 CHANGELOG 现已分离为独立的英文和中文部分，提高可读性
- **滚动提示** - 每个文档顶部加入"请往下翻页查看中文说明"提示
- **版本发布协议** - 在 AGENTS.md 中添加版本与发布协议，规范变更流程

### 变更
- **文档结构** - 英文内容在前，完整中文翻译在后，用分隔线清晰划分
- **双语文档** - README 和 CHANGELOG 均实现双语，结构分明

### 修复
- 无（仅为文档更新）

### 已知问题
- 与 1.2.2 相同；无新问题

### 已测试
- ✅ 文档渲染测试（Markdown 查看器）
- ✅ 所有双语链接和引用验证

---

## [1.2.2] - 2026-03-18（最终测试与优化）

### 新增
- **完整双语文档** - README 现在分离英文和中文章节以提高可读性
- **测试文物部分** - 记录已验证场景和手动验证步骤
- **生产就绪清单** - 每个功能都有清晰的状态指示

### 变更
- **安装方式**：从两步（add + edit）简化为单步简洁消息模板（约1200字符），可靠避免 CLI 长度限制
- **消息内容**：现在在单行中包含所有基本执行逻辑（使用 `\n` 转义）；完整细节保存在 README 中供参考
- **任务发现**：技能现在自动创建5个任务（hrbp, parenting, decoration, memory_master, main）；不支持手动预过滤（需要时可修改技能）
- **错误处理**：安装程序在任务已存在或编辑操作失败时提供更清晰的反馈

### 修复
- **cron edit 命令混淆**：解决了之前对 `cron update` 的误解（不存在）；确认正确命令是 `openclaw cron edit --message`
- **过度删除问题**：优化测试流程，避免删除不相关的预存在任务；卸载脚本仅删除技能创建的任务
- **Heredoc 引号问题**：通过使用带转义换行的单行消息消除，提高了脚本可靠性

### 已知问题
- **CLI 长度限制仍然存在**：`openclaw cron add --message` 会截断超过 ~1KB 的消息。变通方法：使用简洁模板。如需完整详细消息，安装后需手动 `cron edit --message`（用户需自行操作）。
- **无代理过滤**：技能自动发现所有代理；无法通过标志限制单个代理。要测试单个代理，可编辑技能的 `install.sh` 或在卸载后手动创建该代理的任务。
- **Self-improve-agent 依赖**：技能本身不需要它，但系统级学习依赖于独立的 `self-improve-agent` 技能（可选，需要 API key）。

### 已测试
- ✅ 卸载+重新安装流程工作正常，不会留下孤立任务
- ✅ 所有5个任务创建正确调度（上海时间周日 03:00-05:00 错开）
- ✅ 任务负载包含状态跟踪、去重、领域上下文、移动文件标记
- ✅ 安装期间网关日志无错误
- ✅ 每日笔记 (2026-03-18) 捕获完整会话历史

---

## [1.2.1] - 2026-03-18（两步消息注入尝试）

### 新增
- **两步消息注入尝试**：由于 CLI 长度限制，安装程序首先创建带短消息的任务，然后尝试通过 `cron edit` 更新为完整详细信息
- **改进的错误报告**：如果完整消息更新失败，安装程序现在会警告

### 变更
- **安装脚本**：添加状态验证和更好的错误处理

### 修复
- **命令混淆**：早期关于 `cron update` 的错误陈述已修正为 `cron edit`

---

## [1.2.0] - 2026-03-18

### 新增
- **状态持久化与断点续传** - 每个代理任务维护 `.compression_state.json` 以从中断处恢复
- **去重** - 在追加前检查目标文件，避免同一 daily note 产生重复条目
- **剩余笔记报告** - 摘要包含仍待处理的老笔记数量
- **增强错误处理** - 单个笔记失败不会停止整个任务；错误被记录并继续
- **移动文件标记** - 已处理笔记移动到 `memory/processed/` 目录，清晰分离
- **领域特定提取指南** - 每个任务包含 DOMAIN_CONTEXT 以定制提取（通用、HR/工作、育儿、装修）
- **预检查验证** - 脚本在注册前验证代理列表、工作区存在性和 memory 目录

### 变更
- **任务命名** - 从 `peragent_compression_` 改为 `per_agent_compression_` 以提高可读性
- **超时增加** - 从 300 秒增加到 1200 秒以适应更大的笔记集
- **消息负载丰富化** - 包含具体文件路径、状态结构和日期头格式的详细执行计划
- **交付模式** - 使用 `--best-effort-deliver` 确保即使部分失败也尝试通知

### 修复
- **状态文件路径** - 正确定义为 `{workspace}/memory/.compression_state.json`
- **处理目录** - 显式创建为 `{workspace}/memory/processed/`
- **目标章节** - 清晰的追加位置：USER.md (`## Personal Info / Preferences`), IDENTITY.md (`## Notes`), SOUL.md (`## Principles`/`## Boundaries`), MEMORY.md (`## Key Learnings`)

### 已知问题
- 无干运行模式用于测试（未来增强）
- 无性能优化（缓存、索引）- 对典型工作量可接受

---

## [1.1.0] - 2026-03-18（初始公开发布）

### 新增
- 通过 `openclaw agents list --json` 自动发现所有代理
- 交错每周调度（周日，从 03:00 开始，间隔 30 分钟）
- 工作区隔离 - 每个代理压缩自己的 memory 文件
- 偏好、决策和个人信息的基本提取
- 所有追加条目的 Markdown 日期头
- 通过钉钉连接器发送摘要通知
- 卸载脚本以删除所有 `per_agent_compression_*` 任务
- 包含故障排除指南的全面 README

---

## 升级说明

### 从 1.1.0/1.2.0 升级到 1.2.2
1. 运行 `./uninstall.sh` 删除旧任务
2. 替换技能目录为 v1.2.2
3. 运行 `./install.sh` 注册任务
4. 现有的 `.compression_state.json` 文件将被保留（向后兼容）

### 从 1.2.2 到 1.2.3（文档更新）
无功能变更。只需将技能文件更新到最新版本即可。

### 全新安装
只需将技能放置在 `/root/.openclaw/workspace/skills/` 后运行 `./install.sh`。

---

## 版本对比

| 功能 | 1.1.0 | 1.2.0 | 1.2.1 | 1.2.2 | 1.2.3 |
|------|-------|-------|-------|-------|-------|
| 自动发现 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 状态持久化 | ❌ | ✅ | ✅ | ✅ | ✅ |
| 去重 | ❌ | ✅ | ✅ | ✅ | ✅ |
| 领域过滤 | ❌ | ✅ | ✅ | ✅ | ✅ |
| 移动文件标记 | ❌ | ✅ | ✅ | ✅ | ✅ |
| 双语文档 | ❌ | ❌ | ❌ | ✅ | ✅ |
| 测试文物 | ❌ | ❌ | ❌ | ✅ | ✅ |
| 生产就绪标签 | ❌ | ❌ | ❌ | ✅ | ✅ |
| CLI 长度变通 | ❌ | ❌ | ⚠️ 两步尝试 | ✅ 简洁模板 | ✅ 简洁模板 |

---

**注意**
此技能正在积极迭代和测试中。虽然核心功能稳定，但边缘情况（如消息长度限制）可能需要进行手动干预。查看 CHANGELOG 获取最新更新。
