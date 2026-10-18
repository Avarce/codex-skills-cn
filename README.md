# Codex Skills 中文精选：装上就好用的 skill（2026）

> **最后核对：2026 年 10 月 18 日**（北京时间）· Codex CLI 0.163.0
> 每个都看过源码，装进 Codex 试过能认到。觉得有用就点右上角 ⭐ Star，下次挑 skill 不用再到处翻。

由 [AONIR](https://aonir.com/?utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=brand_home) 整理维护。我们提供 ChatGPT 会员充值服务，和 OpenAI 没有隶属关系；这页只讲 Codex 的 skill，充值相关只放在[最后一节](#国内怎么开通-codex)。

第一次在 Codex 里装 skill，先看[安装说明](INSTALL.md)：Codex 认哪几个文件夹、怎么确认装上了、装多了有什么影响。

## 效果预览

图片都来自各个 skill 自己的仓库，点开仓库能看到更多。

<table>
<tr>
<td width="50%" valign="top"><img src="https://raw.githubusercontent.com/tt-a1i/archify/main/docs/assets/archify-readme-hero.png" alt="archify 生成的系统架构图"><br><b>archify</b>：一句话描述，画出架构图</td>
<td width="50%" valign="top"><img src="https://github.com/user-attachments/assets/8960e78c-69bb-4b7e-aa95-6fad64b70314" alt="guizang-ppt 生成的网页 PPT 页面"><br><b>guizang-ppt</b>：网页 PPT，文字能改、能演示</td>
</tr>
<tr>
<td valign="top"><img src="https://raw.githubusercontent.com/ningzimu/codex-ppt-skill/main/assets/slides_example.png" alt="codex-ppt 生成的手绘风格 PPT"><br><b>codex-ppt</b>：每页一张图，风格统一</td>
<td valign="top"><img src="https://raw.githubusercontent.com/Leonxlnx/taste-skill/main/examples/v2/night-dates.webp" alt="taste-skill 做的活动网站首页"><br><b>taste-skill</b>：做出来的网页不像 AI 模板</td>
</tr>
<tr>
<td valign="top"><img src="https://raw.githubusercontent.com/Yuan1z0825/nature-skills/main/skills/nature-figure/assets/chart-atlas/atlas-01-bar-charts.png" alt="nature-figure 的期刊风格柱状图"><br><b>nature-skills</b>：期刊风格的科研配图</td>
<td valign="top"><img src="https://raw.githubusercontent.com/JimLiu/baoyu-skills/main/screenshots/xhs-images-styles/fresh.webp" alt="baoyu-xhs-images 生成的小红书图卡" width="60%"><br><b>baoyu-skills</b>：小红书图卡，十几种风格</td>
</tr>
</table>

## 按需求挑

| 我想 | 推荐 | 你可以这样说 |
|---|---|---|
| 让 AI 先想清楚再写代码 | [superpowers](#superpowers先想清楚再写代码) | 「我想给这个项目加一个导出 CSV 的功能」 |
| 别乱加依赖、能少写就少写 | [ponytail](#ponytail能不写的代码就不写) | 「$ponytail-review 看看这次改动」 |
| 大任务做到一半不断线 | [planning-with-files](#planning-with-files长任务把进度写进文件) | 「用 planning-with-files 规划：把项目从 JavaScript 迁到 TypeScript」 |
| 看懂一个陌生项目 | [archify](#archify把代码和想法画成图) | 「用 archify 把这个仓库的架构画出来」 |
| 前端做得好看 | [taste-skill](#taste-skill让前端别那么像-ai-做的) | 「用 design-taste-frontend 给我的咖啡店做一个落地页」 |
| 做 PPT | [guizang-ppt / codex-ppt](#做-pptguizang-ppt-和-codex-ppt) | 「把这篇文章做成 10 页的 PPT」 |
| 写论文、画科研图 | [nature-skills](#nature-skills写论文和科研绘图) | 「用 nature-figure 把这组实验数据画成期刊配图」 |
| 中文去 AI 味 | [shuorenhua](#shuorenhua中文去-ai-味) | 「用 shuorenhua 改一下这段 README」 |
| 公众号、小红书配图 | [baoyu-skills](#baoyu-skills公众号和小红书) | 「用 baoyu-xhs-images 把这篇笔记做成 6 张图卡」 |
| 操作浏览器、处理 PDF | [OpenAI 官方 skill](#openai-官方目录浏览器pdfgithub) | 「$playwright 打开 localhost:3000，把注册流程走一遍并截图」 |

在 Codex 里，可以输入 `$` 加 skill 名字直接叫它，也可以直接说要做什么，Codex 会按 skill 的说明自己挑。

## 写代码

### superpowers：先想清楚，再写代码

[obra/superpowers](https://github.com/obra/superpowers) · 30 万星 · MIT

你说「帮我加个导出功能」，没装它的 Codex 多半直接开始改。装了它，Codex 会先问你几个问题、给出两三种做法让你选，把方案写成计划，再按计划一步步做，每一步先写测试，最后跑过测试才说做完。修 bug 也有专门的流程：先复现、找到原因再改。

适合要改好几个文件的功能，或者一个查了很久的 bug。改一行配置、问个用法就不用开它，它会坚持先讨论、先写计划。

装法：它在 OpenAI 官方的插件市场里。桌面版左边栏 Plugins → Coding → Superpowers → 点 `+`；CLI 里输入 `/plugins`，搜 `superpowers` 安装。

15 个 skill 各管什么、哪些可以关掉、在 Codex 里要开的开关，见[深度解读](deep-dives/superpowers.md)。

### ponytail：能不写的代码就不写

[DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) · 16.7 万星 · MIT

<img src="https://raw.githubusercontent.com/DietrichGebert/ponytail/main/assets/v5/beforeafter.png" alt="ponytail 前后对比：加一个日期选择器，没装时写了 335 行，装了以后 10 行" width="560">

AI 写代码前先过几道问题：这东西真的要吗？项目里是不是已经有了？浏览器、标准库能不能直接做？上图是作者给的例子，同样是「加一个日期选择器」，没装时 AI 写了 335 行，装了以后用浏览器自带的日期输入框，10 行。

另外带三个命令：`$ponytail-review` 审当前改动，`$ponytail-audit` 审整个仓库，`$ponytail-debt` 把代码里标了 `shortcut:` 的地方收成一张待还清单。

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

它带两个 hooks（会自动运行的小脚本）。装完在 Codex 里打开 `/hooks`，看过脚本再确认，然后开一个新对话。

### planning-with-files：长任务，把进度写进文件

[OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files) · 2.7 万星 · MIT

做多步骤的大任务时，让 AI 把计划、查到的东西、做到哪一步分别写进 `task_plan.md`、`findings.md`、`progress.md`。Codex 的长会话经常要压缩上下文，压缩完、断了、换个对话，它都能从这几个文件里接着做。

适合一次要跑几十步、跨好几个对话的任务。仓库里还有中文版（`planning-with-files-zh`）。

```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files --agent codex -g --yes
```

想要它自动提醒和收尾检查，还可以装它的 hooks，步骤见作者的 [Codex 说明](https://github.com/OthmanAdi/planning-with-files/blob/main/docs/codex.md)。

### archify：把代码和想法画成图

[tt-a1i/archify](https://github.com/tt-a1i/archify) · 8.5 万星 · MIT

把一段描述、一个计划或者整个代码库画成图：架构图、流程图、时序图、数据流、状态机、ER 图都行。输出一个 HTML 文件，浏览器打开，节点能点开看细节，能切深色浅色（效果见上面第一张图）。

接手陌生项目、想先看清结构时最有用，写文档配图、给不懂代码的人讲方案也用得上。

```bash
npx skills add tt-a1i/archify --skill archify --agent codex -g --yes --copy
```

要 Node.js 18 以上。中文说明见仓库里的 `README_ZH.md`。

### taste-skill：让前端别那么像 AI 做的

[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) · 9.9 万星 · MIT

给 AI 一套做网页的审美规则，字体、间距、配色、版式往成熟设计靠，少出那种一眼就是 AI 生成的渐变卡片页（效果见上面第四张图）。仓库里有十几个风格变体，默认的叫 `design-taste-frontend`。

适合落地页、个人站、产品介绍页。

```bash
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend" --agent codex -g --yes
```

要 Node 22 以上，电脑上要有 Chrome、Edge 或 Chromium；找配图要联网。

## 办公和内容

### 做 PPT：guizang-ppt 和 codex-ppt

两个都好用，路子不一样。要能改字、要上台演示，选 guizang；要每页都好看、拿去发图，选 codex-ppt。

**[op7418/guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill)** · 2.8 万星 · AGPL-3.0

做出来是一个 HTML 文件，横向翻页，带演讲者视图、讲稿备注和计时，文字都能改。在 Codex 里还会问你要不要用 GPT-Image 配图，也能顺手出公众号、小红书封面。

```bash
npx skills add https://github.com/op7418/guizang-ppt-skill --skill guizang-ppt-skill --agent codex -g --yes
```

**[ningzimu/codex-ppt-skill](https://github.com/ningzimu/codex-ppt-skill)** · 7,000 星 · MIT

每一页直接生成成一张图，再拼成 PPTX，风格统一（效果见上面第三张图）。文字不能直接改，要改字可以再用作者的 [image-to-editable-ppt-skill](https://github.com/ningzimu/image-to-editable-ppt-skill) 转回可编辑的 PPT。

```bash
npx -y skills@latest add ningzimu/codex-ppt-skill --skill codex-ppt --agent codex --global --yes
```

每页都要生成图片，会用到图片生成的额度。

### nature-skills：写论文和科研绘图

[Yuan1z0825/nature-skills](https://github.com/Yuan1z0825/nature-skills) · 4.9 万星 · Apache-2.0

中文作者做的一整套科研 skill，22 个：查文献、核引用、精读论文出阅读卡、按 Nature 风格润色、画期刊配图、论文转中文 PPT、写数据可用性声明，还能把论文改成专利草稿。

一次别全装，用到哪个装哪个。比如只要画图：

```bash
npx skills add Yuan1z0825/nature-skills --agent codex --skill nature-figure --yes --copy
```

这条装进当前项目；加 `--global` 就所有项目都能用。装 `nature-writing`、`nature-polishing`、`nature-reader`、`nature-paper2ppt` 时，要把 `nature-shared` 一起装上。

### shuorenhua：中文去 AI 味

[MrGeDiao/shuorenhua](https://github.com/MrGeDiao/shuorenhua) · 2,152 星 · MIT

改写中文，去掉模板腔，事实和数字一个不丢。作者给的例子：

> 原文：值得注意的是，本轮我们完成了对重试策略的调整，失败请求数从 24 次降到 7 次。简而言之，重试策略已经调整过了。
>
> 改写：本轮调整了重试策略，失败请求数从 24 次降到 7 次。

写 README、公众号、周报、产品说明时用得上，也可以只标出问题不改。在你的项目根目录运行：

```bash
git clone --depth 1 https://github.com/MrGeDiao/shuorenhua.git /tmp/shuorenhua
mkdir -p .agents/skills/shuorenhua/references
cp /tmp/shuorenhua/SKILL.md .agents/skills/shuorenhua/
cp /tmp/shuorenhua/references/editing-guide.md /tmp/shuorenhua/references/examples.md .agents/skills/shuorenhua/references/
```

英文用 [blader/humanizer](https://github.com/blader/humanizer)（5.8 万星，MIT），按维基百科总结的「AI 写作迹象」改英文：`npx skills add blader/humanizer --global --agent codex --yes`。

### baoyu-skills：公众号和小红书

[JimLiu/baoyu-skills](https://github.com/JimLiu/baoyu-skills) · 2.7 万星 · MIT

宝玉（JimLiu）的一组内容创作 skill：排版 Markdown、转成公众号能用的 HTML、生成封面和文章配图、小红书图卡（效果见上面最后一张图）、信息图、漫画。作者也建议别全装，做公众号、小红书常用这四个：

```bash
npx skills add jimliu/baoyu-skills --agent codex -g \
  --skill baoyu-format-markdown --skill baoyu-markdown-to-html \
  --skill baoyu-cover-image --skill baoyu-xhs-images --yes
```

出图在 Codex 里会用自带的 `imagegen`，用的是你的额度。

## OpenAI 官方目录：浏览器、PDF、GitHub

[openai/skills](https://github.com/openai/skills) · 2.8 万星 · Apache-2.0

Codex 自带 5 个：画图的 `imagegen`、查官方文档的 `openai-docs`、做插件的 `plugin-creator`、做 skill 的 `skill-creator`、装 skill 的 `skill-installer`。另外有 39 个精选的可以按需装，这几个最常用：

| skill | 能帮你做什么 |
|---|---|
| `playwright` | 在终端里操作真实浏览器：填表、点按钮、截图、调试页面 |
| `pdf` | 读 PDF、生成 PDF，按页渲染出来检查版式 |
| `gh-fix-ci` | GitHub Actions 挂了，让它读日志、修好 |
| `security-best-practices` | 按你用的语言和框架做安全检查 |

用 Figma、Linear、Notion、Sentry 的，也各有对应的一组。在 Codex 里输入：

```text
$skill-installer playwright
```

playwright 要电脑上有 Node.js；pdf 会装几个 Python 库，渲染页面要 Poppler。

## 深度解读

- [superpowers 在 Codex 里怎么用：15 个 skill 拆开看，哪些值得留](deep-dives/superpowers.md)

## 国内怎么开通 Codex

用 Codex 要有 ChatGPT 的套餐。Free 和 Go 只能在桌面版用少量的 GPT-5.6 Terra、GPT-6 Luna；GPT-6.1 Sol、Astra 和 CLI 要 Plus 起步。没有海外卡，可以微信付款，由 AONIR 充值到你自己的账号：

| 套餐 | 微信付款 | 官方价 | Codex 额度 |
|---|---|---|---|
| [ChatGPT Plus](https://aonir.com/chatgpt-plus/?utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=cta_plus) | ¥168 / 月 | $20 | 基准，每 5 小时和每周各有上限 |
| [ChatGPT Pro 5×](https://aonir.com/chatgpt-pro/?plan=5x&utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=cta_pro_5x) | ¥730 / 月 | Pro 100，$100 | Plus 的 5 倍，没有 5 小时限制 |
| [ChatGPT Pro 10×](https://aonir.com/chatgpt-pro/?plan=20x&utm_source=github&utm_medium=referral&utm_campaign=codex_skills&utm_content=cta_pro_10x) | ¥1350 / 月，客服开通（账号需是免费版） | Pro 200，$200 | Plus 的 10 倍，没有 5 小时限制 |

Codex 装不上、登录不了、报错，看我们另一个仓库：[Codex 国内使用与报错速查](https://github.com/Avarce/codex-china-guide)。

## 推荐一个 skill

觉得哪个 skill 该收、哪条写得不对，[开一个 Issue](../../issues/new)：贴上仓库链接、你用它做什么、在 Codex 里装没装上。我们看过源码、装一遍之后补进来，记在[更新日志](CHANGELOG.md)里。

## 转载与许可

本页文字采用 [CC BY 4.0](LICENSE) 许可：可以转载、翻译、改编，注明出处并附上本仓库链接即可。文中的 skill 和图片都归原作者所有，以各自仓库的许可证为准。Codex、ChatGPT、OpenAI 是 OpenAI 的商标，本仓库与 OpenAI 没有隶属关系。
