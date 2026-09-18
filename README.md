# 云原生可观测 · 从自研 SDK 到 AI 可观测

七年间完整走过可观测体系的四代演进，在数百应用、数千业务单元、数万节点的规模上做过完整落地。

技术栈：Java、Python。

**→ 个人站点：https://huangwenbang110-droid.github.io/**

---

## 演进线

| 阶段 | 做了什么 | 关键转折 |
|---|---|---|
| Octopus · 第一代监控平台 | 自研采集 SDK 与平台应用层，覆盖日志与指标。底层用开源组件，上层自研 | 调用链当时不在视野里——不是评估后放弃，是根本没进需求 |
| APM · 应用性能监控 | 引入调用链，第一次认真做技术选型评估 | 业务方问"这个请求慢在哪一步"，日志和指标答不上来 |
| 云原生应用可观测 | 指标 / 日志 / 调用链三信号统一，Kubernetes 环境适配 | 规模上量之后的架构约束 |
| OpenTelemetry 统一 | 协议与语义迁移，存量兼容 | 迁移的真实代价，几乎没人公开写 |
| AIOps | 大模型做告警根因分析，40+ 只读 MCP 工具 | 大模型在工程上的边界：幻觉、成本、时延 |
| AI 可观测 | AI 应用的 Token 成本治理与内容脱敏 | 为什么治理层必须在 Span 层，而不是采集层 |

---

## 文章系列

**→ 文章仓库：https://github.com/huangwenbang110-droid/articles**

| # | 主题 | 状态 |
|---|---|---|
| 1 | 日志和指标都有了，就是看不见慢在哪 | [已发布](https://github.com/huangwenbang110-droid/articles/blob/main/01-第一代监控平台.md) |
| 2 | 从 SDK 到 Agent：侵入式埋点的失效点 | 写作中 |
| 3 | Agent 架构落地：Java 与 Python 两条栈的分歧 | 待写 |
| 4 | 迁移到 OpenTelemetry：一份代价清单 | 写作中 |
| 5 | 日均数十 TB：规模带来的工程问题 | 待写 |
| 6 | AIOps 落地：大模型做告警根因分析的工程边界 | 待写 |
| 7 | 40+ 只读 MCP 工具：为什么全部设计成只读 | 待写 |
| 8 | AI 可观测：成本与内容治理为什么必须在 Span 层 | 待写 |

写的是每一代**为什么要重做**，以及迁移的代价。

---

## 开源项目

**ai-observability-guard** — 开发中，即将开源

零侵入的 Java Agent，在 OpenTelemetry Span 层为 Java AI 框架补上成本治理与内容脱敏；配一个 Spring Boot Starter 补上调用前的预算熔断与循环检测。

- 治理层挂 OTel 的 SpanProcessor 与 SpanExporter 装饰器，不增强任何框架类
- 一个实现覆盖 Spring AI 与 LangChain4j，对框架版本免疫
- 自定义属性一律走 `ai.guard.*` 命名空间，不占用还在演进的 `gen_ai.*`
