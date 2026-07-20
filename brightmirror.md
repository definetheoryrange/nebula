# NovaIndex

NovaIndex 是一个面向技术调研、内容聚合与数字档案构建的轻量级外链资源索引系统。项目定位为技术社区、内容创作者与研究人员提供可自部署、高可读性的外链资源导航与元数据归档方案，解决分散链接难以归类、版本失控和访问不可靠的问题。通过结构化的资源清单与静态文档站点生成能力，NovaIndex 可快速将非结构化的 URL 集合转化为具备目录、标签、备注与状态监控的活文档。

---

## 功能概览

- **批量外链管理**：支持通过 YAML 或 CSV 导入链接清单，自动生成 Markdown 目录页与详情页，便于社区共建。
- **资源状态探测**：集成轻量级 HTTP 探活模块，定时检测链接可达性，输出可用性报表，标记失效或重定向资源。
- **标签与分类引擎**：支持多级标签（如 `#video`, `#domain`, `#archive`），可按批次、领域、语种进行快速筛选。
- **静态站点生成**：内置 Handlebars 模板引擎，可将资源数据渲染为适合 GitHub Pages 或 Cloudflare Pages 部署的纯静态 HTML 文档。
- **变更审计日志**：记录每次资源增删改的操作人、时间戳与变更摘要，满足团队协作与合规追溯需求。
- **自定义元数据字段**：允许为每条外链附加备注、过期日期、备用域名或快照链接，提升资源韧性。
- **搜索与过滤接口**：提供基于 Lunr.js 的离线全文搜索，支持按域名后缀、状态码、更新时间范围过滤。
- **开放 API 端点**：暴露 RESTful JSON 接口，便于与其他运维系统或自动化脚本集成。

---

## 应用场景

- **技术社区资源导航**：开源社区可利用 NovaIndex 整理项目依赖的文档镜像、教程站点、CDN 源站地址，避免社区成员在分散的第三方平台反复查找。
- **数字档案长期保存**：图书馆或数字人文团队可将历史网页快照与关联外链统一编目，配合状态探测自动标记失效链接，辅助决策是否需要重新采集。
- **合规审查辅助工具**：法务或内容审核团队可批量导入待审查域名列表，通过自定义标签（如“待复核”“低风险”“已屏蔽”）进行状态跟踪，并导出审计报告。
- **个人知识库增强**：研究员或个人开发者可将浏览器收藏夹导出的链接集导入 NovaIndex，结合备注字段记录阅读心得或重要摘要，形成可搜索的私人外链数据库。

---

## 快速开始

以下命令适用于 Linux / macOS / Windows WSL 环境。请确保已安装 Git、Node.js 18+ 与 pnpm。

```bash
# 1. 克隆项目仓库
git clone https://github.com/novaindex/novaindex.git
cd novaindex

# 2. 安装依赖
pnpm install

# 3. 构建核心模块并启动开发服务器
pnpm run build
pnpm run start:dev
```

默认启动端口为 `3000`，访问 `http://localhost:3000` 可查看示例资源看板。如需导入自有链接清单，请将数据文件置于 `data/sources/` 目录下，并执行 `pnpm run import`。

---

## 安装要求

| 依赖项         | 必需版本            | 说明                                                                 |
|----------------|---------------------|----------------------------------------------------------------------|
| Node.js        | >=18.12.0           | 运行时环境，需支持 ES Module 与 Fetch API                           |
| pnpm           | >=7.0.0             | 包管理器，用于快速安装依赖与执行工作区脚本                           |
| Git            | >=2.30.0            | 版本控制工具，用于克隆仓库与提交变更                                 |
| SQLite3        | 系统自带或手动安装   | 轻量级嵌入式数据库，用于存储资源元数据与审计日志（生产模式推荐）     |
| 内存           | >=512 MB            | 开发模式最低内存要求，生产环境建议 1 GB 以上                         |
| 磁盘空间       | >=200 MB            | 包含依赖、构建产物与示例数据，实际占用随资源条目数线性增长           |
| 操作系统       | Linux / macOS / WSL | 原生支持 Windows 可通过 WSL2 运行，未在原生 Windows 下完整测试       |

---

## 文档导航

| 层面           | 目录/文件                     | 回答的问题                                                           |
|----------------|-------------------------------|----------------------------------------------------------------------|
| 用户手册       | `docs/user-guide/`            | 如何导入链接、设置探活策略、生成静态站点、自定义主题样式？           |
| 开发指南       | `docs/development/`           | 如何扩展探活器、增加新元数据字段、编写迁移脚本或调试 API 接口？      |
| 运维参考       | `docs/operations/`            | 如何配置反向代理、定时任务、备份 SQLite 数据库以及监控探活日志？     |
| 设计文档       | `docs/design/architecture.md` | 系统模块划分、数据流走向、插件机制与未来兼容性设计是怎样的？         |
| 贡献者手册     | `CONTRIBUTING.md`             | 代码风格要求、提交信息规范、测试流程和 PR 合并规则有哪些？           |
| 常见问题       | `docs/faq.md`                 | 探活超时如何调整、迁移到 PostgreSQL 是否可行、如何批量更新标签？     |

---

## 资源列表

### 媒体与娱乐类资源

- <code>bajiaoshipinapp.org.cn</code>
- <code>jiujiuyeye.org.cn</code>
- <code>zhongwenzimusiwa.org.cn</code>
- <code>renqiyouma.org.cn</code>
- <code>xiaodiaowang.org.cn</code>
- <code>chengrenjingpin18.org.cn</code>
- <code>jiujiurenqi.org.cn</code>
- <code>91shaofu.org.cn</code>

