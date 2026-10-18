# Codex Skills 中文指南：值得装的 skill，和装之前要知道的事（2026）

> **最后核对：2026 年 10 月 18 日**（北京时间）· Codex CLI 0.163.0 · macOS
> 这里收的每个 skill，我们都看过源码，装进 Codex 确认能认到，量了它占多少提示词。没有跑任务的会写明「没有跑任务」。
> 觉得有用就点右上角 ⭐ Star，下次挑 skill 不用再翻十篇清单文章。

网上的 skill 清单很多，大多是英文仓库的标题翻译一遍。真装到 Codex 里，常见的坑是这几个：装的文件夹不对，Codex 根本不认；装了一堆，每次请求都多带两万多字的技能说明；插件里带着 hooks，每发一句话都在跑你没看过的脚本；还有人把许可证不允许外用的 skill 也推荐给你。

下面先讲这几件事，再按用途推荐 10 组 skill。

**要用 Codex，先得有 ChatGPT 账号的套餐。** Free 和 Go 只能在桌面版用 GPT-5.6 Terra、GPT-6 Luna，额度很少；GPT-6.1 Sol、Astra 和 CLI 要 Plus 起步，skill 和子 agent 用多了额度消耗也快。没有海外卡，可以微信付款，由 [AONIR](https://aonir.com/?utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=brand_home) 充值到你自己的 ChatGPT 账号：

| 套餐 | 微信付款 | 官方价 | Codex 额度 |
|---|---|---|---|
| [ChatGPT Plus](https://aonir.com/chatgpt-plus/?utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=top_plus) | **¥168 / 月** | $20 | 基准，每 5 小时和每周各有上限 |
| [ChatGPT Pro 5×](https://aonir.com/chatgpt-pro/?plan=5x&utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=top_pro_5x) | **¥730 / 月** | Pro 100，$100 | Plus 的 5 倍，没有 5 小时限制 |
| [ChatGPT Pro 10×](https://aonir.com/chatgpt-pro/?plan=20x&utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=top_pro_10x) | **¥1350 / 月**，客服开通（账号需是免费版） | Pro 200，$200 | Plus 的 10 倍，没有 5 小时限制 |

Codex 装不上、登录不了、报错，看我们另一个仓库：[Codex 国内使用与报错速查](https://github.com/Avarce/codex-china-guide)。

## 目录

- [先看这 5 件事](#先看这-5-件事)
- [推荐清单](#推荐清单)
- [暂不推荐的几类](#暂不推荐的几类)
- [怎么停用、卸载](#怎么停用卸载)
- [深度解读：superpowers](deep-dives/superpowers.md)
- [我们怎么测的](#我们怎么测的) · [更新日志](CHANGELOG.md) · [推荐一个 skill](#推荐一个-skill)

## 先看这 5 件事

### 1. Codex 从哪几个文件夹读 skill

官方文档写的是 `~/.agents/skills`（你自己的）和项目里的 `.agents/skills`（跟着项目走）。我们在 CLI 0.162.1 和 0.163.0 上各试了一遍，结果一样：

| 放在哪 | Codex 认不认 |
|---|---|
| `~/.agents/skills/` | 认 |
| `~/.codex/skills/` | 认（官方的 `$skill-installer` 装到这里） |
| 项目里的 `.agents/skills/` | 认 |
| 项目里的 `.codex/skills/` | 认 |
| `~/.claude/skills/` | **不认** |
| 项目里的 `.claude/skills/` | **不认** |

从 Claude Code 搬过来的 skill，要复制或者软链接到 `~/.agents/skills/`，原地放着 Codex 看不到。文件格式不用改，两边都是一个文件夹加一个 `SKILL.md`。

用 `npx skills add` 装的，要带上 `--agent codex`。我们试了 skills 1.7.2：`--agent codex --global` 会装到 `~/.agents/skills/`。

### 2. 怎么确认装上了

- 桌面版：左边栏的 **Skills** 里能看到。
- CLI：输入 `/skills`，或者打一个 `$` 看补全列表里有没有。
- 不想开对话、不想耗额度，可以用这条命令看 Codex 实际会带给模型的技能清单，它只在本地生成，不会发请求：

```bash
codex debug prompt-input "hi" | grep -o "archify: [^\\]*"
```

把 `archify` 换成你要查的 skill 名，有输出就是认到了。搜不到，先看文件夹放对没有，再看 `SKILL.md` 开头有没有 `name` 和 `description`：官方要求两项都要有。有的 skill 在 `agents/openai.yaml` 里写了 `allow_implicit_invocation: false`，它就不会出现在这份清单里，只能用 `$名字` 手动叫出来，这是作者故意的。

### 3. 装得越多，每次请求带得越多

Codex 每次请求都会把所有 skill 的名字、说明和路径一起发给模型，正文要等用到时才读。官方文档说，这份清单默认最多占上下文的 2%，不知道上下文多大时按 8,000 字符算；装太多时先截短说明，再不行就不列出，并给出提示。

我们量的数字（CLI 0.163.0，默认模型）：

- 什么都不装，Codex 自带的 5 个 skill 清单约 **2,000 字符**。
- 把这页推荐的全装上（57 个），清单变成约 **21,400 字符**。这次没有被截短，但每次请求的输入里都带着这两万多字。
- 单个仓库差别很大：shuorenhua 只多 348 字符，nature-skills 全装要多 8,240 字符。

所以只装用得上的。项目专用的 skill 放项目里的 `.agents/skills/`，别全放在 `~/.agents/skills/`；暂时不用的，按[下面](#怎么停用卸载)停用。

### 4. skill、插件和 hooks 的区别

- **skill**：一个文件夹，`SKILL.md` 里写着什么时候用、怎么做，可能带几个脚本。
- **插件**：打包好的一组 skill，还可能带 MCP 连接和 hooks，用 `codex plugin add` 装，或者在桌面版的 Plugins 里装。
- **hooks**：在你发消息、调用工具、会话开始这些时刻自动运行的脚本，不用模型决定。

hooks 最该小心。新装的插件带 hooks 时，Codex 会让你先在 `/hooks` 里看一遍、确认信任才生效。别直接点信任，打开它要运行的脚本看一眼。本页推荐的里面，ponytail 和 planning-with-files 带 hooks；superpowers 给 Codex 的插件包里 hooks 是空的。

### 5. 用别人的 skill 前，看三样

1. 会不会跑脚本、装依赖。有 `scripts/` 文件夹的，或者正文里写着 `pip install`、`npx` 的，用的时候会在你电脑上执行。skills 安装器装完也会提醒你：skill 用的是 agent 的全部权限。
2. 要不要另外的 API Key。比如 OpenAI 官方目录里的 speech（文字转语音）和 transcribe（语音转文字）要设 `OPENAI_API_KEY`，按 API 另外计费，不走你的 ChatGPT 套餐额度。
3. 许可证。大部分是 MIT、Apache-2.0，随便用；有的只是公开源码、不许外用，见[暂不推荐](#暂不推荐的几类)。

## 推荐清单

按用途分。「清单多占」是装上后每次请求多带的字符数，「正文」是用到时 Codex 才读的 `SKILL.md` 长度，都是我们 10 月 18 日在 CLI 0.163.0 上量的。

| 用途 | 推荐 | 清单多占 | 要注意 |
|---|---|---|---|
| 官方的 | [OpenAI 官方目录](#1-openai-官方目录先看自带的再挑几个装) | 每个 300 到 500 | speech、transcribe 要 API Key |
| 写代码的流程 | [superpowers](#2-superpowers先想清楚再写代码) | 3,506（15 个） | 每次对话开头都会先读它的说明 |
| 少写代码 | [ponytail](#3-ponytail能不写的代码就不写) | 2,059（6 个） | 带 hooks |
| 长任务不断线 | [planning-with-files](#4-planning-with-files长任务把进度写进文件) | 837 | 带 7 个 hooks |
| 画架构图、流程图 | [archify](#5-archify把代码和想法画成图) | 1,048 | 要 Node 18+ |
| 前端好看一点 | [taste-skill](#6-taste-skill让前端别那么像-ai-做的) | 1,067 | 装的名字是 `design-taste-frontend`；要 Node 22+ 和 Chrome |
| 做 PPT | [guizang-ppt / codex-ppt](#7-做-pptguizang-ppt-和-codex-ppt) | 770 / 315 | 别用 Anthropic 的 pptx |
| 写论文、科研图 | [nature-skills](#8-nature-skills写论文和科研绘图) | 全装 8,240 | 只装用得上的几个 |
| 中文去 AI 味 | [shuorenhua](#9-shuorenhua中文去-ai-味) | 348 | 英文用 humanizer |
| 公众号、小红书 | [baoyu-skills（挑几个）](#10-baoyu-skills公众号和小红书只挑几个) | 4 个共 1,751 | 别装 danger 开头的 |

### 1. OpenAI 官方目录：先看自带的，再挑几个装

[openai/skills](https://github.com/openai/skills) · 2.8 万星 · 每个 skill 单独是 Apache-2.0 · 最近更新 9 月 8 日

OpenAI 维护的 skill 目录。Codex 自带 5 个：画图的 `imagegen`、查 OpenAI 文档的 `openai-docs`、做插件的 `plugin-creator`、做 skill 的 `skill-creator`、装 skill 的 `skill-installer`。另外有 39 个精选的可以按需装。

值得装的几个：`playwright`（在终端里操作真实浏览器，填表、截图、调试页面）、`pdf`（读和生成 PDF，按页渲染出来检查版式）、`gh-fix-ci`（GitHub Actions 挂了，让它读日志修）、`security-best-practices`（按语言和框架做安全检查，你明确要求时才触发）。用 Figma、Linear、Notion、Sentry 的，各有对应的一组。

装法：在 Codex 里输入

```text
$skill-installer playwright
```

会装到 `~/.codex/skills/`，装完一般自动认到，不行就重启 Codex。

实测：playwright、pdf 装上能认到，清单分别多 422、447 字符，正文 3,771、2,517 字符。没有跑任务。

注意：playwright 要电脑上有 `npx`（装 Node.js 就有）；pdf 会用 `uv pip install` 装 reportlab、pdfplumber、pypdf，渲染页面还要 Poppler。speech、transcribe 要 `OPENAI_API_KEY`，按 API 另外计费。

### 2. superpowers：先想清楚，再写代码

[obra/superpowers](https://github.com/obra/superpowers) · 30 万星 · MIT · 最近更新 10 月 10 日

一整套写代码的流程，15 个 skill。动手前先问清需求、出方案，写计划，按计划测试先行地实现，最后验证、做 code review。修 bug 有专门的排查流程。

适合要做一个功能、改一块不小的代码，又不想让 AI 一上来就乱改的时候。改一行配置、问个用法就别用它：它会坚持先讨论、先写计划，小事也走一遍流程。

装法：它在 OpenAI 官方的插件市场里。桌面版：左边栏 Plugins → Coding 分类里的 Superpowers → 点 `+`。CLI：输入 `/plugins`，搜 `superpowers`，选 Install Plugin。

实测：15 个 skill 都能认到，清单多 3,506 字符。其中 `using-superpowers` 的说明要求每次对话开始都先读它，正文 3,180 字符；最长的 `subagent-driven-development` 有 32,358 字符，用到时才读。Codex 里只做了安装和加载测试；我们在 Claude Code 里用这套流程已经有一段时间。

注意：有两个 skill 会开子 agent 并行干活，额度会用得明显更快。详细的拆解、哪些 skill 值得留、哪些可以停用，见[深度解读](deep-dives/superpowers.md)。

### 3. ponytail：能不写的代码就不写

[DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) · 16.7 万星 · MIT · 最近更新 10 月 18 日

让 AI 写代码前先过几道问题：这东西真的需要吗？项目里是不是已经有了？标准库能不能做？能少写就少写。另外带几个命令：`ponytail-review` 审当前改动，`ponytail-audit` 审整个仓库，`ponytail-debt` 把代码里标了 `shortcut:` 的地方收成一张待还清单。

适合嫌 AI 动不动就加依赖、写一大堆封装的人。要它从零搭一套完整架构的时候，就先关掉。

装法：

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

它带两个 hooks。装完在 Codex 里打开 `/hooks`，确认信任前先看它们要跑的脚本，然后开一个新对话。

实测：6 个 skill 都能认到，清单多 2,059 字符，6 个正文加起来 20,999 字符。hooks 没有测。没有跑任务。

注意：它会在代码里写 `shortcut:` 注释，不想要可以在 `AGENTS.md` 里说明。作者特别提醒只从 GitHub 上的 `DietrichGebert/ponytail` 或 npm 的 `@dietrichgebert/ponytail` 装，它从不带 `.exe`、`.dll`，带了的就不是他发的。

### 4. planning-with-files：长任务，把进度写进文件

[OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files) · 2.7 万星 · MIT · 最近更新 10 月 17 日

做多步骤的大任务时，让 AI 把计划、查到的东西、做到哪一步分别写进 `task_plan.md`、`findings.md`、`progress.md`。上下文压缩了、会话断了、换个对话，它都能从文件里接着做。

适合一次要跑几十步、跨好几个对话的任务。Codex 的长会话经常要压缩上下文，压缩完它还能从文件里找回进度。一两步就完的小事用不着，多写三个文件反而啰嗦。

装法（只装 skill）：

```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files --agent codex -g --yes
```

想要自动提醒和收尾检查，还得装它的 hooks，步骤见作者的 [Codex 说明](https://github.com/OthmanAdi/planning-with-files/blob/main/docs/codex.md)。已经有 `~/.codex/hooks.json` 的，别直接覆盖，要把它的 7 项手动合进去。

实测：装上能认到，清单多 837 字符；正文 39,713 字符，在这页里算长的，用一次就读一次。hooks 没有测。没有跑任务。

注意：7 个 hooks 分别在会话开始、你发消息、调用工具前后、请求权限、压缩上下文、结束时运行脚本，确认信任前一个个看。仓库里也有中文版 skill（`planning-with-files-zh`）。

### 5. archify：把代码和想法画成图

[tt-a1i/archify](https://github.com/tt-a1i/archify) · 8.5 万星 · MIT · 最近更新 10 月 18 日

把一段描述、一个计划或者整个代码库画成图，架构图、流程图、时序图、数据流、状态机、ER 图都行。输出一个独立的 HTML 文件，浏览器打开，节点能点开看细节。

接手陌生项目想先看清结构时最有用，写文档配图、给不懂代码的人讲方案也用得上。

装法：

```bash
npx skills add tt-a1i/archify --skill archify --agent codex -g --yes --copy
```

我们用这条命令在干净的环境里装过，会放到 `~/.agents/skills/archify/`。

实测：装上能认到，清单多 1,048 字符，正文 15,723 字符。没有跑任务。

注意：要 Node.js 18 以上。中文说明见仓库里的 `README_ZH.md`。

### 6. taste-skill：让前端别那么像 AI 做的

[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) · 9.9 万星 · MIT · 最近更新 10 月 18 日

给 AI 一套做网页的审美规则：字体、间距、配色、版式往成熟设计靠，少出那种一眼就是 AI 生成的渐变卡片页。仓库里有十几个变体，默认的那个叫 `design-taste-frontend`。

适合做落地页、个人站、产品介绍页；后台管理这类只求能用的页面用不着。

装法（只装默认的那个）：

```bash
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend" --agent codex -g --yes
```

也可以当插件装：`codex plugin marketplace add Leonxlnx/taste-skill`，再 `codex plugin add taste-skill@taste-skill`，装完开新会话。

实测：装上能认到（名字是 `design-taste-frontend`，不是文件夹名），清单多 1,067 字符，正文 29,282 字符。没有跑任务。

注意：脚本要 Node 22 以上，电脑上要有 Chrome、Edge 或 Chromium；找配图要联网。仓库里 `image-to-code-skill` 这类变体会先生成设计图，会用到图片生成额度。

### 7. 做 PPT：guizang-ppt 和 codex-ppt

搜「Codex 做 PPT」，很多文章教你装 Anthropic 的 `pptx`。那个许可证不允许拿出去用（见[暂不推荐](#暂不推荐的几类)）。下面两个可以放心装，路子不一样：

**[op7418/guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill)** · 2.8 万星 · AGPL-3.0 · 最近更新 8 月 7 日

做的是网页 PPT：一个 HTML 文件，横向翻页，带演讲者视图和讲稿备注，文字能改。在 Codex 里还会问你要不要用 GPT-Image 配图，也能顺手出公众号、小红书封面。

```bash
npx skills add https://github.com/op7418/guizang-ppt-skill --skill guizang-ppt-skill --agent codex -g --yes
```

实测：装上能认到，清单多 770 字符，正文 25,146 字符。没有跑任务。AGPL 只管改了再分发的情况，自己用不受影响。

**[ningzimu/codex-ppt-skill](https://github.com/ningzimu/codex-ppt-skill)** · 7,000 星· MIT · 最近更新 10 月 13 日

每页直接生成成图片，再拼成 PPTX，风格统一、好看，但文字不能直接改。要改字，作者另有一个 [image-to-editable-ppt-skill](https://github.com/ningzimu/image-to-editable-ppt-skill)，把图片页转回可编辑的 PPT。

```bash
npx -y skills@latest add ningzimu/codex-ppt-skill --skill codex-ppt --agent codex --global --yes
```

实测：装上能认到，清单多 315 字符，正文 10,391 字符。没有跑任务。

注意：codex-ppt 每页都要生成图片，确认样张后还会一页开一个子 agent 并行做，额度用得快。要能改字、要演示，选 guizang；要成品图好看、拿去发，选 codex-ppt。

### 8. nature-skills：写论文和科研绘图

[Yuan1z0825/nature-skills](https://github.com/Yuan1z0825/nature-skills) · 4.9 万星 · Apache-2.0 · 最近更新 10 月 16 日

中文作者做的一整套科研 skill，22 个：查文献核引用、精读论文出阅读卡、按 Nature 风格润色、画期刊配图、论文转中文 PPT、写数据可用性声明，甚至把论文改成专利草稿。

适合研究生和写论文的人，但别一次全装。

装法：只装用得上的。比如只要画图，装进当前项目：

```bash
npx skills add Yuan1z0825/nature-skills --agent codex --skill nature-figure --yes --copy
```

加 `--global` 就装到你自己的 `~/.agents/skills/`，所有项目都能用。装 `nature-writing`、`nature-polishing`、`nature-reader`、`nature-paper2ppt` 这几个时，要把 `nature-shared` 一起装上，它们要读里面的共享资料。

实测：全装是 23 个，Codex 认到 22 个，清单多 8,240 字符，是这页最多的。没列出的那个 `nature-shared` 是给其他几个当共享资料的，作者在它的 `agents/openai.yaml` 里关掉了自动触发，属于正常。没有跑任务。

注意：作者自己也建议按需装。有几个要联网下载文献；`nature-downloader` 涉及知网等机构访问，用自己有权限的账号。

### 9. shuorenhua：中文去 AI 味

[MrGeDiao/shuorenhua](https://github.com/MrGeDiao/shuorenhua) · 2,152 星· MIT · 最近更新 10 月 18 日

改写或审读中文文字，去掉模板腔：没话找话的开头、排比三连、空洞的总结句。保留事实、术语和「谁做了什么」，也可以只标问题不改。

写 README、公众号、周报、产品说明时用得上。

装法：作者推荐放进项目里长期用，在你的项目根目录运行：

```bash
git clone --depth 1 https://github.com/MrGeDiao/shuorenhua.git /tmp/shuorenhua
mkdir -p .agents/skills/shuorenhua/references
cp /tmp/shuorenhua/SKILL.md .agents/skills/shuorenhua/
cp /tmp/shuorenhua/references/editing-guide.md /tmp/shuorenhua/references/examples.md .agents/skills/shuorenhua/references/
```

作者的说明是把文件放在项目根目录的 `shuorenhua/`，再在 `AGENTS.md` 里写一句什么时候用它；放进 `.agents/skills/` 是让 Codex 直接把它认成 skill，两种都行。

实测：装上能认到，清单只多 348 字符，正文 3,511 字符，是这页最轻的。没有跑任务。

英文用 [blader/humanizer](https://github.com/blader/humanizer)（5.8 万星，MIT），按维基百科总结的「AI 写作迹象」改英文：`npx skills add blader/humanizer --global --agent codex --yes`。实测清单多 569 字符，正文 32,309 字符。

### 10. baoyu-skills：公众号和小红书，只挑几个

[JimLiu/baoyu-skills](https://github.com/JimLiu/baoyu-skills) · 2.7 万星 · MIT · 最近更新 9 月 10 日

宝玉（JimLiu）的一组内容创作 skill，21 个：排版 Markdown、转成公众号能用的 HTML、生成封面和文章配图、小红书图卡、信息图、漫画、翻译、发公众号和微博。

装法：作者自己也说别全装，用到哪个装哪个。做公众号、小红书常用的四个：

```bash
npx skills add jimliu/baoyu-skills --agent codex -g \
  --skill baoyu-format-markdown --skill baoyu-markdown-to-html \
  --skill baoyu-cover-image --skill baoyu-xhs-images --yes
```

也可以把对应的文件夹直接复制到项目的 `.agents/skills/`。

实测：上面 4 个都能认到，清单共多 1,751 字符，正文共 69,961 字符。没有跑任务。

注意：封面、配图在 Codex 里会优先用自带的 `imagegen` 出图，用的是你的额度。**名字带 `danger` 的别装**：`baoyu-danger-gemini-web`、`baoyu-danger-x-to-markdown` 走的是逆向出来的网页接口，作者自己就标了 danger，账号出了问题没人负责。`baoyu-post-to-wechat` 要公众号的接口权限，或者用 Chrome 远程调试登录。

## 暂不推荐的几类

**Anthropic 的 docx、pptx、xlsx、pdf 四个 skill。** [anthropics/skills](https://github.com/anthropics/skills) 里大部分是 Apache-2.0，但这四个不是：README 写的是公开源码、不开源，每个文件夹里的许可证不允许把它们复制、保存到 Anthropic 自己的服务以外。放进 `~/.agents/skills` 给 Codex 用，就在这个范围外。做 PPT 用上面的 guizang 或 codex-ppt，做 PDF 用 OpenAI 官方的 `pdf`。同一个仓库里的 `frontend-design`、`webapp-testing`、`skill-creator`、`mcp-builder` 是 Apache-2.0，可以用。

**逆向、渗透类。** 比如最近很火的 [morluto/rea](https://github.com/morluto/rea)（31.7 万星）。OpenAI 帮助中心写明，风险较高的网络安全请求可能被拒，或者要额外核查；openai/codex 里 10 月 16 日就有人报，正常的本地测试被误判拦下（[#54588](https://github.com/openai/codex/issues/54588)）。这类 skill 本身不违法，但在 Codex 里用之前，先想清楚自己的场景会不会被拦。

**走逆向接口的 skill。** 名字或说明里写着 reverse-engineered API、unofficial API 的，账号风险自己担。

**账号池、多账号切换、中转站。** OpenAI 的条款不允许共享账号、转售访问权限，被限了没地方说理。

**一行 `curl … | bash` 装的。** 先把脚本下载下来看一遍再运行。比如 [Understand-Anything](https://github.com/Egonex-AI/Understand-Anything) 的安装脚本会把整个仓库克隆到 `~/.understand-anything/repo`，再建软链接；内容本身没问题，但这一步你应该知道它在干什么。

## 怎么停用、卸载

- **单个 skill**：删掉它的文件夹就是卸载。只想暂时不用，在 `~/.codex/config.toml` 里加：

```toml
[[skills.config]]
path = "/Users/你的用户名/.agents/skills/某个skill/SKILL.md"
enabled = false
```

改完重启 Codex。

- **插件**：`codex plugin remove 插件名`；加过的插件市场用 `codex plugin marketplace remove 名字` 去掉。
- **hooks**：在 `/hooks` 里看现在加载了哪些；单独装的 hooks 在 `~/.codex/hooks.json` 里删对应的项。

## 我们怎么测的

- **环境**：macOS，Codex CLI 0.163.0（文件夹那张表另外在 0.162.1 上试过）。每次测试都用一个临时的空 HOME，不碰我们自己的 Codex 配置，也不登录。
- **认不认得到、占多少**：把 skill 复制进临时 HOME 的 `.agents/skills/`，运行 `codex debug prompt-input "hi"`，看输出的技能清单里有没有它，比装之前多了多少字符。这条命令只在本地生成，不发请求，不耗额度。
- **看源码**：每个推荐的仓库都克隆下来读过 `SKILL.md`、脚本和 hooks，查有没有联网下载、装依赖、读密钥、要求模型无条件服从这类写法。
- **没做的**：没有在 Codex 里真正跑任务，效果好坏以作者的说明和你自己试的为准。插件的 hooks 也没有实际触发。
- 星数、许可证、最近更新日期是 10 月 18 日从 GitHub 读的。

## 推荐一个 skill

觉得哪个 skill 该收、哪条写得不对，[开一个 Issue](../../issues/new) 告诉我们：贴上仓库链接、你用它做什么、在 Codex 里装没装上。我们看过源码、装一遍之后补进来，记在[更新日志](CHANGELOG.md)里。

## 转载与许可

本页文字采用 [CC BY 4.0](LICENSE) 许可：可以转载、翻译、改编，注明出处并附上本仓库链接即可。文中提到的每个 skill 都归原作者所有，以各自仓库的许可证为准。Codex、ChatGPT、OpenAI 是 OpenAI 的商标，本仓库与 OpenAI 没有隶属关系。
