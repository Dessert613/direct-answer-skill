# Direct Answer Skill

让 GPT 直达问题，减少假想反驳、重复免责声明和无依据的自我削弱。

面向 GPT-6 Astra、GPT-6.1 Sol 常见的“叠甲式回答”设计，采用标准 `SKILL.md` 结构。首版提供表达规则和人工验收案例，可在支持该格式的工具中使用。

## 它处理什么

- 你说“今天适合散步”，回答却讨论能否据此推断长期气候。
- 你要求推荐方案，回答列出两面观点，始终不给出选择。
- 同一项限制已经说明，每轮仍重新提醒。
- 文本已有充分依据，润色后却被添加大量无依据的保留措辞。

核心做法：按用户实际立场回答，先给结论，把必要的不确定性放到具体主张旁边，删除为假想批评准备的句子。

## 安装到 Codex

将本仓库的 `skills/direct-answer` 文件夹复制到你的 Codex skills 目录。例如，默认 Windows 路径为：

```text
C:\Users\<用户名>\.codex\skills\direct-answer\SKILL.md
```

如果使用自定义 `CODEX_HOME`，复制到该目录下的 `skills/direct-answer`。

也可以在项目内使用 `.agents/skills/direct-answer/SKILL.md`。安装后开启新会话，并确认工具能发现 `direct-answer`。

本项目不运行安装脚本，不修改默认系统指令，也不自动注册 Hooks。

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

## 设计取舍

按表达的用途判断，不靠禁词表。必要的否定、事实纠错、真实不确定性以及有实际作用的风险提示都保留。

首版聚焦回答与文本编辑。它不限制工程测试，不拦截工具调用，不改变授权边界。

## 如何验收

使用 [examples.md](skills/direct-answer/references/examples.md) 中的输入，在同一模型、推理强度和工具环境下分别运行未启用与启用技能的会话。保留完整输出，比较：

1. 是否直接回答问题并给出需要的判断。
2. 是否出现假想反驳或重复限定。
3. 是否保留必要的纠错、条件与不确定性。
4. 在第 5 轮、第 10 轮后是否仍保持效果。

当前版本为初始规则，尚未完成 Astra / Sol 的实测对照。示例是人工编写的验收目标，不是模型实测结果。

## 项目结构

```text
skills/direct-answer/SKILL.md                核心技能
skills/direct-answer/references/examples.md  前后对照与人工验收输入
LICENSE                                     MIT 许可证
```

欢迎通过 Issue 提供模型版本、宿主、推理强度、输入和实际输出。分享前请删除个人信息及凭据。
