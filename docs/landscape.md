# 已有工具覆盖采集与摘要，自建价值需要用真实阅读任务验证

调研日期：2026-10-08 · 为本项目选型与设计服务

**有相近产品，而且“按人物追踪 AI 一手内容”已有开源实践。** 本轮比较六个网站与六个 GitHub 项目：最接近人物追踪思路的是 Follow Builders，最接近现成阅读方案的是 Inoreader，最接近 Cloudflare 部署架构的是 Briefings。建议保留 Cloudflare 项目方向，同时用现成工具作对照，验证自选经营者名单、跨节目访谈识别和经营证据留存能否减少人工整理。

本次核查官方功能文档、公开页面、GitHub README/许可证和部分源码。未注册付费产品、运行第三方项目或测试真实名单。下文的功能是文档或源码所示能力；准确率、覆盖率和维护成本仍需实测。“本次没有确认”不等于产品一定没有此功能。

阅读路径：先看网站对比和开源项目，再看选型结论。具体变化记录在 [两轮评审记录](review_iterations.md)，产品最新约定见 [PRD](prd.md)。

## 1. 现成网站已能做多源订阅和音视频摘要

| 产品 | 与本项目相近的部分 | 借鉴及边界 |
| --- | --- | --- |
| **Inoreader** | RSS、newsletter、YouTube、播客；监测 feed、规则自动摘要和音视频转写 | 第一对照对象。学习先筛选再处理、区分简介/逐字稿/主题；人物身份与经营证据是否能仅靠配置完成，需实测 |
| **Readwise Reader** | 多格式阅读、RSS/newsletter、YouTube、播客逐字稿、可定制摘要与标签 | 学习收件内容与主动收藏分离；feed 自动摘要和音频正文生成的触发方式要逐项检查 |
| **Podwise** | 播客/YouTube 转写、摘要、时间位置、多语言与导出，提供 API/CLI/MCP | 学习从要点跳回原始段落；可以评估为数据提供方，不能由有 API 推断允许无限导出或任意再分发 |
| **Podscan** | 跨播客正文搜索、关键词监测、提醒和 API | 说明跨节目发现是独立的数据服务；现阶段费用超过预算，首版保留已登记节目与有限搜索 |
| **Starter Story** | 创业者访谈、经营案例、收入与增长方式等条目 | 学习经营案例字段和访谈入口；其数据库数字是来源披露，不能当成独立审计结果；可作为未来候选来源 |
| **Recall** | 收藏 YouTube、播客和文章，做摘要、关联知识与导出 | 学习保留原链接和便于迁移的输出；知识图谱与复习功能暂不进入本项目 |

