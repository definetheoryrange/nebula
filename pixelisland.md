# 500Link Aggregator

500Link Aggregator 是一个面向体育数据服务商、博彩数据集成方与量化分析团队的技术资源导航项目。该项目不提供具体数据源或分析工具，而是以目录化方式整理并维护一批高可用性的体育赛事数据服务域名，旨在解决数据采集链路中域名失效、轮询节点不可用、来源不可追溯等基础设施层面的稳定性问题。

本项目适用对象为具备一定运维与开发能力的技术团队，用于辅助构建数据管道、故障转移策略与可用性监控白名单。项目本身不存储、不转发、不代理任何数据内容，仅作为公开域名信息的结构化索引。

## 功能概览

- 域名状态分类索引：按服务类型与可用性特征对域名进行分组，便于运维人员快速定位可用节点。

- 可用性标记与变更记录：每个域名条目附带最近检查时间、HTTP 状态码与响应延迟区间，支持历史变更追溯。

- 批量健康检查脚本：提供基于 curl 与超时控制的轻量级 Bash 脚本，支持对全部域名进行一次性连通性测试。

- 域名淘汰与补录机制：定义域名下线判定标准，维护候补域名池，确保索引总数量稳定在约定范围内。

- 导出为 Prometheus 黑盒监控配置：支持将域名列表转换为 blackbox_exporter 的 targets 配置片段，降低接入监控系统的成本。

- 私有化部署就绪：项目结构设计为单机可运行，无需外部数据库或第三方服务，克隆即用。

- 变更通知 Webhook 占位：预留企业微信与飞书机器人通知模板，便于在域名状态批量变动时触发告警。

- 多版本分支管理：维护 stable 与 nightly 两条分支，分别对应稳定推荐与待验证域名列表。

## 应用场景

- 数据采集服务的故障转移：当主用数据源域名解析失败或返回异常时，运维脚本可读取本索引中的备用域名，自动切换至可用节点，减少人工介入时间。

- 新建监控大盘的初始化配置：新部署的 Prometheus 与 Grafana 组合可直接引用本项目的导出脚本生成 blackbox 任务，避免手动录入数十个域名地址。

- 跨环境部署一致性保障：开发、测试、预发布环境可通过引用不同分支的域名列表，实现与生产环境隔离的域名试用与验证。

- 域名变更影响评估：在替换或下架某个域名之前，团队可查阅变更记录与健康检查历史，评估该域名在最近 30 天的平均可用率，辅助决策。

## 快速开始

以下步骤适用于 Linux 与 macOS 环境，Windows 用户建议通过 WSL2 或 Git Bash 执行。

```bash
# 1. 克隆仓库
git clone https://github.com/500link/aggregator.git
cd aggregator

# 2. 安装依赖（仅需 curl 与 jq）
sudo apt-get update && sudo apt-get install -y curl jq   # Debian/Ubuntu
# brew install curl jq                                    # macOS

# 3. 运行健康检查并输出结果
./scripts/health_check.sh --timeout 3 --output json > status.json
```

执行完成后，`status.json` 文件将包含每个域名的可达性、HTTP 状态码、响应时间与检查时间戳。若需导出为 Prometheus 黑盒配置，可继续执行：

