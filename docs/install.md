# 安装与管理

## 方法一：一条命令安装到 Codex

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

## 方法二：手动复制

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

## 确认安装并开始使用

使用 `skills` CLI 安装后，可查看全局安装列表：

```sh
npx skills list --agent codex --global
```

项目级安装省略 `--global`，并在对应项目目录中运行。

开启新的 Codex 会话，输入 `$direct-answer` 调用技能。Codex CLI / IDE 也可通过 `/skills` 查找；如果没有出现，重启 Codex 后重试。提问示例见 [README 的使用部分](../README.md#使用)。

## 更新与卸载

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

安装位置与命令参考：[Codex 官方技能文档](https://learn.chatgpt.com/docs/build-skills)、[skills CLI 命令说明](https://github.com/vercel-labs/skills/blob/main/docs/commands.md)。
