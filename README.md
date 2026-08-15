# Codex 配置仓库

本仓库用于在不同终端之间同步 Codex 的可移植配置，主要跟踪人工维护的 `agents/`、`skills/` 和相关配置文件。认证信息、会话、数据库、缓存、插件运行数据以及 Codex 自带的 `skills/.system/` 不应提交。

远程仓库：`https://github.com/soul-jh/my-codex-config.git`

## 将仓库接入已有的 `~/.codex`

以下流程保留现有目录中的未跟踪文件。开始前仍应完整备份，因为远程仓库中同路径的文件会替换本地版本。

```bash
backup="$HOME/.codex.backup.$(date +%Y%m%d-%H%M%S)"
cp -a "$HOME/.codex" "$backup"

cd "$HOME/.codex"
git init
git remote remove origin 2>/dev/null || true
git remote add origin https://github.com/soul-jh/my-codex-config.git
git fetch origin master
git reset --mixed origin/master
git branch -M master

# 先检查：远程会覆盖哪些已跟踪文件；未跟踪文件不会被删除。
git status --short

# 确认后，让远程版本覆盖 Git 跟踪范围。
git restore .
git branch --set-upstream-to=origin/master master
```

如果 `git restore .` 前发现本地同名配置仍需保留，先从备份中复制到其他位置并手工合并。不要使用 `git clean`，否则可能删除认证、会话和系统生成文件。

接入后验证：

```bash
cd "$HOME/.codex"
git remote -v
git branch -vv
git status --short
```

## 日常更新

在任意终端执行：

```bash
cd "$HOME/.codex"
git status --short
git pull --ff-only
```

如果跟踪文件有本地修改，先审阅并提交，或者手工合并；不要直接丢弃不确定的改动。新安装且值得跨终端共享的 agent/skill，需要显式 `git add` 后提交；本机运行数据保持未跟踪。

```bash
cd "$HOME/.codex"
git add README.md agents skills .gitignore
git diff --cached
git commit -m "chore: update Codex configuration"
git push
```

## 跟踪边界

- 跟踪：人工维护的 agents、用户 skills、README 和共享配置。
- 不跟踪：`skills/.system/`；它由 Codex 管理，升级时可能变化。
- 不提交：`auth.json`、历史记录、会话、SQLite 数据库、日志、缓存、临时文件和插件缓存。
- 本地新增但仓库没有的文件默认保留；只有明确确认后才删除。

## Agents（14）

| Agent | 能力 |
| --- | --- |
| `api-tester` | API 功能、兼容性、性能和第三方集成测试 |
| `application-security-engineer` | 威胁建模、安全审查与应用安全治理 |
| `backend-architect` | 后端、数据库、API 与云端架构设计 |
| `code-reviewer` | 从正确性、维护性、安全和性能维度审查代码 |
| `database-optimizer` | 数据库建模、索引、查询和性能优化 |
| `devops-automator` | 基础设施自动化、CI/CD 和云端运维 |
| `document-generator` | 生成 PDF、PPTX、DOCX 和 XLSX 文档 |
| `frontend-developer` | Web 前端、组件、交互和性能开发 |
| `incident-responder` | 安全事件调查、遏制、响应和复盘 |
| `minimal-change-engineer` | 用最小、可审阅的改动完成修复 |
| `performance-benchmarker` | 性能测试、瓶颈分析和回归检测 |
| `senior-developer` | 复杂功能实现和高级工程开发 |
| `software-architect` | 系统设计、领域建模和架构决策 |
| `technical-writer` | README、API 文档、教程和技术说明 |

## 用户 Skills（61）

### 通用开发与设计

| Skill | 能力 |
| --- | --- |
| `api-and-interface-design` | API、类型契约和模块边界设计 |
| `code-review-and-quality` | 多维度代码审查与质量检查 |
| `code-simplification` | 在不改变行为的前提下简化代码 |
| `deprecation-and-migration` | 旧接口、旧系统的弃用与迁移 |
| `frontend-ui-engineering` | 可访问、响应式、生产级 UI 开发 |
| `git-workflow-and-versioning` | Git 分支、提交、发布和版本管理 |
| `incremental-implementation` | 将较大改动拆成可验证的小步实现 |
| `source-driven-development` | 基于官方资料做技术实现 |
| `spec-driven-development` | 先形成规格，再实施复杂需求 |
| `test-driven-development` | 以测试驱动功能和缺陷修复 |
| `vercel-react-best-practices` | React/Next.js 性能最佳实践 |

### 规划、上下文与协作

