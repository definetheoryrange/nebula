# QiuTan Resource Hub

QiuTan Resource Hub is a curated technical resource aggregation platform designed for developers, data analysts, and sports technology enthusiasts who require real-time access to structured football match data, live score feeds, and predictive analytics endpoints. The project serves as a centralized navigation and documentation layer for a family of specialized data services, eliminating the need for users to maintain individual integration contracts with multiple upstream providers. By providing a consistent interface specification, example payload structures, and health-check routines, QiuTan Resource Hub reduces the overhead of prototyping sports-data-driven applications from weeks to minutes. This repository does not host or proxy the underlying data; instead, it publishes machine-readable discovery metadata, authentication samples, and usage quotas for each registered endpoint, making it suitable for both ad-hoc scripting and production-grade deployment pipelines.

## 功能概览

- **Endpoint Discovery Registry** – Maintains a versioned catalog of all active score and prediction service URLs with last-seen timestamps and response latency percentiles.
- **Payload Schema Validation** – Provides JSON Schema definitions for every incoming and outgoing message contract, enabling compile-time type checking in TypeScript and Python.
- **Rate Limit Simulation** – Includes a built-in token-bucket simulator that replicates the throttle behavior of each upstream service for load-testing client backoff strategies.
- **Geographic Failover Routing** – Implements a client-side retry chain that automatically shifts traffic between regional mirrors based on HTTP status code and DNS resolution time.
- **Data Normalisation Pipeline** – Transforms heterogeneous score formats (e.g., half-time, full-time, extra-time, penalty shootout) into a unified event-stream model with millisecond precision.
- **Health Probe Aggregator** – Supplies a lightweight sidecar process that concurrently pings all registered endpoints every 30 seconds and exposes a Prometheus-compatible metrics endpoint.
- **CLI Query Tool** – Offers a single-binary command-line interface for ad-hoc lookups, supporting both interactive mode and cron-friendly JSON output.
- **Snapshot Archiver** – Enables scheduled dumps of the latest match states to local Parquet files, facilitating offline analysis and model training without repeated API calls.

## 应用场景

- **Real-Time Dashboard Development** – Frontend engineers can use the unified schema and sample responses to build and style live scoreboards, goal alerts, and possession heatmaps without waiting for backend API finalisation.
- **Predictive Model Training** – Data scientists can schedule hourly snapshots via the archiver module, accumulating a time-series dataset of match states, odds movements, and final results to train gradient-boosted tree or LSTM models for outcome forecasting.
- **Automated Betting System Prototyping** – Quantitative researchers can leverage the rate-limit simulator and failover logic to stress-test their arbitrage and hedging algorithms under realistic network conditions, ensuring production resilience.
- **Educational Workshops and Hackathons** – University instructors can distribute the repository as a self-contained lab environment, where students practice REST API consumption, error handling, and data visualisation using a consistent, documented dataset.
- **Integration Testing for Sports Apps** – QA teams can point their test harnesses to the staging endpoints listed in the registry, validating parsing logic, UI rendering, and push notification triggers against a wide variety of scoreline permutations.

## 快速开始

