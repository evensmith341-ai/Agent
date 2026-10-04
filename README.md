# Agent：每日 AI 简报

这是一个可复用的 Codex 技能。按指定日期检索并核验 AI 行业新闻、arXiv 论文和 GitHub 开源项目，使用简体中文输出标题、摘要、来源链接和日期。

## 项目文件

- `skills/daily-ai-brief/SKILL.md`：日期筛选、来源核验、检索与输出规则。
- `skills/daily-ai-brief/agents/openai.yaml`：技能名称、简介和默认提示词。

## 安装

将 `skills/daily-ai-brief` 整个目录复制到 Codex 的用户技能目录，默认是 `~/.codex/skills/`。如果设置了 `CODEX_HOME`，使用该目录下的 `skills/`。

Windows PowerShell 示例（在本仓库根目录执行）：

```powershell
$skillRoot = if ($env:CODEX_HOME) { Join-Path $env:CODEX_HOME 'skills' } else { Join-Path $env:USERPROFILE '.codex/skills' }
New-Item -ItemType Directory -Path $skillRoot -Force | Out-Null
Copy-Item -LiteralPath './skills/daily-ai-brief' -Destination $skillRoot -Recurse
```

若目标目录已有同名技能，请先备份并确认是否替换。

## 使用

```text
使用 $daily-ai-brief 生成 2026-10-03 的每日 AI 简报。
```

可以指定主题、每类条数和时区。默认每类最多 5 条，时区为 `Asia/Shanghai`。没有指定日期时，技能会先询问日期。

## 检索能力

运行环境需要网页搜索与原文读取、arXiv 官方页面或 Atom API 访问，以及 GitHub 仓库、许可证与 Release 读取能力。优先使用当前已有的内置联网工具和 GitHub 连接器；能够读取公开数据时，无需额外付费模型 API。

每条内容必须来自成功读取并核验的真实来源。来源不可访问、认证失败或限流时，说明原因及所需配置，并继续处理可用类别。检索成功但没有合格内容时保留空分类，不编造或扩大日期范围凑数。

本项目提供技能规则与界面配置，执行检索时使用 Codex 会话中的工具。