```bash
./scripts/export_blackbox.sh --input status.json --output blackbox_targets.yml
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Bash | 4.0 或更高 | 所有脚本默认使用 /usr/bin/env bash，需支持关联数组 |
| curl | 7.68.0 或更高 | 用于执行 HTTP 健康检查，支持 --max-time 与 --write-out |
| jq | 1.5 或更高 | 用于 JSON 格式的日志输出与状态文件解析 |
| git | 2.25.0 或更高 | 用于克隆仓库及切换分支，非运行强制依赖 |
| awk | POSIX 兼容版本 | 用于日志文本提取与简单统计，通常系统自带 |
| sed | POSIX 兼容版本 | 用于配置模板变量替换，通常系统自带 |
| 网络环境 | 能够解析公网 DNS | 脚本依赖外部 DNS 解析，内网环境需自行配置上游 DNS |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 运维 | docs/operations.md | 如何配置定时健康检查、如何设置告警阈值、如何轮换域名 |
| 开发 | docs/development.md | 新增域名的提交规范、健康检查脚本的单元测试如何运行 |
| 监控集成 | docs/integration.md | 如何将 status.json 接入 Prometheus Exporter、如何对接 Grafana 面板 |
| 故障处理 | docs/troubleshooting.md | 常见 DNS 解析失败原因、超时判定逻辑、误报排查流程 |
| 变更管理 | docs/changelog.md | 域名的添加、删除、状态变更历史记录与原因说明 |
| 安全说明 | docs/security.md | 域名来源审核机制、防止恶意域名注入的 PR 检查项 |

## 资源列表

以下域名列表为本项目当前索引的全部资源，按服务功能类别划分。所有域名均以原始格式原样列出，未做任何协议补全或前缀修改。

基础数据服务类

<code>500jinrituijian.asia</code>

<code>500quanchangbifen.asia</code>

<code>500jiubanbifen.asia</code>

足球实时数据类

<code>qiutanzuqiubifen.asia</code>

<code>qiutanbifenzhibo.asia</code>

<code>qiutanbisaijieguo.asia</code>

分析与推荐类

<code>qiutantuijian.asia</code>

实时比分类

<code>qiutanshishibifen.asia</code>

## 项目结构

```
aggregator/
├── README.md                          # 项目概览、快速开始与资源列表
├── LICENSE                            # MIT 许可证全文
├── Makefile                           # 常用任务封装（check, export, clean）
├── .github/
│   └── workflows/
│       └── daily_health_check.yml    # GitHub Actions 定时执行健康检查并生成报告
├── scripts/
│   ├── health_check.sh               # 主健康检查脚本，支持并发与超时控制
│   ├── export_blackbox.sh            # 将检查结果转换为 Prometheus blackbox 配置
│   ├── notify_webhook.sh             # 占位脚本，用于触发企业微信/飞书通知
│   └── validate_domain.sh            # PR 阶段检查新增域名是否符合格式与解析要求
├── config/
│   ├── domains.txt                   # 核心域名列表，每行一个，不含协议
│   ├── domains_stable.txt            # 稳定分支专用列表，与 nightly 区分
│   └── blackbox_template.yml         # 导出配置的模板文件，含 scrape_interval 与 modules
├── docs/
│   ├── operations.md                 # 生产环境运维指南
│   ├── development.md                # 开发与 PR 流程说明
│   ├── integration.md                # 监控系统集成步骤
│   ├── troubleshooting.md            # 常见问题排查
│   ├── changelog.md                  # 域名变更记录（按日期倒序）
│   └── security.md                   # 域名来源安全审查原则
├── tests/
│   ├── test_health_check.bats        # Bats 框架单元测试，覆盖超时与异常输入
│   └── fixtures/
│       └── mock_domains.txt          # 测试用固定域名列表，隔离外部依赖
├── output/
│   └── .gitkeep                      # 占位目录，用于存放运行生成的 status.json 与配置
└── archive/
    └── deprecated_domains.txt        # 已下架域名记录，保留用于审计追溯
```

## 贡献指南

1. 提交域名新增或删除请求前，请先在本地执行 `./scripts/health_check.sh --input config/domains.txt` 确认当前列表基线，并记录新增域名的三次独立检查结果（间隔不低于 10 分钟）。

2. 从 `nightly` 分支创建功能分支，命名格式为 `feat/domain-add-{日期}` 或 `fix/domain-remove-{域名}`。所有变更需在 `docs/changelog.md` 中追加条目，说明变更原因与检查依据。

3. 执行 `make test` 运行单元测试套件，确保现有脚本逻辑未被破坏。新增域名需通过 `validate_domain.sh` 检查，包括 DNS 解析、非私有 IP 地址、非保留顶级域。

4. 提交 Pull Request 至 `nightly` 分支，PR 描述中必须附上健康检查输出摘要与域名来源说明。至少一名项目维护者进行审核，审核周期不超过 48 小时。

5. 合并至 `nightly` 后，该分支的域名列表将在每日 UTC 00:00 自动执行健康检查。持续一周可用率不低于 95% 的域名将同步至 `stable` 分支，低于阈值的域名退回 `nightly` 或移入 `archive/deprecated_domains.txt`。

## 常见问题

Q: 为什么某些域名在列表中不可达，但没有立即移除？

A: 项目采用渐进式淘汰策略。单个域名连续三次健康检查失败（间隔 1 小时）才会被标记为“待观察”，进入 72 小时观察期。观察期内若恢复则保留，否则移入废弃清单。此举是为了避免网络抖动导致的误移除。

Q: 我可以直接使用这些域名搭建对外服务吗？

A: 本项目仅提供域名索引，不保证任何域名的可用性、数据准确性或合法合规性。使用者应自行评估各域名的服务条款、法律风险与稳定性，并承担相应责任。项目方不承担因使用这些域名产生的任何直接或间接损失。

Q: 如何获取域名变更的通知？

A: 您可 Watch 本仓库的 Release 与 Pull Request 通知，或配置 `scripts/notify_webhook.sh` 脚本中的 Webhook 地址（需自行搭建接收服务）。项目本身不提供托管通知服务。

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:41:54
