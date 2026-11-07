# Direct Answer Skill

让 GPT 直达问题，减少假想反驳、重复免责声明和无依据的自我削弱。

面向 GPT-6 Astra、GPT-6.1 Sol 常见的“叠甲式回答”设计，采用标准 `SKILL.md` 结构。当前版本综合社区提示词、开源项目与失效反馈，提供核心规则、长对话提醒和 25 组验收案例。

## 它处理什么

- 你说“今天适合散步”，回答却讨论能否据此推断长期气候。
- 你要求推荐方案，回答列出两面观点，始终不给出选择。
- 同一项限制已经说明，每轮仍重新提醒。
- 文本已有充分依据，润色后却被添加大量无依据的保留措辞。
- 已经给出推荐，结尾却又要求“回到真实任务测试”，把建议变成泛化评估流程。

## 核心方法

围绕用户的问题形成判断，再决定哪些限定值得保留。每项限定都要满足三项条件：

| 条件 | 要回答的问题 |
| --- | --- |
| 相关 | 它会改变当前哪个结论、选择或操作？ |
| 具体 | 能否指出受影响的主张、实际后果或判断条件？ |
| 新增 | 它是否还没解释过，或场景确实发生了变化？ |

满足条件就放到对应主张旁边。仅为防范假想批评、重复证明自己谨慎的句子直接删去。证据支持的结论保持原有力度。

这套方法由本项目综合整理。资料中的观察、反例和取舍见 [设计与来源](docs/design.md)。

### 表达自然度作为辅助

