# superpowers 在 Codex 里怎么用：15 个 skill 拆开看，哪些值得留

> 核对：2026 年 11 月 14 日读了 superpowers 7.0.2 的源码（各 `SKILL.md` 的长度、给 Codex 的插件包、多 agent 开关）；Codex 里的安装和加载测试是 10 月 18 日在 7.0.0、Codex CLI 0.163.0 上做的，没有跑任务；官方插件市场现在发的是哪一版没有核对，可能比仓库晚。下面关于「用起来怎样」的部分，来自我们在 Claude Code 里用这套流程的经验。

[obra/superpowers](https://github.com/obra/superpowers)，作者 Jesse Vincent，31 万星，MIT。它是一套写代码的规矩：先问清楚要做什么，写成计划，测试先行地实现，做完自己验证、找人 review。这些规矩拆成了 15 个 skill，模型在对应的时刻自己去读。

## 装了它，Codex 会怎么做

你说「帮我加个导出功能」，没装它的 Codex 多半直接开始改代码。装了它，Codex 会先问你几个问题、给出两三种做法让你选，把选定的方案写成计划文件，再一步步按计划做，每一步先写测试，最后跑一遍测试才说做完了。

先问问题的这一步（`brainstorming`），作者在 7.0.1（10 月 21 日）和 7.0.2（11 月 3 日）里调过两次，因为不少人反映 7.0.0 默认问的问题很快就让人烦、太套路。现在它先读项目里已有的文件，只问答案会改变做法、又没有现成答案的问题，其余的直接给出它的打算让你改；你没说为什么要做，它先问这个；有几种做法可选时，列出来并推荐一个（[更新说明](https://github.com/obra/superpowers/blob/main/RELEASE-NOTES.md)）。

## 15 个 skill

「正文」是用到时 Codex 要读的 `SKILL.md` 长度。11 月 14 日按 7.0.2 量的；有变化的，括号里是 10 月 18 日 7.0.0 的数。

| skill | 做什么 | 什么时候触发 | 正文（字符） |
|---|---|---|---|
| `using-superpowers` | 告诉模型：回答之前先看有没有该用的 skill | **每次对话开始** | 3,180 |
| `brainstorming` | 动手前先问需求、给几种方案让你选 | 要做新功能、改行为、做任何「新东西」之前 | 15,190（10,073） |
| `writing-plans` | 把方案写成一步步的计划文件 | 需求定了、开始改代码之前 | 10,317 |
| `executing-plans` | 自己按计划一步步做 | 有计划、在当前会话里自己实现 | 18,258（20,249） |
| `subagent-driven-development` | 每个任务派一个子 agent 去做，主会话负责检查 | 有计划、任务之间互不依赖 | 19,625（32,358） |
| `dispatching-parallel-agents` | 几件不相干的事同时派出去 | 有两件以上能并行的独立任务 | 6,052 |
| `test-driven-development` | 先写会失败的测试，再写代码让它通过 | 写任何功能或修 bug 之前 | 9,556 |
| `systematic-debugging` | 先找到根因再修，不靠猜 | 遇到 bug、测试失败、行为不对 | 9,506 |
| `verification-before-completion` | 说「好了」之前必须先跑命令看结果 | 准备说完成、提交、开 PR 之前 | 3,598 |
| `requesting-code-review` | 做完一块找 reviewer 检查 | 完成任务、合并之前 | 2,970 |
| `receiving-code-review` | 收到 review 意见先核实，不盲目照改 | 收到 review 反馈时 | 6,165 |
| `using-git-worktrees` | 在独立的工作目录里开发，不弄乱当前目录 | 开始一个功能、执行计划之前 | 6,803 |
| `finishing-a-development-branch` | 做完以后决定合并、开 PR 还是保留 | 实现完、测试都过了 | 7,729 |
| `writing-skills` | 教你写自己的 skill，并且先测试再发布 | 写或改 skill 的时候 | 16,698（26,523） |
| `diagnosing-superpowers` | 一次会话跑歪了，查原因、写 bug 报告 | 你想知道「为什么这么慢、这么贵」时 | 6,920（6,883） |

15 个的说明加起来，让每次请求的技能清单多 **3,506 字符**（10 月 18 日在 Codex CLI 0.163.0 上量的；7.0.2 的 15 条说明和 7.0.0 一字不差）。

## 在 Codex 里装

它在 OpenAI 官方的插件市场里，不用加第三方市场：

- 桌面版：左边栏 Plugins → Coding → Superpowers → 点 `+`。
- CLI：输入 `/plugins`，搜 `superpowers`，选 Install Plugin。

给 Codex 的插件包里 hooks 是空的（它给 Claude Code 和 Cursor 准备了 hooks），所以装完不用去 `/hooks` 确认什么。只想要其中几个，也可以把对应的文件夹从仓库的 `skills/` 复制到 `~/.agents/skills/`。

**要用子 agent 的那两个，得先开一个开关。** 仓库里给 Codex 的说明写着，`subagent-driven-development` 和 `dispatching-parallel-agents` 要用多 agent 功能，需要在 `~/.codex/config.toml` 里加：

```toml
[features]
multi_agent = true
```

没开的话，Codex 会退回 `executing-plans`，自己一步步做。

## 它会多花多少

- 每次请求：技能清单多 3,506 字符。
- 每次对话开头：`using-superpowers` 的说明要求先读它，正文 3,180 字符。
- 用到时：大的几个一次就是上万字符。7.0.2 里是 `subagent-driven-development` 19,625、`executing-plans` 18,258、`writing-skills` 16,698、`brainstorming` 15,190。7.0.1 把前三个删短了（7.0.0 时是 32,358、20,249、26,523）：`subagent-driven-development` 和 `writing-skills` 把细节挪进单独的文件，用到那一步再读；`executing-plans` 删了一段重复的示例。`brainstorming` 调过两次，从 10,073 变长了。
- 子 agent：每派一个子 agent，它都要从头读需要的上下文、自己跑一遍。仓库里特意提醒，派子 agent 时要给它干净的上下文，默认设置会把整段对话都复制过去。并行做几件事，额度就按几份算。

Plus 的额度本来就紧，每 5 小时有上限。在 Plus 上用它，尽量别开子 agent 那两个。Pro 没有 5 小时限制，只算每周额度（官方定价页，10 月 18 日核对）；美国时间 10 月 29 日起 Pro 的 Codex 额度翻倍，Pro 100 约为 Plus 的 10 倍（Codex 负责人 Tibo 的说法）。

## 什么时候值得，什么时候别用

值得用的时候：
- 一个要改好几个文件的功能，希望 AI 先和你对齐再动手。
- 一个查了很久没头绪的 bug。`systematic-debugging` 要求先复现、找根因，比让它随便改几行试试靠谱。
- 你经常被 AI 的「已经修好了」骗。`verification-before-completion` 要求它拿出跑过的命令和结果再说。

不该用的时候：
- 改一行配置、问个用法、写个一次性小脚本。它会坚持先讨论、先写计划，小事也走完整流程，又慢又费额度。
- 你已经有自己的一套流程，团队规范写在 `AGENTS.md` 里。两套规矩会打架。

折中的办法是只留常用的几个。比如不写 skill 的人用不到 `writing-skills`，不排查 superpowers 本身的用不到 `diagnosing-superpowers`，Plus 用户可以先停掉子 agent 那两个。在 `~/.codex/config.toml` 里按路径停用：

```toml
[[skills.config]]
path = "/你的路径/superpowers/skills/writing-skills/SKILL.md"
enabled = false
```

插件装的，文件在插件的缓存目录里。运行 `codex debug prompt-input "hi"`，输出里有一张 Skill roots 表，`r0`、`r1` 这些对应的就是实际文件夹，拼上 skill 后面的短路径就是要填的 `path`。

## 回到总览

[Codex Skills 中文指南](../README.md)