以上为第 95/113 批次收录的外链数据，均以原始域名形式记录。NovaIndex 不对其内容作任何背书，仅提供结构化索引与可用性监控能力，具体使用需遵守对应网站的条款与当地法律法规。

---

## 项目结构

```
novaindex/
├── src/                           # 核心源代码目录
│   ├── core/                      # 核心模块：配置加载、数据库连接、事件总线
│   │   ├── config.ts              # 环境变量与配置文件解析
│   │   └── database.ts            # SQLite 连接池与迁移管理
│   ├── crawler/                   # 资源探活与元数据抓取子模块
│   │   ├── probe.ts               # HTTP 探活器，支持超时重试与状态码判定
│   │   └── parser.ts              # 从响应头或 HTML 提取标题、编码信息
│   ├── importer/                  # 导入器：支持 CSV / JSON / YAML 格式
│   │   ├── csv-loader.ts
│   │   └── yaml-loader.ts
│   ├── generator/                 # 静态站点生成器
│   │   ├── markdown-renderer.ts   # 将资源数据渲染为 Markdown 列表页
│   │   └── html-template.ts       # Handlebars 模板包装与布局
│   ├── api/                       # RESTful API 路由层
│   │   ├── resources.ts           # 资源增删改查端点
│   │   └── health.ts              # 探活状态聚合接口
│   └── cli/                       # 命令行工具入口
│       ├── import.ts              # 导入命令实现
│       └── probe.ts               # 手动触发全量探测命令
├── data/                          # 数据存储与示例文件
│   ├── sources/                   # 存放待导入的原始链接文件
│   │   └── sample-batch-95.csv    # 第 95 批资源示例数据
│   └── db/                        # SQLite 数据库文件存放位置
│       └── novaindex.db
├── docs/                          # 完整文档目录（用户手册、开发指南、运维手册）
│   ├── user-guide/                # 面向最终用户的教程与截图
│   ├── development/               # 面向贡献者的架构与调试说明
│   └── operations/                # 面向运维人员的部署与监控方案
├── tests/                         # 单元测试与集成测试
│   ├── unit/                      # 探活器、解析器、导入器的隔离测试
│   └── integration/               # API 与数据库交互的端到端测试
├── scripts/                       # 辅助脚本：备份、迁移、种子数据填充
│   ├── backup-db.sh
│   └── seed-demo.sh
├── package.json                   # 项目依赖与脚本定义
├── pnpm-lock.yaml                 # 依赖锁定文件
├── tsconfig.json                  # TypeScript 编译配置（严格模式）
├── .env.example                   # 环境变量示例（含探活超时、端口、日志级别）
└── README.md                      # 项目入口文档（即本文件）
```

---

## 贡献指南

1. **阅读行为准则与设计愿景**：在提交任何代码或文档前，请先阅读 `CODE_OF_CONDUCT.md` 与 `docs/design/architecture.md`，确保理解项目的核心理念——稳定性优先、元数据中立、社区驱动。
2. **选择未认领的 Issue 或提议新特性**：访问 GitHub Issues 查看标签为 `help wanted` 或 `good first issue` 的任务。若新增特性或修改数据结构，请先创建 Discussion 进行方案讨论，避免重复劳动或不兼容变更。
3. **分叉主仓库并创建功能分支**：从 `main` 分支签出本地分支，命名遵循 `feature/` 或 `fix/` 前缀，例如 `feature/add-http2-probe`。所有提交需通过 ESLint 与 Prettier 格式检查。
4. **编写单元测试并更新相关文档**：新增或修改功能时，需在 `tests/` 下补充对应的单元测试；若影响用户可见行为，须同步更新 `docs/user-guide/` 下的说明和 `README.md` 中的示例。
5. **提交 Pull Request 并等待 Code Review**：PR 描述需清晰说明变更动机、测试覆盖范围与破坏性变更提示。至少需要两位维护者批准方可合并。合并后 CI 将自动构建并部署预览站点。

---

## 常见问题

**Q: 探活检测频繁出现超时或连接拒绝，应如何调整？**

A: 可在项目根目录的 `.env` 文件中修改 `PROBE_TIMEOUT`（默认 5000 毫秒）和 `PROBE_RETRY`（默认 2 次）。若目标站点有防火墙限制，建议在 `PROBE_USER_AGENT` 中设置常见浏览器 UA 字符串。同时检查网络环境是否需配置代理，通过 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量指定。

**Q: 是否可以直接使用 MySQL 或 PostgreSQL 替代 SQLite？**

A: 目前官方仅内置 SQLite 适配器以降低部署门槛。若需要使用外部关系型数据库，可基于 `src/core/database.ts` 中的抽象接口自行实现连接池与查询适配层。社区已有实验性 PostgreSQL 驱动，但尚未合并到主干，您可关注 `feat/postgres-support` 分支的进展。

**Q: 如何备份已导入的资源数据与审计日志？**

A: 默认 SQLite 数据库文件位于 `data/db/novaindex.db`，建议使用 `scripts/backup-db.sh` 脚本定期导出 SQL 转储或直接复制该文件。审计日志表 `audit_logs` 记录全部变更，您也可通过 API `GET /api/audit?since=...` 导出指定时间范围内的操作记录用于归档。

---

## 许可证

MIT License。详见项目根目录的 `LICENSE` 文件。您可以自由使用、修改、分发本软件，但需保留原始版权声明与免责条款。

> 外链数量: 8 | 生成时间: 2026-07-20 16:42:53
