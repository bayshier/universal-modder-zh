# 贡献指南

人类和 AI agent 的贡献都欢迎。其中大多数会是给知识库（`knowledge/`）写的**实战笔记**，由 agent 在
一次改造会话结束时完成。这正是本仓库的意义：一个 agent 弄明白的每款游戏，都变成下一个 agent 能读
的东西。

## 如果你是 AI agent
你刚刚改造了（或试图改造）一款游戏，并且学到了东西。把它们分享出来：

1. **先问你的主人。** pull request 是公开的，挂着 Ta 的 GitHub 身份。告诉 Ta 你想发布什么，把笔记
   给 Ta 看。
2. **确认内容是新的：** `um kb search "<game>"`。如果已有笔记，就去改进它：补上你的版本和踩过的
   坑，修正写错的地方。不要另写一篇。
3. **搭骨架：** `um kb new --game "<game>" --title "<what you built>" --from-scan "<game>" --agent "<agent (model)>"`。
   它会落到 `knowledge/games/<game>/`。跨游戏的方法论放 `techniques/`（`--kind technique`）。
4. **根据你的日志（`MODLOG.md`）填充内容：**
   - 精确版本；
   - 路线以及为什么走它；
   - 引擎的真实行为；
   - 构建步骤；
   - 你验证了什么、没验证什么；
   - 编号的**坑**（症状 → 原因 → 解法）。

   读者是一个从没见过这款游戏的 agent，为 Ta 而写。
5. **校验：** `um kb check <note>`，然后 `um kb index`。
6. **开 PR：** `um kb pr <note>`（dry run 会先展示命令），然后 `um kb pr <note> --yes`。它会建分支、
   提交笔记（+ `media/` + 索引）、必要时 fork、推送，并用 `gh` 开 PR。
   没有 `gh` 的话，手动推分支再在 github.com 上开 PR。

## 收录范围
任何游戏都欢迎：单机或联机、新游戏或你自己架服务器上的老客户端，任何类型任何题材。引擎内部结构、
内存偏移与特征码，对它们都可以写，只要守住下面的硬性规则。

## 硬性规则（违反的 PR 会被关闭）
- **不带游戏内容：** 不收游戏文件、提取出来的素材、ROM 或 ISO，也不收盗版拷贝的链接。
- **不收反编译代码转储。** 用自己的话描述逻辑并给符号命名；*你自己*写的代码片段保持简短
  （`um kb check` 对超过 150 行的代码块直接报错，超过 60 行警告）。
- **泄漏内容：** beta 版本和泄漏 SDK/源码得来的知识，可以用自己的话写，也可以说明出处。但泄漏的
  材料本身不能进仓库：不贴代码、不带附件、不给下载链接或许可证密钥，也不给获取指引。
- **不害其他玩家，不做绕过：**
  - 不做任何在你不掌控的服务器上给其他玩家不公平优势的东西（自瞄、透视、加速、机器人）；
  - 不做反作弊、DRM 或所有权校验的绕过；
  - 不写给带反作弊保护的在线客户端做注入的教程；
  - 唯一例外：对已经无处可买的游戏，笔记可以提到「存在面向没有拷贝的人的社区路线」并链接该社区
    的指南页面。其步骤、密钥、序列号和文件不得进仓库，指向补丁版可执行文件、破解或游戏下载的
    直链同样不行。
- **不带密钥：** API key、token、`.env` 文件。`um kb check` 和 `um publish check` 能拦住常见品种。
- **诚实：**
  - 写明 agent 与模型（`agents:`）；
  - 如实标注 `status`（`idea`、`in-progress`、`working`、`released`、`abandoned`）；
  - 列出你没验证的部分。
- **署名**你建立在谁的成果之上：相关的人与项目。

## 其他贡献
- **工具（`um/`）：**
  - 每个 CLI 组一个模块，docstring 兼作 `--help`；
  - 在 `tests/` 里加测试；
  - `uv run --with pytest pytest -q tests` 必须通过。
- **技能（`skills/`）：** Agent Skills 格式（`SKILL.md` 带 `name` + `description`）。保持 agent 中立：
  说「the agent」，不说具体某家产品。深度材料放 `references/`。
  - 只改 `skills/`。`.agents/skills` 和 `.claude/skills` 是拷贝（不用符号链接，Windows clone 才能用）。
    用 `rm -rf .agents/skills .claude/skills && cp -r skills .agents/skills && cp -r
    skills .claude/skills` 刷新；两处不一致时测试会失败。
- **引擎作战手册**（`skills/mod-any-game/references/engines/`）：路线、工具、坑。链接到权威项目；
  版本会变，所以要写「以当前发布版为准」。
- **案例（`examples/`）：** 只放你自己的代码和素材。第三方 SDK 用 `fetch` 脚本拉取，并通过
  `um publish check --game <install>`。

CI 会在每个 PR 上跑测试、`um kb check --index` 和 CLI 帮助界面。
