# Agent：每日 AI 简报

这是一个可复用的 Codex 技能。按指定日期检索并核验 AI 行业新闻、arXiv 论文和 GitHub 开源项目，使用简体中文输出标题、摘要、来源链接和日期。

## 项目文件

- `skills/daily-ai-brief/SKILL.md`：日期筛选、来源核验、检索与输出规则。
- `skills/daily-ai-brief/agents/openai.yaml`：技能名称、简介和默认提示词。

## 安装前准备

- 已安装并登录支持技能的 Codex。
- 使用命令行克隆时，需要已安装 Git；也可以下载 ZIP，无需 Git。
- 生成简报时，需要可用的联网搜索与网页读取能力。

## 安装

### 推荐：从 Release 下载

1. 打开[最新 Release](https://github.com/evensmith341-ai/Agent/releases/latest)，在 **Assets** 中下载 `daily-ai-brief.zip`。
2. 解压，将其中的 `daily-ai-brief` 文件夹复制到用户主目录下的 `.agents/skills/`，没有该目录时先创建。
3. 在 Codex 中尝试本文的调用示例。技能没有出现时，重启 Codex 后再试。

Windows 通常是 `C:\Users\你的用户名\.agents\skills\`；macOS / Linux 为 `~/.agents/skills/`。ZIP 内附有 `安装说明.md`。如果已有同名技能，先备份并移走旧目录再安装。

[直接下载技能 ZIP](https://github.com/evensmith341-ai/Agent/releases/latest/download/daily-ai-brief.zip)

### 方式一：克隆仓库后安装

先在终端执行：

```bash
git clone --depth 1 https://github.com/evensmith341-ai/Agent.git
cd Agent
```

然后根据系统选择以下命令。安装后的技能可用于本机其他项目。

**Windows（PowerShell）**

```powershell
$skillRoot = Join-Path $env:USERPROFILE '.agents/skills'
$skillTarget = Join-Path $skillRoot 'daily-ai-brief'
if (Test-Path -LiteralPath $skillTarget) {
    throw '已存在 daily-ai-brief，请先备份并移走旧目录，再重新安装。'
}
New-Item -ItemType Directory -Path $skillRoot -Force | Out-Null
Copy-Item -LiteralPath './skills/daily-ai-brief' -Destination $skillRoot -Recurse
```

**macOS / Linux（终端）**

```bash
(
  set -e
  skill_target="$HOME/.agents/skills/daily-ai-brief"
  if [ -e "$skill_target" ] || [ -L "$skill_target" ]; then
    echo "已存在 daily-ai-brief，请先备份并移走旧目录，再重新安装。"
    exit 1
  fi
  mkdir -p "$HOME/.agents/skills"
  cp -R ./skills/daily-ai-brief "$skill_target"
)
```

手动安装采用当前 [OpenAI 官方文档](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)列出的用户技能目录 `~/.agents/skills/`。已通过安装器安装的用户，使用安装器返回的实际安装位置。

### 方式二：下载 ZIP 后安装

1. 在[仓库主页](https://github.com/evensmith341-ai/Agent)点击 **Code → Download ZIP**。
2. 解压后，找到 `skills/daily-ai-brief` 文件夹。
3. 将整个 `daily-ai-brief` 文件夹复制到用户主目录下的 `.agents/skills/`，没有该目录时先创建。

Windows 通常是 `C:\Users\你的用户名\.agents\skills\`；macOS / Linux 为 `~/.agents/skills/`。如果已有同名技能，先备份并移走旧目录再安装。

两种手动安装方式最终都应得到以下结构：

```text
.agents/skills/daily-ai-brief/
├── SKILL.md
└── agents/
    └── openai.yaml
```

### 方式三：让 Codex 安装

在 Codex 聊天中粘贴以下内容：

```text
使用 $skill-installer 安装这个 GitHub 仓库中的技能：
https://github.com/evensmith341-ai/Agent/tree/main/skills/daily-ai-brief
```

Codex 安装器负责下载文件并选择安装位置，无需手动克隆仓库。

## 验证安装

安装完成后，在新一轮对话中尝试下面的调用示例。如果技能没有出现或未被识别，重启 Codex 后再试。参见 [OpenAI 官方安装说明](https://learn.chatgpt.com/docs/build-skills#install-curated-skills-for-local-use)。

## 使用

```text
使用 $daily-ai-brief 生成 2026-10-03 的每日 AI 简报。
```

可以指定主题、每类条数和时区。默认每类最多 5 条，时区为 `Asia/Shanghai`。没有指定日期时，技能会先询问日期。

## 更新

如果是克隆安装，在本仓库目录执行 `git pull --ff-only`；如果是 Release 或 ZIP 安装，重新下载并解压最新版本。备份并移走技能安装目录中的旧 `daily-ai-brief` 文件夹，然后按上述步骤复制最新文件，再尝试调用技能。

## 检索能力

运行环境需要网页搜索与原文读取、arXiv 官方页面或 Atom API 访问，以及 GitHub 仓库、许可证与 Release 读取能力。优先使用当前已有的内置联网工具和 GitHub 连接器；能够读取公开数据时，无需额外付费模型 API。

每条内容必须来自成功读取并核验的真实来源。来源不可访问、认证失败或限流时，说明原因及所需配置，并继续处理可用类别。检索成功但没有合格内容时保留空分类，不编造或扩大日期范围凑数。

本项目提供技能规则与界面配置，执行检索时使用 Codex 会话中的工具。