```bash
# Clone the repository with full history and submodules
git clone --recurse-submodules https://github.com/qiutan-resource-hub/core.git
cd core

# Install production dependencies and development toolchains
pip install -e .[dev] --extra-index-url https://download.pytorch.org/whl/cpu

# Initialise the local endpoint registry from the master manifest
python -m qiutan.cli init --manifest config/manifest.prod.yaml

# Run the integrated health probe and output a connectivity report
python -m qiutan.cli probe --parallel 8 --timeout 3.0 --output report.json

# Start the lightweight metrics sidecar on port 9090
python -m qiutan.sidecar --port 9090 --registry registry.sqlite3
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | >=3.10, <3.13 | Core runtime; type hints require 3.10+ pattern matching |
| aiohttp | >=3.9.0 | Async HTTP client used by the probe aggregator and failover router |
| pydantic | >=2.5.0 | Data validation and settings management via type-annotated models |
| prometheus-client | >=0.19.0 | Exposes the `/metrics` endpoint for Prometheus scraping |
| pyarrow | >=14.0.0 | Parquet serialisation for the snapshot archiver module |
| click | >=8.1.0 | CLI framework providing subcommand routing and help generation |
| pytest | >=8.0.0 | Development-only test runner for unit and integration tests |
| mypy | >=1.8.0 | Development-only static type checker enforcing strict optional and no-any-return |
| sqlite3 | built-in | Embedded database for local registry cache and probe history |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | `docs/getting-started/` | 如何配置第一个查询、理解响应结构、处理常见 HTTP 错误码 |
| 端点规范 | `docs/endpoints/` | 每个 URL 的请求方法、必填头信息、查询参数释义和示例 curl 命令 |
| 运维手册 | `docs/operations/` | 如何自定义健康检查阈值、调整回退策略、迁移 registry 数据库 |
| 架构设计 | `docs/architecture/` | 各模块的耦合关系、事件循环模型、线程池边界和内存占用估算 |
| 性能调优 | `docs/performance/` | 连接池大小建议、DNS 缓存 TTL、批量查询批处理大小的经验值 |
| 贡献者指引 | `docs/contributing/` | 代码风格约定、提交信息格式、新增 endpoint 的 PR 模板 |

## 资源列表

### 实时比分服务

- <code>qiutanquanchangbifen.asia</code>
- <code>qiutanwanzhengbanbifen.asia</code>

### 即时比分与直播数据

- <code>jiebaobifen.asia</code>
- <code>jiebaozuqiubifen.asia</code>
- <code>jiebaobifenzhibo.asia</code>

### 实时更新与预测服务

- <code>jiebaoshishibifen.asia</code>
- <code>jiebaozuqiutuijian.asia</code>
- <code>jiebaozuqiuyuce.asia</code>

## 项目结构

```
core/
├── config/                                 # 环境配置与静态清单
│   ├── manifest.prod.yaml                  # 生产环境端点列表及权重
│   ├── manifest.staging.yaml               # 预发布环境隔离配置
│   └── schema/                             # JSON Schema 定义目录
│       ├── match_event_v1.json             # 统一比赛事件模型
│       └── prediction_result_v2.json       # 预测结果输出模型
├── src/qiutan/                             # 核心源码包
│   ├── __init__.py                         # 包版本及导出符号
│   ├── client/                             # HTTP 客户端封装及重试策略
│   │   ├── connector.py                    # aiohttp 连接池工厂
│   │   └── failover.py                     # 区域感知的故障转移逻辑
│   ├── models/                             # Pydantic 数据实体
│   │   ├── score.py                        # 比分、半场、加时数据类
│   │   └── registry.py                     # 端点注册表记录模型
│   ├── probes/                             # 健康检查与延迟测量
│   │   ├── aggregator.py                   # 并发探针调度器
│   │   └── metrics.py                      # Prometheus 指标收集器
│   ├── cli/                                # Click 命令行子命令
│   │   ├── init.py                         # 初始化本地 registry
│   │   ├── probe.py                        # 手动执行探测并输出报告
│   │   └── archive.py                      # 触发快照归档任务
│   ├── sidecar/                            # 独立 sidecar 进程入口
│   │   └── server.py                       # 启动 /metrics 与 /health 端点
│   └── utils/                              # 通用工具函数
│       ├── time.py                         # 时区转换与 ISO 格式化
│       └── crypto.py                       # HMAC 签名生成（预留）
├── tests/                                  # 单元测试与集成测试
│   ├── unit/                               # 纯逻辑测试，无外部依赖
│   └── integration/                        # 需联网或 mock 服务的测试
├── scripts/                                # 运维辅助脚本
│   ├── migrate_registry.py                 # registry 数据库迁移工具
│   └── seed_dummy_data.py                  # 开发环境填充示例数据
├── docs/                                   # 完整文档（参见上一节）
├── requirements.txt                        # 生产环境依赖锁定
├── requirements.dev.txt                    # 开发环境额外依赖
├── pyproject.toml                          # PEP 621 项目元数据
├── Makefile                                # 常用任务快捷指令（lint, test, run）
└── README.md                               # 本文件
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从主分支 `main` 切出 `feature/your-topic` 或 `fix/issue-number` 分支，确保分支名称语义化。
2.  **编写或更新单元测试** – 所有新增端点适配器或转换逻辑必须附带至少 3 个正向及 2 个负向测试用例，覆盖率不得低于 90%。
3.  **执行完整测试套件与类型检查** – 在提交前运行 `make test` 和 `make lint`，确保无 mypy 错误、无 flake8 警告，且所有 pytest 通过。
4.  **更新文档与示例** – 若修改了公共接口或配置格式，请同步更新 `docs/` 下对应的 Markdown 文件，并在 `examples/` 中补充新的 curl 或 Python 片段。
5.  **提交 Pull Request 并填写模板** – 清晰描述变更动机、影响范围、测试结果以及是否向后兼容。至少一名维护者审批后方可合并。

## 常见问题

**问：多个端点返回的比分数据存在时间戳偏差，如何对齐到统一时钟？**

答：建议使用响应头中的 `Date` 字段或服务端返回的 `server_timestamp` 毫秒值，客户端侧采用 NTP 同步的本地时钟仅计算相对延迟。本仓库的 `models/score.py` 中已提供 `normalise_timestamp` 方法，能够自动处理秒级与毫秒级时间戳并转换为 UTC 时区。若上游未提供时间戳，请启用 `probes/aggregator.py` 中的 `clock_skew_detection` 标志，该选项会通过往返时间估算偏移量。

**问：注册表中某个端点频繁超时，是否应该将其从配置中移除？**

答：不应直接移除，而建议调整 `manifest.prod.yaml` 中该端点的 `weight` 字段为较低值（例如从 10 降至 1），并设置 `failover_threshold` 为 3 次连续失败。sidecar 进程会自动将其标记为 `degraded` 状态，并在 `/metrics` 中暴露 `endpoint_health_status` 指标。当连续失败超过 20 次后，系统会发出告警日志，但保留该条目以供人工复盘。恢复后，可通过 `python -m qiutan.cli probe --reset` 重置其状态。

**问：如何自定义快照存档的存储位置和压缩格式？**

答：请在项目根目录下创建 `.env` 文件，设置 `SNAPSHOT_BASE_DIR=/path/to/your/storage` 以及 `SNAPSHOT_COMPRESSION=zstd`。当前支持的压缩算法包括 `uncompressed`、`snappy`、`gzip` 和 `zstd`。若未指定，默认使用 `snappy` 并存放在 `./data/snapshots/` 下，按 `YYYY/MM/DD/` 子目录分层。

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:42:41
