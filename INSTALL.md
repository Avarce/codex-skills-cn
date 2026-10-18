# 在 Codex 里装 skill：放哪、怎么确认、怎么关

> 核对：2026 年 10 月 18 日 · Codex CLI 0.162.1、0.163.0 · macOS

## 放在哪个文件夹

官方文档写的是 `~/.agents/skills`（你自己的，所有项目都能用）和项目里的 `.agents/skills`（跟着项目走）。我们在两个版本上各试了一遍：

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

Codex 每次请求都会带上所有 skill 的名字和说明，正文等用到时才读。官方说这份清单默认最多占上下文的 2%，装太多时会先截短说明，再不行就不列出。

我们量的（CLI 0.163.0，默认模型）：

| 装了什么 | 清单多带多少字符 |
|---|---|
| 什么都不装（Codex 自带 5 个） | 约 2,000（底数） |
| shuorenhua | 348 |
| codex-ppt | 315 |
| playwright / pdf | 422 / 447 |
| humanizer | 569 |
| guizang-ppt | 770 |
| planning-with-files | 837 |
| archify | 1,048 |
| taste-skill | 1,067 |
| baoyu-skills 里的 4 个 | 1,751 |
| ponytail（6 个） | 2,059 |
| superpowers（15 个） | 3,506 |
| nature-skills 全装（23 个） | 8,240 |
| 精选页上的全部（57 个） | 清单总共约 21,400 |

全装上这次没有被截短，但每次请求都多带两万多字。所以只装用得上的：项目专用的放项目里的 `.agents/skills/`，暂时不用的按下面的办法关掉。

## 插件和 hooks

插件是打包好的一组 skill，用 `codex plugin add` 或桌面版的 Plugins 装。有的插件带 hooks，就是在你发消息、调用工具、会话开始这些时刻自动运行的脚本。新装带 hooks 的插件，Codex 会让你先在 `/hooks` 里确认，确认前打开它要跑的脚本看一眼。

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

每次都用一个临时的空 HOME，不碰自己的 Codex 配置、也不登录：把 skill 复制进去，运行上面那条 `codex debug prompt-input`，看清单里有没有它、多了多少字符。推荐的仓库都克隆下来读过 `SKILL.md`、脚本和 hooks。没有在 Codex 里真正跑任务，效果以作者的说明和你自己试的为准。

[回到精选列表](README.md)
