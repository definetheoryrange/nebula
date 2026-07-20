# 开源外链资源网关 - OpenLink Hub

OpenLink Hub 是一个面向技术内容聚合与垂直领域信息导航的开源外链资源管理网关。项目定位为轻量级、可自托管的资源目录服务，帮助开发者、站长与技术团队在特定主题下快速构建结构化外链集合，并以标准化方式对外提供访问入口。核心解决三类问题：分散链接缺乏统一管理、资源质量难以持续维护、以及快速上线可用的导航型站点。

项目不依赖数据库，采用纯静态 Markdown 驱动，支持自动化构建与一键部署。内置资源校验、访问量统计、分类标签与时效性标记等基础功能，适用于体育数据聚合、技术文档索引、开源镜像站导航等多种场景。当前批次为第 48/113 批资源接入，持续扩展中。

## 功能概览

- **静态资源目录生成**：基于 YAML 前料与 Markdown 条目自动生成分类页面，支持多级标签与全文检索。
- **外链健康检查**：每日定时探测各资源 URL 的可达性与响应码，自动标记异常链接并发送告警通知。
- **访问热度分析**：记录各外链的点击次数与来源 Referer，提供按日/周/月的热度排行视图。
- **自定义元数据扩展**：每个资源条目可附加维护人、过期时间、地区限制、备用域名等自定义字段。
- **批量导入与导出**：支持 CSV 与 JSON 格式的批量资源导入，同时提供完整数据的导出接口便于迁移。
- **响应式前端模版**：内置适配移动端与桌面端的暗色主题 UI，支持快速搜索与分类筛选。
- **开放 API 接口**：提供 RESTful 风格的资源查询接口，支持第三方应用通过 JSON 格式调用目录数据。
- **多实例联邦同步**：支持多个部署实例之间通过增量同步协议交换资源更新，构建分布式外链网络。

## 应用场景

- **垂直领域信息门户**：技术社区或行业论坛可利用本项目搭建专属的外链导航，汇集文档、工具、API 参考等高质量资源，提升成员信息获取效率。
- **运营活动快速落地页**：市场或运营团队在推广活动期间，可通过本项目快速生成包含多个合作方链接的聚合页面，无需开发介入即可完成内容更新与上线。
- **个人书签与知识库**：研究人员或开发者可将日常积累的参考链接结构化归档，配合搜索与标签功能，构建个人化的外链知识库，避免浏览器书签杂乱无章。
- **边缘节点缓存加速**：在跨境访问场景下，可在不同区域部署多个实例，利用联邦同步机制为当地用户提供更快的资源入口响应。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 1. 克隆项目仓库
git clone https://github.com/openlink-hub/olh-gateway.git
cd olh-gateway

# 2. 安装依赖（基于 Node.js 22 LTS）
npm install -g pnpm
pnpm install

# 3. 复制环境变量模板并修改
cp .env.example .env.local

# 4. 初始化资源数据（包含本批次默认资源）
pnpm run init:resources