**Inoreader 的功能重叠已相当大。** 官方[自动规则说明](https://www.inoreader.com/blog/2025/10/introducing-new-rule-triggers-and-actions-translations-summaries-and-more.html)列出自动创建摘要；[Intelligence 文档](https://www.inoreader.com/blog/2026/01/get-insights-with-inoreader-intelligence.html)说明音视频的简介、逐字稿、带时间主题及文章视图。[定价页](https://www.inoreader.com/pricing)列出 Pro 月付 $9.99、年付折合 $7.50/月。实际 AI、转写及报告的额度另行核验，不能把订阅价当成无限处理成本。

**Readwise 已支持播客，不能按旧资料把它排除。** [2025 年 12 月更新](https://blog.readwise.io/update-dec2025/)说明节目 RSS 可逐集触发转写；[Ghostreader 文档](https://docs.readwise.io/reader/docs/faqs/ghostreader)说明自动摘要可自定义，而 feed 自动摘要需要自己的 OpenAI key。[Feed 文档](https://docs.readwise.io/reader/docs/faqs/feed)还明确了部分正文、OPML 和刷新行为。自定义中文经营摘要本身不是本项目独有能力。

**Podwise 与 Podscan 解决不同问题。** [Podwise](https://podwise.ai/)偏单集内容理解与知识管理；其网站列出 CLI/OpenAPI 等入口。[Podscan](https://podscan.fm/)偏跨节目监测，当前[定价](https://podscan.fm/pricing)入门档列为 $100/月。本轮只学习后者的发现机制，不将其费用悄悄加入原有 $30/月方案。

**案例库与阅读器可以互补。** [Starter Story](https://www.starterstory.com/)提供创始人访谈和按经营指标组织的案例；[Recall](https://www.recall.it/)强调跨格式保存及 Markdown 导出。前者适合启发收录字段，后者适合启发保存体验；都不能仅由公开首页判断已完整满足指定人物每日追踪。

## 2. 开源项目优先判断能复用什么

| 项目 | 已查看的能力与运行依赖 | 复用决定 |
| --- | --- | --- |
| [Follow Builders](https://github.com/zarazhangrui/follow-builders) | 按人物整理 X、播客和博客；集中更新 feed，由 agent 生成中英日报/周报 | 最接近人物优先的选源思路；README 标 MIT，本轮未见独立 LICENSE，复制代码前确认完整许可；当前只借鉴选源与语言配置 |
| [Briefings](https://github.com/hirefrank/briefings) | RSS 日摘要/周报；Workers + Hono + D1/R2，另用 KV 和四条 Queues | 最接近技术参考；[MIT](https://github.com/hirefrank/briefings/blob/main/LICENSE)，择取小模块，不整库 fork |
| [AI News Brief](https://github.com/frankzch/ai-news-brief) | 多源采集、YouTube 字幕、中英摘要与两层去重；Python + PostgreSQL/pgvector + 浏览器 | [MIT](https://github.com/frankzch/ai-news-brief/blob/main/LICENSE)，学习处理分层；不移植整套运行环境或热度阈值 |
| [TrendRadar](https://github.com/sansan0/TrendRadar) | 热榜/RSS、关键词分组、AI 分析、日报 HTML；Python/Docker | [GPL-3.0](https://github.com/sansan0/TrendRadar/blob/master/LICENSE)；本轮只借鉴增量展示与筛选体验，不复制代码 |
| [news-summary](https://github.com/guo-yichen/news-summary) | RSS + Supadata YouTube 字幕 + 部分预建播客 feed，中文日报到 Notion/邮件；Python + GitHub Actions | 调研时未找到明确许可证，代码不纳入复用；参考定时采集顺序，不能视作任意播客转写方案 |
| [ai-podcast-ripper](https://github.com/mbjornson/ai-podcast-ripper) | RSS 拉新集、转写、长稿分块、重试；macOS + Python + FFmpeg + 本地模型服务 | 调研时未找到明确许可证，代码不纳入复用；本机依赖与托管目标不同 |

**Follow Builders 让自建理由更需要具体。** [README](https://github.com/zarazhangrui/follow-builders/blob/main/README.md)描述了集中维护的 AI 研究者、创始人、产品经理和工程师名单，以及 26 个 X 账号、6 个播客、2 个博客；feed 每日更新，可生成英文、中文或双语简报。[客户端准备脚本](https://github.com/zarazhangrui/follow-builders/blob/main/scripts/prepare-digest.js#L29)指向项目维护的三个 feed。其“不需要内容 API key”不等于 agent 推理与整个流程免费。本轮只查看文件和 feed 结构，没有运行它或验证日更质量。

它适合借鉴人物优先和集中来源配置，但默认名单由项目维护，与用户自主维护经营者名单不同；AI 从业身份也不能代替商业成果证据。它是 news-summary 的预建播客 feed 来源，这说明下游所谓播客支持可能依赖一个有限目录。首版不直接导入其人物名单，也不新增这一公共 feed 依赖；未来评估时需单独验证覆盖和上游更新状态。

**Briefings 最有借鉴价值，也最需要检查处理边界。** 本轮读取了 [提交 3f023e3](https://github.com/hirefrank/briefings/commit/3f023e3a33dbf276c46a49fef764bbc37dbfa3cd) 的源码：其 [feed 提取](https://github.com/hirefrank/briefings/blob/3f023e3a33dbf276c46a49fef764bbc37dbfa3cd/src/services/feed/feed-service.ts#L401)清洗 RSS 内已有正文；[摘要路径](https://github.com/hirefrank/briefings/blob/3f023e3a33dbf276c46a49fef764bbc37dbfa3cd/src/services/summarization/summarization-service.ts#L60)限制每批十篇，[结构化 prompt](https://github.com/hirefrank/briefings/blob/3f023e3a33dbf276c46a49fef764bbc37dbfa3cd/src/services/summarization/summarization-service.ts#L769)将每篇截到约 2,000 字符。它适合参考部署与来源状态，不适合直接承担完整长访谈的证据抽取。README 的防幻觉表述属于项目声明，本轮未验证效果。

**成熟度不能从 README 长度或星数推出。** GitHub API 核查时六个项目均未归档；最后 push 分别为 Follow Builders 2026-10-08、Briefings 2026-02-02、AI News Brief 2026-09-19、TrendRadar 2026-09-13、news-summary 2026-10-08、ai-podcast-ripper 2026-09-28。这些日期只说明仓库活动，不能证明生产可靠。后续选定复用模块时，固定 commit、核对许可证、针对真实来源运行测试，并保留归属说明。

news-summary 和 ai-podcast-ripper 的许可证接口当时返回 404，结合根目录和 README 检查后仍未确认授权：[前者仓库 API](https://api.github.com/repos/guo-yichen/news-summary) · [后者仓库 API](https://api.github.com/repos/mbjornson/ai-podcast-ripper)。这两者作为设计参考，不当作已获许可的代码库。本轮没有引入任何第三方源码或依赖。

## 3. 先证明省下人工整理，再扩大功能

“生成中文摘要”“按收入字段组织信息”都可能通过现成产品的自定义 prompt 完成。自建理由必须更具体：同一个人出现在不同节目时能否正确关联；数字与出处是否长期一致；采集缺失能否清楚看见；每天是否减少查找、核对和修正的时间。

建议第一阶段先做限时两小时的 Inoreader 配置初筛，只判断可行性。质量比较等双方均有实际输出后进行：同一组冻结的 20 条真实样本、相同 URL 与摘要要求、相同人工输入，补充及修正时间单列。不能拿人工精修的理想卡片与工具原始输出对比；账号、额度或时间不足记未测，不记失败。

发现测试另用相同人物、别名、节目来源和固定时间窗，隐藏事先保存的访谈核对集，报告“已知核对集命中率”，不推断全网覆盖。七天试运行再记录每天整理时间。现成工具满足需求时，向产品负责人呈现轻量配置替代；当前 Cloudflare 约束不自动改变。

借鉴成熟工具的固定分类、来源入口、按需转写和失败提示。第一版暂缓知识图谱、聊天、思维导图、全文搜索、全网播客索引和实时推送，让每项新增功能都能对应一次真实阅读决策。

两轮修改已落实到 v0.3：第一轮收窄差异主张并补来源能力；第二轮修正样本分母、公平对照、完整度的可观察边界和预算排队。真实名单上的覆盖、摘要准确性、供应商完整度信号、维护时间和账单仍未实测，文档评审不能代替这些验证。
