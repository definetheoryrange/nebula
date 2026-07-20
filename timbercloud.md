# NeoLink Navigator

NeoLink Navigator is a curated, high-availability technical resource aggregation and navigation system designed for developers, data analysts, and operations engineers who require rapid access to specialized information streams. The project addresses the critical challenge of fragmented, unreliable, or constantly shifting external data sources by providing a structured, version-controlled, and community-validated repository of actionable URLs and their associated metadata. Unlike a simple bookmark manager, NeoLink Navigator treats each link as a first-class entity with lifecycle tracking, availability monitoring, and contextual documentation, enabling teams to maintain operational continuity even when upstream providers change their endpoints or access patterns. The primary target users are SRE teams managing external integrations, data pipeline engineers ingesting third-party APIs, and technical researchers who depend on consistent access to niche information portals.

## 功能概览

- **Structured Link Registry** – Maintains a centralized, YAML-based catalog of all managed URLs with fields for category, expected content type, update frequency, and fallback priorities.
- **Automated Availability Probes** – Periodically tests each endpoint using configurable health checks (HTTP status, response time, and content signature) and logs historical uptime data.
- **Tag-Based Filtering and Search** – Supports hierarchical tags (e.g., `region/asia`, `sport/football`, `type/live`) for rapid filtering via CLI or web UI, with full-text search across descriptions and annotations.
- **Versioned Snapshot History** – Records every change to the link database in a Git-compatible format, allowing rollback to any previous state and diff visualization between revisions.
- **Metadata Enrichment Pipeline** – Automatically extracts and caches Open Graph metadata, SSL certificate validity, and DNS resolution records for each URL upon addition or update.
- **Watchlist and Notification** – Enables users to subscribe to specific links or categories; sends email or webhook alerts when a monitored endpoint changes its status, content pattern, or TLS configuration.
- **RESTful API and Webhook Receiver** – Exposes a read-write JSON API for programmatic management and a webhook endpoint that accepts external change events to update the registry in near real-time.

## 应用场景

- **Live Data Feed Integration for Sports Analytics Platforms** – Data engineering teams can use NeoLink Navigator to manage the lifecycle of live score and prediction endpoints, ensuring that their ETL pipelines always reference the correct, working base URLs even when upstream providers rotate domain names. The automated probe system pre-validates each endpoint before deployment, reducing integration failures during peak traffic hours.

- **Regional Content Aggregation for News and Media Monitors** – Researchers tracking regional events across Asia can organize links by country or topic, with the watchlist feature notifying them immediately if a news portal changes its RSS feed location or returns unexpected error codes. The snapshot history allows auditing of when and why a particular source was modified or deprecated.

- **Operational Runbook Automation for SRE Teams** – Site reliability engineers can embed the NeoLink Navigator API into their automated runbooks, dynamically fetching the current primary and secondary endpoints for external dependencies during incident response. The metadata enrichment helps quickly identify SSL expiration risks or DNS propagation delays before they cause user-facing outages.

- **Academic Dataset Versioning for Longitudinal Studies** – Researchers conducting long-term studies on online information availability can use the versioned snapshot feature to maintain a reproducible record of which URLs were active on specific dates, along with their content signatures, providing a verifiable audit trail for published results.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/neolink-io/navigator.git
cd navigator

# Install production dependencies and development tools
make install
# This executes: pip install -r requirements.txt && npm ci --only=production

# Initialize the local link database and configuration
./bin/navigator init --seed-data

# Run the service in development mode (API on port 8080, UI on port 3000)
./bin/navigator serve --dev --api-port 8080 --ui-port 3000

