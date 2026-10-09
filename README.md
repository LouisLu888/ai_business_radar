# AI Business Radar

每天追踪指定 AI 创业者与经营者的一手内容，提供原始链接、中文摘要和经营主题分类，供手机阅读。

**状态：方案评审中。当前仓库只有设计文档与配置示例，尚未实现、采集或部署。**

- [产品需求与手机页面](docs/prd.md)：先看这个，确认第一版解决的问题。
- [实施计划与评审项](docs/plan.md)：开发顺序、交付标准与成本预算。
- [前后端技术设计](docs/rfc.md)：Cloudflare 架构、数据模型、采集和部署。
- [验证计划](docs/test.md) · [工作记录](docs/working.md)
- [名单填写示例](config/people.example.yaml)

## 隐私

This repository is designed to be publishable with only fake examples.

代码和方案公开。部署实例默认供单人登录使用；真实追踪名单、阅读记录、采集正文、日志、凭证和资源标识保存在私有配置或云端存储中，不提交到 Git。示例人物、结果和域名都是虚构占位。

## 开发状态

产品负责人 review 通过后，按实施计划先验证真实来源，再开始应用开发。生产部署目标为 Cloudflare；字幕与音频转写允许通过外部数据 API 获取，具体依赖见技术设计。
