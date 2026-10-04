# Agent：每日 AI 热点简报

这是一个可复用的 Codex 技能，生成每日 AI 热点新闻概要，并筛选 GitHub 近 7 天热门 AI 技能（skill）。技能榜按区间内新增获星排序，介绍每个技能的内容和作用，附真实来源、准确统计区间及检索时间。累计 Star 仅作为仓库背景信息展示。

新闻默认检索今天，也可指定日期。技能包括实际提供 AI 助手/智能体技能包的仓库与技能合集（如包含 `SKILL.md`），按仓库计 Star；泛 AI 模型、应用、框架和纯链接导航清单不进入技能榜。

## 项目文件

- `skills/daily-ai-brief/SKILL.md`：热点新闻检索、近七日技能筛选、技能介绍与作用及来源核验规则。
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
使用 $daily-ai-brief 生成今天的 AI 热点新闻概要，以及近一周最热门的 5 个 GitHub AI 技能，注明 Star 数据来源和统计区间。
```

只查看技能榜也可以：

```text
使用 $daily-ai-brief 筛选近 7 天热门的 5 个 GitHub AI 技能，介绍每个技能的内容和作用，并注明新增获星与统计区间。
```

可以指定新闻日期、主题、条数、技能仓库数量和时区。默认最多 5 条新闻、5 个技能仓库，时区为 `Asia/Shanghai`；新闻未指定日期时采用今天。技能榜固定为本次检索截止时间前 7 天，按可核验的区间新增获星降序排序。技能榜不会自动扩大新闻日期范围，用户只请求一部分时仅输出该部分。

每个技能仓库附「介绍」「作用」、GitHub 仓库主页链接、近 7 日新增获星、累计 Star 和统计时间；合集介绍至少一个已核验的代表技能。推荐链接统一采用 `https://github.com/owner/repo`，不展示 raw 文件链接、API 地址或 `SKILL.md` 文件链接；这些只用于后台核验。近 7 日筛选针对获星区间，仓库本身不必是近 7 日内创建的。

新增获星与包含取消 Star 影响的净增长采用不同口径，不能混排。来源日界和覆盖范围如实标注；只能取得自然周合计且无法核验近 7 日窗口时，说明缺口。历史数据不足时不编造排名，不用累计 Star 替代。当前版本只输出近七日技能榜，不生成累计总榜、独立趋势榜或月榜。历史新闻默认搭配当前近七日技能榜，榜单代表本次检索范围。

## 更新

如果是克隆安装，在本仓库目录执行 `git pull --ff-only`；如果是 Release 或 ZIP 安装，重新下载并解压最新版本。备份并移走技能安装目录中的旧 `daily-ai-brief` 文件夹，然后按上述步骤复制最新文件，再尝试调用技能。

## 检索能力

运行环境需要网页搜索与新闻原文读取，以及 GitHub 仓库搜索、Star 元数据、实际技能文件和近 7 日获星历史读取能力。优先使用当前已有的内置联网工具和 GitHub 连接器；能够读取公开数据时，无需额外付费模型 API。

每条内容必须来自成功读取并核验的真实来源。来源不可访问、认证失败或限流时，说明原因及所需配置，并继续处理可用类别。检索成功但没有合格内容时保留空分类，不编造或扩大日期范围凑数。

本项目提供技能规则与界面配置，执行检索时使用 Codex 会话中的工具。