# In a separate terminal, run the availability probe once to validate all stored links
./bin/navigator probe --full-scan
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 – 3.12 | 核心 API 服务与后台任务调度器，使用 asyncio 和 aiohttp |
| Node.js | 18.x LTS 或 20.x LTS | 前端 UI 构建（React 18）及 Webhook 测试工具 |
| Redis | 7.0+ | 缓存层，存储探测结果、会话数据和临时锁；支持 Sentinel 模式 |
| PostgreSQL | 14+ 或 15+ | 主数据库，存储链接元数据、历史快照和用户配置；需启用 JSONB 和全文搜索扩展 |
| Docker | 24.0+ (可选) | 用于本地开发环境编排，生产部署建议使用 Kubernetes |
| Make | 4.3+ | 构建自动化脚本，用于依赖安装、测试和容器镜像构建 |
| Git | 2.30+ | 版本控制，用于管理快照历史及与远程仓库同步 |
| OpenSSL | 3.0+ | 用于证书验证子系统的本地库依赖 |
| curl | 8.0+ | 健康探测引擎的备选后端，当 Python 实现超时时自动降级 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | `/docs/user-guide/` | 如何添加新链接、如何配置探测策略、如何使用看板查看状态、如何导入/导出链接列表 |
| 运维手册 | `/docs/operations/` | 如何部署高可用集群、如何备份和恢复数据库、如何配置外部监控集成、如何调整探测并发度 |
| 开发参考 | `/docs/developer/` | API 端点详细规范、Webhook 载荷格式、数据库迁移流程、贡献代码的测试覆盖率要求 |
| 设计决策 | `/docs/design/` | 为何选择 PostgreSQL 而非 MongoDB、快照压缩策略、探测超时与重试算法、TLS 指纹对比机制 |
| 故障排查 | `/docs/troubleshooting/` | 常见探测错误码释义、数据库锁冲突解决方案、Redis 连接池调整建议、UI 加载缓慢的排查步骤 |

## 资源列表

### 实时比分与赛事数据
- <code>500bifenzhibo.asia</code>
- <code>500shishibifen.asia</code>

### 赛事预测与分析
- <code>500saiguo.asia</code>
- <code>500yuce.asia</code>
- <code>500zuqiuyuce.asia</code>

### 足球推荐与比分聚合
- <code>500zuqiutuijian.asia</code>
- <code>500zuqiubifenwang.asia</code>

### 综合推荐信息
- <code>500jinrituijian.asia</code>

## 项目结构

