# Direct Answer

让 GPT 少叠甲，先把问题答清楚。

面向 GPT Astra、Sol 等模型的回答风格 Skill，处理假想反驳、重复免责声明、绕圈子的推荐和模板化表达。适用于日常问答、方案选择、文章改写和资料整理。

## 回答可以更直接

下面是几组编辑示例：

| 场景 | 绕圈子的写法 | 直接的写法 |
| --- | --- | --- |
| 只需要离线查阅，A 支持离线，B 必须联网 | 建议优先考虑 A，但最终还应回到真实任务，以实际使用效果为准。 | 选 A，它支持你需要的离线查阅。 |
| 总结 100 个用例的测试结果 | 在这 100 个用例上，成功率从 70% 提高到 85%，但这不能证明它在所有环境下都更好。 | 在这 100 个测试用例上，成功率从 70% 提高到 85%。 |
| 写一段功能介绍 | 这不仅是一次功能升级，更是效率的飞跃。它支持下载记录。 | 本页支持下载记录。 |

推荐已经给出，就让回答停在推荐上。限制确实影响当前选择时，把它放到对应结论旁边说清楚。

## 安装

需要 Node.js 22.20 或更新版本，以及 Git。在终端运行：

```sh
npx skills add https://github.com/Dessert613/direct-answer-skill --skill direct-answer --agent codex --global --copy
```

安装后开启新会话，输入 `$direct-answer` 使用。

项目级安装、Windows / macOS / Linux 手动安装，以及更新和卸载方法，见 [安装说明](docs/install.md)。

## 使用

**回答问题、给出建议：**

```text
$direct-answer
请比较这两个方案，给出你的推荐和关键理由。
```

**改写文章或说明：**

```text
$direct-answer
把下面这段话写得自然、直接，保留原意，只输出正文：
……
```

**纠正已经开始叠甲的回答：**

```text
$direct-answer
重写上一条，删掉假想反驳、重复限定和泛化的评测收尾，直接回答我的问题。
```

## 它怎样选择该说的话

- 回应用户实际表达的意思，不替用户补上一个极端观点再反驳。
- 给出有依据的判断，让必要条件紧挨着相关结论。
- 已经解释过的限制，没有新情况就不再重复。
- 清理空话和模板句，保留文章原有的态度与语气。

核心规则用三个问题判断限定语是否值得保留：它是否影响当前问题？能否说清具体影响？是否提供了新信息？

## 文档

- [核心技能](skills/direct-answer/SKILL.md)：完整回答规则。
- [表达自然度](skills/direct-answer/references/natural-writing.md)：空话、长句与文风的处理。
- [长对话使用](skills/direct-answer/references/continuity.md)：重新调用与简短提醒。
- [设计与来源](docs/design.md)：规则的形成过程。
- [参与改进](docs/contributing.md)：反馈方式与案例检查。

有哪句话又让你觉得“怎么还在叠甲”，欢迎在 Issue 里贴出原句和希望的写法。

## 许可与参考

[MIT License](LICENSE)。表达自然度部分参考 [Humanizer-zh](https://github.com/op7418/Humanizer-zh)，相关许可随技能一起提供，见 [第三方许可](skills/direct-answer/THIRD_PARTY_NOTICES.md)。
