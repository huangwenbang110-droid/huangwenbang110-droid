# 云原生可观测 · 从自研监控平台到 AI 可观测

2019 年开始做可观测平台，到现在换了四代。做过采集 SDK、Agent 架构、OpenTelemetry 接入，现在在做 AIOps 与 AI 可观测。

**技术栈**：OpenTelemetry / Java / Python / React / Spring AI / LangChain / Kubernetes / ClickHouse / Kafka

**→ 个人站点：https://huangwenbang110-droid.github.io/**

---

## 演进线

| 阶段 | 做了什么 | 关键转折 |
|---|---|---|
| Octopus · 第一代监控平台 | 自研采集 SDK 与平台应用层，覆盖日志与指标。底层用开源组件，上层自研 | 调用链当时不在视野里——不是评估后放弃，是根本没进需求 |
| APM · 应用性能监控 | 引入调用链，第一次认真做技术选型评估 | 业务方问"这个请求慢在哪一步"，日志和指标答不上来 |
| 云原生应用可观测 | 指标 / 日志 / 调用链三信号统一，Kubernetes 环境适配 | 规模上量之后的架构约束 |
| OpenTelemetry 接入 | **不切换，双协议并行**：自研路径式编号与 OTel 标准父子结构同时支持、双发协议，存量探针不受影响 | 同时维护两条语义映射的代价 |
| AIOps | 用大模型处理运维数据：告警降噪与收敛、根因分析、自动查询并生成诊断报告；40+ 只读 MCP 工具 | 大模型在工程上的边界：幻觉、成本、时延 |
| AI 可观测 | 观测 LLM 应用本身：轨迹追踪、性能指标、成本分析、质量评估 | 传统 APM 的底座不够用：Token 语义、内容本身、工具调用、非确定性输出 |

---

## 文章系列

**→ 文章仓库：https://github.com/huangwenbang110-droid/articles**

| # | 主题 | 状态 |
|---|---|---|
| 1 | 平台、日志、指标 | [已发布](https://github.com/huangwenbang110-droid/articles/blob/main/01-第一代监控平台.md) |
| 2 | 从 SDK 到 Agent | 写作中 |
| 3 | Agent 架构落地 | 待写 |
| 4 | 拥抱开源协议，但不切换：接入 OpenTelemetry 的代价清单 | 写作中 |
| 5 | 双协议并行 | 待写 |
| 6 | 日均数十 TB：规模带来的工程问题 | 待写 |
| 7 | AIOps 探索 | 待写 |
| 8 | AI 可观测落地 | 待写 |

每篇写两层：这一代**怎么实现的**（方案、选型、数据模型），以及后来**为什么要换掉**。

---

## 开源项目

**ai-observability-guard** — 开发中，即将开源

零侵入的 Java Agent，在 OpenTelemetry Span 层为 Java AI 框架补上成本治理与内容脱敏；配一个 Spring Boot Starter 补上调用前的预算熔断与循环检测。

- 治理层挂 OTel 的 SpanProcessor 与 SpanExporter 装饰器，不增强任何框架类
- 一个实现覆盖 Spring AI 与 LangChain4j，对框架版本免疫
- 自定义属性一律走 `ai.guard.*` 命名空间，不占用还在演进的 `gen_ai.*`