| Skill | 能力 |
| --- | --- |
| `ask` | 通过 OMC 路由 Claude、Codex 或 Gemini 咨询 |
| `context-engineering` | 优化项目规则和 Agent 上下文 |
| `planning-and-task-breakdown` | 把需求拆成有序、可执行任务 |
| `planning-with-files` | 用文件维护复杂任务计划和进度 |
| `project-session-manager` | 使用 worktree/tmux 管理开发会话 |
| `prompt-engineering-patterns` | 设计和优化生产级提示词 |
| `using-agent-skills` | 发现并正确调用适用 skills |

### 排障、质量与安全

| Skill | 能力 |
| --- | --- |
| `debugging-and-error-recovery` | 系统化定位根因并恢复故障 |
| `observability-and-instrumentation` | 日志、指标、追踪和告警建设 |
| `performance-optimization` | 前后端、查询和数据库性能优化 |
| `security-and-hardening` | 输入、认证、存储和外部集成加固 |
| `security-best-practices` | Python、JS/TS、Go 安全专项审查 |
| `sentry` | 只读分析 Sentry 事件和生产错误 |
| `trace` | 多假设、证据驱动的问题追踪 |

### 浏览器、前端验证与设计

| Skill | 能力 |
| --- | --- |
| `agent-browser` | 浏览器导航、表单、截图、抓取和自动化 |
| `browser-testing-with-devtools` | 使用 Chrome DevTools 调试浏览器应用 |
| `figma` | 获取 Figma 设计上下文、变量和资源 |
| `figma-implement-design` | 将 Figma 节点高保真实现为代码 |
| `playwright` | 使用 Playwright 自动化真实浏览器 |
| `visual-verdict` | 对截图和参考图进行结构化视觉验收 |

### 文档、架构与工具配置

| Skill | 能力 |
| --- | --- |
| `architecture-diagram` | 生成系统、云和网络架构图 |
| `documentation-and-adrs` | 编写文档并记录架构决策 |
| `jupyter-notebook` | 创建和维护实验、教程类 Notebook |
| `mcp-setup` | 配置常用 MCP 服务 |

### GitHub 与交付自动化

| Skill | 能力 |
| --- | --- |
| `ci-cd-and-automation` | 配置 CI/CD、测试门禁和部署流程 |
| `gh-address-comments` | 处理 GitHub PR/Issue 审查意见 |
| `gh-fix-ci` | 调查并修复 GitHub Actions 失败 |

### Gstack 工作流

| Skill | 能力 |
| --- | --- |
| `gstack/benchmark` | 浏览器性能回归检测 |
| `gstack/browse` | 快速无头浏览器测试 |
| `gstack/careful` | 为危险命令提供安全防护 |
| `gstack/diagram` | 生成 Mermaid/Excalidraw 图表 |
| `gstack/document-generate` | 为项目或功能补齐文档 |
| `gstack/document-release` | 发布后更新相关文档 |
| `gstack/freeze` | 将编辑范围限制在指定目录 |
| `gstack/guard` | 启用完整安全与目录边界保护 |
| `gstack/health` | 生成代码质量概览 |
| `gstack/investigate` | 系统化根因调查 |
| `gstack/plan-eng-review` | 从工程管理视角审查计划 |
| `gstack/qa` | 测试 Web 应用并修复问题 |
| `gstack/qa-only` | 仅报告 Web QA 问题，不修改代码 |
| `gstack/review` | 合并前代码审查 |
| `gstack/setup-browser-cookies` | 向无头浏览器导入 Chromium Cookie |
| `gstack/ship` | 测试、审查、提交、推送和创建 PR |
| `gstack/unfreeze` | 清除 `freeze` 设置的编辑边界 |

### 代码库理解

| Skill | 能力 |
| --- | --- |
| `understand-anything/understand` | 生成代码库架构知识图谱 |
| `understand-anything/understand-diff` | 分析 diff/PR 的影响范围和风险 |
| `understand-anything/understand-domain` | 提取业务领域知识和流程图 |
| `understand-anything/understand-explain` | 深入解释文件、函数或模块 |
| `understand-anything/understand-onboard` | 为新成员生成项目入门指南 |

### Skill 管理

| Skill | 能力 |
| --- | --- |
| `find-skills` | 查找可安装的扩展 skill |

## 系统 Skills（6，不纳入 Git）

系统 skills 位于 `skills/.system/`，由 Codex 安装和升级：

- `imagegen`：生成或编辑图片。
- `openai-docs`：查询 OpenAI/Codex 官方文档。
- `plugin-creator`：创建 Codex 插件。
- `review-agent`：创建或维护代码审查 Agent。
- `skill-creator`：创建或维护 skills。
- `skill-installer`：安装官方或 GitHub skills。

> 系统 skills 的数量或名称可能随 Codex 升级变化；不要为了与仓库对齐而删除它们。
