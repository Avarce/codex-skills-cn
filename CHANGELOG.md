# 更新日志

收录、改动、撤下 skill，或者 Codex 改了 skill 的规则，这里记一笔。日期为北京时间。

## 2026-10-30

- 「国内怎么开通 Codex」的套餐表：美国时间 10 月 29 日起 Pro 三档的 Work / Codex 额度翻倍，倍数改为约为 Plus 的 10、20、50 倍，表下注明出处（OpenAI 帮助中心、Codex 负责人 Tibo）。收录的 skill 没有变化。
- Pro 200 的 AONIR 链接换成新地址 `/chatgpt-pro-200/`（旧地址自动跳转）。
- 更正：「国内怎么开通 Codex」原来写「GPT-6.1 Sol、Astra 和 CLI 要 Plus 起步」，改成官方的说法：Free 和 Go 在桌面版里用 GPT-5.6 Terra、GPT-6 Luna，GPT-6.1 Sol、GPT-6.1 Terra、Astra 和网页版的云端任务要 Plus 或 Pro；表下写明倍数是 Tibo 的说法（帮助中心只写了翻倍）。页首日期改为 10 月 30 日，收录的 skill 仍是 10 月 19 日核对的。planning-with-files 的 Codex 说明链接更新（作者把默认分支改成了 master）。

## 2026-10-19（第三版）

- 新收 4 个：mattpocock/skills、agent-browser、hyperframes、frontend-design（和 taste-skill 放在一起）。都按页面上的命令在 Codex 0.163.0 里装过，能认到。
- 每个推荐配一张统一的卡片：左边是做什么、怎么说，右边是效果图，没有效果图的画成步骤。去掉了开头的效果预览，图不再出现两次。
- 星数改成自动更新的徽章（GitHub 星数、最后一次提交、skills.sh 安装量），正文里不再写死数字。
- 「按需求挑」的第三列从示例说法改成「装上以后，它会怎么做」，示例说法留在每条的卡片上。
- 加了「本期值得看」和「这页怎么更新」：每两周核一次，新收的写在本期，装不上或停更的撤下。
- taste-skill 作者 10 月 19 日发了 2.1.1 版，`npx skills add` 会因为说明的格式报错跳过它，装法改成 Codex 插件。
- 安装说明补了新收的 4 组各占多少字符；全装 96 个时，Codex 把 48 条较长的说明截短到 240 到 550 字符。
- 旧条目里能在命令行装的，安装命令都重跑了一遍。

## 2026-10-18（第二版）

- 首页改成「效果预览 → 按需求挑 → 推荐详情」：开头放 6 张各 skill 自己仓库里的效果图，表格里每个推荐配一句「你可以这样说」。
- 文件夹、确认装上、装多了的影响、hooks、停用卸载，挪到单独的[安装说明](INSTALL.md)，附每个 skill 占多少字符的表。
- 页首只留一句维护说明，开通方式放到最后一节。

## 2026-10-18

第一版。

- 「先看这 5 件事」：Codex 认哪几个文件夹（在 CLI 0.162.1、0.163.0 上实测，认 `~/.agents/skills`、`~/.codex/skills` 和项目里的同名目录，不认 `.claude/skills`）；不发请求就能确认装没装上的命令；装多了的代价（自带 5 个约 2,000 字符，本页全装约 21,400 字符）；skill、插件、hooks 的区别；用别人的 skill 前看哪三样。
- 推荐 10 组：OpenAI 官方目录、superpowers、ponytail、planning-with-files、archify、taste-skill、guizang-ppt 和 codex-ppt、nature-skills、shuorenhua（加英文的 humanizer）、baoyu-skills 里挑出的 4 个。每组写明怎么装、实测认没认到、占多少字符、要注意什么。
- 深度解读：superpowers 的 15 个 skill 逐个拆开，加上在 Codex 里要开的 `multi_agent` 开关和停用办法。