主要目标仍是减少防御性回答。在回答成立之后，再参考 [Humanizer-zh](https://github.com/op7418/Humanizer-zh) 的思路清理空话、重复和模板腔，梳理生硬长句，并保持原文语气。

顺序是：保留信息与用户范围 → 处理防御性表达 → 改善自然度。正式文本保留正式程度，清楚的句子可以不改；不凭空增加数字、经历或情绪。具体规则见 [表达自然度](skills/direct-answer/references/natural-writing.md)。

## 安装

### 方法一：一条命令安装到 Codex

先安装 [Node.js](https://nodejs.org/) **22.20 或更新版本**和 [Git](https://git-scm.com/)，然后在终端运行：

```sh
npx skills add https://github.com/Dessert613/direct-answer-skill --skill direct-answer --agent codex --global --copy
```

按提示确认后，技能对当前用户的所有项目可用。`--copy` 使用文件复制方式安装。

如果只想在某个项目中使用，在该项目的根目录运行下面的命令：

```sh
npx skills add https://github.com/Dessert613/direct-answer-skill --skill direct-answer --agent codex --copy
```

项目级安装使用 `.agents/skills/direct-answer/`。全局安装使用用户目录下的 `.agents/skills/direct-answer/`。

使用 Claude Code 时，将命令中的 `--agent codex` 改为 `--agent claude-code`；其全局技能目录为 `~/.claude/skills/`。

### 方法二：手动复制

无需 Node.js。先[下载 ZIP](https://github.com/Dessert613/direct-answer-skill/archive/refs/heads/main.zip)并解压，或克隆仓库：

```sh
git clone https://github.com/Dessert613/direct-answer-skill.git
```

进入下载或克隆得到的仓库根目录，复制完整的 `skills/direct-answer` 文件夹，包括 `references` 和第三方许可文件。

**Windows PowerShell：**

```powershell
$skillParent = Join-Path $env:USERPROFILE '.agents\skills'
New-Item -ItemType Directory -Path $skillParent -Force | Out-Null
Copy-Item -LiteralPath '.\skills\direct-answer' -Destination $skillParent -Recurse -Force
```

**macOS / Linux：**

```sh
mkdir -p "$HOME/.agents/skills"
cp -R skills/direct-answer "$HOME/.agents/skills/"
```

安装后的结构应当是：

```text
~/.agents/skills/direct-answer/
├── SKILL.md
├── THIRD_PARTY_NOTICES.md
└── references/
    ├── continuity.md
    ├── examples.md
    └── natural-writing.md
```

手动安装到单个项目时，将同一文件夹放入该项目的 `.agents/skills/` 即可。

### 确认安装并开始使用

使用 `skills` CLI 安装后，可查看全局安装列表：

```sh
npx skills list --agent codex --global
```

项目级安装省略 `--global`，并在对应项目目录中运行。

开启新的 Codex 会话，输入 `$direct-answer` 调用技能。Codex CLI / IDE 也可通过 `/skills` 查找；如果没有出现，重启 Codex 后重试。具体提问示例见下面的“使用”。

### 更新与卸载

使用 `skills` CLI 全局安装的技能，可单独更新：

```sh
npx skills update direct-answer --global
```

项目级安装在项目目录执行 `npx skills update direct-answer --project`。

卸载全局安装：

```sh
npx skills remove direct-answer --agent codex --global
```

项目级卸载在项目目录运行相同命令，并省略 `--global`。手动安装的技能，更新时复制最新的完整文件夹；卸载时移除安装位置的 `direct-answer` 文件夹。

技能本体是 Markdown 指令与参考文件，不附带安装脚本，不修改默认系统指令，也不自动注册 Hooks。

安装位置与命令参考：[Codex 官方技能文档](https://learn.chatgpt.com/docs/build-skills)、[skills CLI 命令说明](https://github.com/vercel-labs/skills/blob/main/docs/commands.md)。

## 使用

显式调用：

```text
$direct-answer
请分析这两个方案，并给出你推荐的选择。
```

修改已有文本：

```text
$direct-answer
请保留事实、数据和证据强度，清除下面这段文字中的防御性表达，只输出修改后的正文：
……
```

其他支持 skills 的工具可按其规则安装 `skills/direct-answer` 目录。仅支持普通提示词的工具，可以手动使用 `SKILL.md` 正文；自动发现与持续生效取决于宿主。

### 长对话中再次出现叠甲

```text
$direct-answer
重写上一条：回应我的实际主张，限定须相关、具体且新增。
保留必要纠错和真实风险，直接输出修改后的回答。
```

上下文摘要、重新调用和短提醒的用法见 [持续使用说明](skills/direct-answer/references/continuity.md)。本项目提供手动提醒文本；未实现自动注入，也不承诺跨会话记忆。

## 设计取舍

按表达的用途判断，不靠禁词表。必要的否定、事实纠错、真实不确定性以及有实际作用的风险提示都保留。

技能聚焦回答与文本编辑。工程测试、工具调用和授权边界按实际任务处理。

文风调整保留作者立场、归因与完成状态；文件编辑同时保护代码、命令、路径、链接目标和结构。这里的“去 AI 味”是改善表达，不用于作者身份判断或检测器承诺。

## 如何验收

使用 [examples.md](skills/direct-answer/references/examples.md) 中的输入，在同一模型、推理强度和工具环境下分别运行未启用与启用技能的会话。保留完整输出，比较：

1. 是否直接回答问题并给出需要的判断。
2. 是否出现假想反驳或重复限定。
3. 是否保留必要的纠错、条件与不确定性。
4. 在第 5 轮、第 10 轮后是否仍保持效果。
5. 清理模板腔后，是否保留作者声音、独立信息、归因和文件内容。
6. 普通建议是否自然结束，评测方法是否只在用户要求或存在决定性未知时出现。

重点同时检查两类结果：无用途的防御性表达是否减少，必要纠错与真实不确定性是否保留。按含义判断，避免仅统计否定词数量。

当前规则尚未完成 Astra / Sol 的实测对照。案例包含用户提供的测试输出与人工教学示例，目标回答为编辑参考；文件结构检查不代表行为效果已经验证。

## 项目结构

```text
skills/direct-answer/SKILL.md                核心技能
skills/direct-answer/references/examples.md  前后对照与人工验收输入
skills/direct-answer/references/continuity.md 长对话提醒与上下文交接
skills/direct-answer/references/natural-writing.md 辅助表达规则
docs/design.md                              资料综合与设计取舍
skills/direct-answer/THIRD_PARTY_NOTICES.md   随技能分发的第三方来源与许可
LICENSE                                     MIT 许可证
```

欢迎通过 Issue 提供模型版本、宿主、推理强度、输入和实际输出。分享前请删除个人信息及凭据。
