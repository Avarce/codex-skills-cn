# 更新日志

收录、改动、撤下 skill，或者 Codex 改了 skill 的规则，这里记一笔。日期为北京时间。

## 2026-10-18

第一版。

- 「先看这 5 件事」：Codex 认哪几个文件夹（在 CLI 0.162.1、0.163.0 上实测，认 `~/.agents/skills`、`~/.codex/skills` 和项目里的同名目录，不认 `.claude/skills`）；不发请求就能确认装没装上的命令；装多了的代价（自带 5 个约 2,000 字符，本页全装约 21,400 字符）；skill、插件、hooks 的区别；用别人的 skill 前看哪三样。
- 推荐 10 组：OpenAI 官方目录、superpowers、ponytail、planning-with-files、archify、taste-skill、guizang-ppt 和 codex-ppt、nature-skills、shuorenhua（加英文的 humanizer）、baoyu-skills 里挑出的 4 个。每组写明怎么装、实测认没认到、占多少字符、要注意什么。
- 暂不推荐：Anthropic 的 docx、pptx、xlsx、pdf（许可证不允许拿到 Anthropic 服务以外用），逆向和渗透类，走逆向接口的，账号池和中转站，一行 `curl | bash` 装的。
- 深度解读：superpowers 的 15 个 skill 逐个拆开，加上在 Codex 里要开的 `multi_agent` 开关和停用办法。
