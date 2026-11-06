# 在 Codex 里装 skill：放哪、怎么确认、怎么关

> 核对：2026 年 11 月 7 日 · Codex CLI 0.167.0 · macOS（10 月 19 日在 0.162.1、0.163.0 上核对过一遍）

## 放在哪个文件夹

官方文档写的是 `~/.agents/skills`（你自己的，所有项目都能用）和项目里的 `.agents/skills`（跟着项目走）。我们在 0.162.1、0.163.0、0.167.0 上各试了一遍，结果一样：

| 放在哪 | Codex 认不认 |
|---|---|
| `~/.agents/skills/` | 认 |
| `~/.codex/skills/` | 认（官方的 `$skill-installer` 装到这里） |
| 项目里的 `.agents/skills/` | 认 |
| 项目里的 `.codex/skills/` | 认 |
| `~/.claude/skills/`、项目里的 `.claude/skills/` | 不认 |

从 Claude Code 搬过来的 skill，复制或软链接到 `~/.agents/skills/` 就行，文件不用改。用 `npx skills add` 装的，记得带 `--agent codex`，它会装到 `~/.agents/skills/`。

## 怎么确认装上了

- 桌面版：左边栏 **Skills** 里能看到。
- CLI：输入 `/skills`，或者打一个 `$` 看补全列表。
- 不开对话、不耗额度：下面这条命令只在本地生成 Codex 要带给模型的技能清单，不发请求。把 `archify` 换成你要查的名字，有输出就是认到了。

```bash
codex debug prompt-input "hi" | grep -o "archify: [^\\]*"
```

搜不到的话，先看文件夹对不对，再看 `SKILL.md` 开头有没有 `name` 和 `description`。少数 skill 在 `agents/openai.yaml` 里关掉了自动触发，它不会出现在清单里，只能用 `$名字` 叫出来。

## 装多了会怎样

Codex 每次请求都会带上所有 skill 的名字和说明，正文等用到时才读。官方说这份清单默认最多占上下文的 2%（不知道上下文多大时按 8,000 字符算），装太多时会先截短说明，再不行就不列出。

我们量的（默认模型，每组单独装在一个空的 HOME 里）：

| 装了什么 | 11 月 7 日（0.167.0） | 10 月 19 日（0.163.0） |
|---|---|---|
| 什么都不装（底数） | 约 2,000（自带 5 个，4 个进清单） | 约 2,000（自带 5 个） |
| shuorenhua | 124 | 348 |
| codex-ppt | 185 | 315 |
| playwright / pdf | 292 / 317 | 422 / 447 |
| frontend-design | 297 | 427 |
| guizang-ppt | 344 | 770 |
| humanizer | 432 | 569 |
| planning-with-files | 707 | 837 |
| archify | 817 | 963 |
| taste-skill | 841（插件装是 886） | 1,175 |
| agent-browser | 1,014 | 1,144 |
| 飞书 lark-cli 里的 4 个（本页那条命令） | 1,055 | 未收录 |
| hyperframes 核心 | 1,378（4 个） | 6,925（10 个） |
| baoyu-skills 里的 4 个 | 1,551 | 1,751 |
| ponytail（6 个） | 2,020 | 2,059 |
| superpowers（15 个） | 3,370 | 3,506 |
| mattpocock/skills（27 个，11 个进清单） | 3,927 | 2,823 |
| 飞书 lark-cli 全装（28 个） | 7,111 | 未收录 |
| nature-skills 全装 | 7,510（24 个，23 个进清单） | 8,240（23 个） |

0.167.0 的清单开头多了一段「skill 放在哪个文件夹」，写的是完整路径，所以字数跟你的用户文件夹路径有关。11 月 7 日这一列按 `/Users/you` 这么长的路径折算过；每多一个文件夹就多一行路径，mattpocock/skills 的 11 个各占一行，所以涨得多。两列的清单格式不一样，不能直接比，变少也不代表 skill 的说明变短了。

手动叫的 skill（`agents/openai.yaml` 里关了自动触发的）不进清单，也就不占这份字数。mattpocock/skills 的 27 个里有 16 个是这种，Codex 自带的 `review-agent` 也是。

**全装会被截短。** 11 月 7 日（0.167.0）：这页上的全装（不含飞书，superpowers、nature-skills 全套和 OpenAI 那 4 个都算上）进清单 80 条，含自带的 4 条。清单总共约 19,400 字符，每条说明都不超过 188 字符，55 条被截，比如 agent-browser 的说明从 925 截到 188。再装上飞书全套 28 个，清单 108 条，每条只剩 92 字符左右，94 条被截。我们测试用的文件夹路径很长，占掉了一点额度；你电脑上的路径短一些，每条说明能多留几个字符。两次都一条没丢，但说明越短，Codex 判断什么时候该用哪个 skill 的依据就越少，自动触发可能变得不准。

10 月 19 日（0.163.0）那次：全装 96 个，79 个进清单，Codex 把 48 条较长的说明截到 240 到 550 字符，agent-browser 从 976 截到 263。再早一版只有 57 个时，全装没有截短。

所以只装用得上的：项目专用的放项目里的 `.agents/skills/`，暂时不用的按下面的办法关掉。

确实要装很多，可以调大这份清单的额度（官方写的上限是 10,000 token）。代价是每次请求都要多带这些字：

```toml
[skills]
max_context_tokens = 10000
```

我们试过：上面那 108 条，调大以后清单从约 17,200 字符涨到约 31,600 字符，每条说明最多保留约 400 字符，被截的从 94 条降到 13 条。

## 插件和 hooks

插件是打包好的一组 skill，用 `codex plugin add` 或桌面版的 Plugins 装。有的插件带 hooks，就是在你发消息、调用工具、会话开始这些时刻自动运行的脚本。新装带 hooks 的插件，Codex 会让你先在 `/hooks` 里确认，确认前打开它要跑的脚本看一眼。从 0.164 起，插件更新以后脚本有变动，Codex 会要你重新确认（[openai/codex#54619](https://github.com/openai/codex/pull/54619)）。

## 怎么关、怎么卸

- 卸载单个 skill：删掉它的文件夹。
- 暂时不用：在 `~/.codex/config.toml` 里加下面几行，改完重启 Codex。

```toml
[[skills.config]]
path = "/Users/你的用户名/.agents/skills/某个skill/SKILL.md"
enabled = false
```

- 卸载插件：`codex plugin remove 插件名`；去掉加过的插件市场：`codex plugin marketplace remove 名字`。
- hooks：在 `/hooks` 里看加载了哪些；单独装的在 `~/.codex/hooks.json` 里删对应的项。

## 我们怎么测的

每次都用一个临时的空 HOME，不碰自己的 Codex 配置、也不登录：能用命令装的，就用精选页上印的那条命令装（superpowers 和 OpenAI 官方目录要在登录后的 Codex 里装，这两个只核了能被认到），运行上面那条 `codex debug prompt-input`，看清单里有没有它、多了多少字符。每组单独量一次，再全装量一次；11 月 7 日这次，能用命令装的直接用命令装好再量；superpowers、OpenAI 那几个和 nature-skills 全装那一行，是把文件夹复制进去量的。推荐的仓库都克隆下来读过 `SKILL.md`、脚本和 hooks。没有在 Codex 里真正跑任务，效果以作者的说明和你自己试的为准。

[回到精选列表](README.md)