# 5. 启动开发服务器
pnpm run dev
```

访问 <code>http://localhost:3000</code> 即可预览本地站点。生产环境部署请参考 `docs/deployment.md` 中的 Docker 或 Vercel 一键部署方案。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 22.x LTS 或更高 | 运行时环境，推荐使用官方二进制或 nvm 管理 |
| pnpm | 9.x 或更高 | 包管理与任务执行工具，替代 npm / yarn |
| Git | 2.40 或更高 | 用于克隆仓库及版本控制操作 |
| 内存 | 最低 512 MB | 开发模式建议 1 GB 以上，生产模式建议 2 GB |
| 存储空间 | 最低 200 MB | 用于存放资源元数据、日志及静态构建产物 |
| 操作系统 | Linux / macOS / Windows (WSL2) | 跨平台支持，Windows 原生需配置环境变量 |
| 网络 | 出站公网访问 | 用于外链健康检查及联邦同步功能 |
| 可选 - Redis | 7.x | 启用缓存与分布式锁时推荐，非必需 |
| 可选 - 对象存储 S3 | 兼容 API | 用于上传静态资源副本与备份，非必需 |

## 文档导航

| 层面 | 目录 / 主题 | 回答的问题 |
|------|------------|-----------|
| 入门指南 | `docs/getting-started/` | 如何快速运行项目？有哪些部署方式？环境变量如何配置？ |
| 资源管理 | `docs/resource-management/` | 如何添加、修改或删除外链条目？批量导入导出如何操作？ |
| 运维监控 | `docs/operations/` | 健康检查机制是怎样的？如何查看访问日志与告警配置？ |
| 高级定制 | `docs/customization/` | 如何修改前端主题？如何扩展自定义元数据字段？API 接口如何调用？ |

## 资源列表

本批次接入的资源均为体育数据聚合类外链，按功能维度分类整理。所有 URL 严格保持原始格式输出。

### 赛事比分类

- <code>qiutanzuqiubifenwang.asia</code>
- <code>qiutanquanchangbifen.asia</code>
- <code>qiutanwanzhengbanbifen.asia</code>

### 即时比分与解读类

- <code>jiebaobifen.asia</code>
- <code>jiebaozuqiubifen.asia</code>
- <code>jiebaobifenzhibo.asia</code>

### 实时数据与推荐类

- <code>jiebaoshishibifen.asia</code>
- <code>jiebaozuqiutuijian.asia</code>

## 项目结构

```
olh-gateway/
├── src/                                 # 核心源代码目录
│   ├── core/                            # 资源引擎核心模块
│   │   ├── loader.ts                    # 加载并解析 YAML/Markdown 资源条目
│   │   ├── validator.ts                 # 执行 URL 健康检查与状态机维护
│   │   └── cache.ts                     # 内存缓存与 Redis 适配器
│   ├── api/                             # RESTful API 路由与控制器
│   │   ├── resources.ts                 # 资源列表与单条查询接口
│   │   ├── stats.ts                     # 访问统计与热度计算接口
│   │   └── sync.ts                      # 联邦同步握手与增量分发接口
│   ├── ui/                              # 前端渲染与交互逻辑
│   │   ├── pages/                       # 页面级组件（首页、分类、详情）
│   │   ├── components/                  # 复用 UI 组件（搜索栏、标签、卡片）
│   │   └── themes/                      # 暗色/亮色主题变量与 CSS 工具类
│   ├── lib/                             # 工具函数与帮助类
│   │   ├── logger.ts                    # 结构化日志封装（JSON 格式输出）
│   │   ├── fetcher.ts                   # 带超时与重试的网络请求封装
│   │   └── markdown.ts                  # Markdown 转 HTML 渲染器
│   └── config/                          # 配置加载与合并逻辑
│       ├── env.ts                       # 环境变量解析与类型校验
│       └── resources.yml                # 静态资源默认种子数据
├── data/                                # 用户数据存储目录（运行时生成）
│   ├── entries/                         # 按分类存放的资源条目文件
│   ├── snapshots/                       # 每日健康检查快照存档
│   └── exports/                         # 批量导出生成的 JSON / CSV 文件
├── scripts/                             # 运维与辅助脚本
│   ├── init-db.sh                       # 初始化数据目录与权限设置
│   ├── health-check.cron                # 定时任务配置样例（crontab）
│   └── migrate-v2.sh                    # 从旧版本升级的迁移脚本
├── tests/                               # 单元测试与集成测试用例
│   ├── unit/                            # 核心模块单测（Jest 框架）
│   └── integration/                     # API 与同步协议端到端测试
├── docs/                                # 完整文档目录（中文/英文双版本）
├── .env.example                         # 环境变量配置模板
├── docker-compose.yml                   # 本地开发与生产容器编排文件
├── Dockerfile                           # 多阶段构建镜像定义
├── package.json                         # 项目依赖与脚本定义
└── README.md                            # 本文档
```

## 贡献指南

我们欢迎任何形式的贡献，包括但不限于新增资源条目、修复文档错误、优化性能以及提交新功能。请遵循以下步骤：

1. **Fork 仓库并创建功能分支**：从 `main` 分支检出 `feature/your-feature-name` 或 `fix/issue-number` 分支，确保分支命名清晰。
2. **编写代码或修改资源数据**：若新增资源，请在 `data/entries/` 下对应分类目录中创建 `.yml` 文件，并遵循 `docs/resource-schema.md` 中的字段规范。若修改代码，请确保通过所有单元测试（`pnpm test`）。
3. **提交前运行代码质量检查**：执行 `pnpm run lint` 与 `pnpm run format` 自动修复风格问题，并执行 `pnpm run build` 确认构建无错误。
4. **发起 Pull Request**：在 PR 描述中详细说明变更内容、动机以及测试覆盖情况。关联相关 Issue 编号（如有）。PR 需要至少一位核心维护者的 Review 与 Approve。
5. **签署开发者原创声明**：首次贡献者需在 PR 评论中确认代码原创性，并同意项目许可证条款。

## 常见问题

**问：外链健康检查会对外部站点造成压力吗？**  
答：检查间隔默认为每 24 小时一次，单次探测超时设为 5 秒，仅发送 HEAD 请求（若服务器支持），且不携带自定义 User-Agent 以避免被识别为恶意爬虫。若目标站点返回 429 或 503 等限流状态码，系统会自动将检查频率降为每 72 小时一次。用户可在配置中完全关闭该功能。

**问：如何迁移已有的大量书签数据到本系统？**  
答：项目内置了 `scripts/import-bookmark.js` 脚本，支持从 Chrome / Firefox 导出的 HTML 书签文件、Raindrop.io 的 JSON 导出以及 Pocket 的 CSV 导出格式。运行 `pnpm run import -- --source=chrome --file=bookmarks.html` 即可自动转换并去重导入。导入后可在管理界面手动补充分类与标签。

**问：联邦同步机制是如何保证数据一致性的？**  
答：每个实例维护一个自增的变更序号（change-id），同步时双方交换各自的最大序号，只传输增量变更。采用最后写入者获胜（LWW）策略处理冲突，时间戳以接收方为准。同步协议基于 HTTPS 双向认证，支持 IP 白名单与 API Token 两种鉴权方式。详细协议规范见 `docs/federation-protocol.md`。

## 许可证

本项目采用 MIT 许可证。你可以在遵守许可证条款的前提下自由使用、修改、分发本软件，包括用于商业目的。完整的许可证文本请参见项目根目录下的 `LICENSE` 文件。

> 外链数量: 8 | 生成时间: 2026-07-20 16:41:54