```
navigator/
├── api/                                # RESTful API 服务实现
│   ├── v1/                             # API 版本 1 路由与处理器
│   │   ├── endpoints/                  # 按资源划分的端点模块 (links, probes, users)
│   │   ├── middleware/                 # 认证、限流、日志中间件
│   │   └── validators/                 # 输入载荷校验 (JSON Schema)
│   └── webhooks/                       # 外部变更事件接收器
│       ├── handlers/                   # GitHub、自定义 JSON 格式解析器
│       └── dispatcher.py               # 事件分发至内部任务队列
├── core/                               # 核心领域逻辑 (与框架无关)
│   ├── models/                         # 数据实体类 (Link, ProbeResult, Snapshot)
│   ├── services/                       # 业务服务 (Registry, ProbeScheduler, Enricher)
│   ├── probes/                         # 探测引擎实现 (HTTP, DNS, SSL)
│   └── storage/                        # 存储抽象接口与 PostgreSQL/Redis 适配器
├── ui/                                 # React 前端单页应用
│   ├── src/
│   │   ├── components/                 # 可复用 UI 组件 (LinkTable, StatusBadge, FilterPanel)
│   │   ├── pages/                      # 路由页面 (Dashboard, Explorer, Watchlist)
│   │   └── hooks/                      # 自定义 React Hooks (useAPI, useWebSocket)
│   └── public/                         # 静态资源与 PWA manifest
├── scripts/                            # 运维与开发辅助脚本
│   ├── migration/                      # 数据库迁移 SQL 与 Python 迁移工具
│   ├── seed/                           # 初始示例数据生成器
│   └── benchmark/                      # 探测并发与 API 负载测试脚本
├── tests/                              # 自动化测试套件
│   ├── unit/                           # 单元测试 (pytest + Jest)
│   ├── integration/                    # 集成测试 (含真实 Redis/PostgreSQL 容器)
│   └── e2e/                            # 端到端测试 (Playwright)
├── config/                             # 环境特定配置
│   ├── development.yaml                # 开发环境覆盖参数
│   ├── production.yaml                 # 生产环境参数 (敏感值从环境变量注入)
│   └── schema.yaml                     # 配置文件 JSON Schema 定义
├── deploy/                             # 部署编排文件
│   ├── docker/                         # Dockerfile 与多阶段构建
│   └── kubernetes/                     # K8s manifests (deployment, service, ingress)
├── docs/                               # 完整文档 (见上方导航章节)
├── Makefile                            # 统一构建入口
├── pyproject.toml                      # Python 依赖与工具链配置
└── README.md                           # 本文件
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从 `main` 分支签出新的分支，命名遵循 `feature/` 或 `fix/` 前缀，并在分支描述中关联对应的 GitHub Issue 编号。确保本地环境已安装所有开发依赖（可通过 `make dev-setup` 一键安装）。
2.  **实现变更并编写/更新测试** – 所有新功能或缺陷修复必须包含对应的单元测试或集成测试，测试覆盖率不得低于 85%。对于涉及链接解析或探测逻辑的变更，需额外提供模拟上游服务的测试用例。
3.  **更新文档字符串与外部文档** – 在代码中添加清晰的 docstring 描述变更行为；若涉及用户可见的功能变化，必须在 `/docs/user-guide/` 或 `/docs/developer/` 中同步更新相应章节，并确保示例命令可正确执行。
4.  **运行完整测试套件与代码检查** – 提交前执行 `make lint` 和 `make test`，确保无风格错误、类型错误（mypy）以及测试失败。所有警告必须被消除或显式标注为可接受。
5.  **提交 Pull Request 并完成评审流程** – 推送分支至你的 Fork 仓库，发起 Pull Request 到 `main` 分支，并填写 PR 模板中的每一项（变更摘要、测试结果、文档影响）。至少需要一位维护者批准且所有 CI 检查通过后方可合并。

## 常见问题

**Q: 探测引擎如何判断一个链接是否“可用”？是否仅依赖 HTTP 状态码？**

A: 不仅依赖 HTTP 状态码。系统默认采用三层判定逻辑：首先，必须返回 2xx 或 3xx（允许重定向，最多跟踪 5 次）；其次，响应体大小需大于配置的最小阈值（默认 1KB）且 Content-Type 匹配预期值（如 text/html 或 application/json）；最后，若该链接有历史内容签名，系统会对比当前响应主体的 SHA-256 哈希是否与最近三次成功记录中的至少一次匹配，以检测内容异常（如被劫持或返回占位页）。用户可通过 API 或 UI 为每个链接单独调整这些参数。

**Q: 如何处理上游域名频繁变更的情况？例如运动数据提供商每周更换一次域名。**

A: 项目内置了“别名组”功能。您可以将多个相关联的 URL 放入同一个逻辑组，并设置优先级顺序。探测引擎会按优先级尝试组内的 URL，直到首个成功响应为止。同时，当组内所有 URL 均失败时，系统会触发紧急通知，并可配置自动创建变更工单。您也可以通过 Webhook 接收器外部驱动更新——例如，订阅上游提供的变更通知 RSS，解析后自动修改组内列表。

**Q: 生产环境中，PostgreSQL 和 Redis 的推荐部署模式是什么？**

A: 对于生产级部署，我们强烈建议 PostgreSQL 使用主从流复制（至少一个同步备用节点）并配合定期 pg_dump 备份到对象存储；Redis 应采用 Sentinel 模式或 Redis Cluster，确保缓存高可用。默认配置文件提供了连接池（最大 50 个连接）和超时（5 秒）的参考值，但您需根据实际的探测并发量（建议每个节点不超过 200 个并行探测）和查询负载进行调整。此外，请务必启用 Redis 的持久化（AOF + RDB）以防止重启后缓存丢失导致数据库压力激增。

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:41:54
