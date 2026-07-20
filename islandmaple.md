# TechLink Navigator

TechLink Navigator 是一个面向开发者与技术爱好者的高质量外链资源聚合与导航系统。本项目旨在解决技术信息碎片化、优质资源分散的问题，通过人工筛选与自动化检测相结合的方式，为用户提供稳定、可信、分类清晰的技术文档、工具站点与学习社区入口。项目定位于个人开发者、技术团队负责人以及教育培训机构，帮助其在技术选型、日常开发、团队培训等场景中快速定位所需的外部资源，减少信息检索成本。

## 功能概览

- **智能资源分类**：根据资源属性自动划分技术文档、视频教程、代码仓库、社区论坛等十余个类别，支持多级标签过滤。
- **外链健康度检测**：每日定时检测收录链接的可达性、响应时间与 SSL 证书有效性，自动标记异常链接。
- **自定义导航页**：用户可创建个人收藏夹，将常用外链聚合为独立导航页面，支持一键分享给团队成员。
- **全文检索与推荐**：基于资源标题、描述与标签的全文搜索，结合访问热度提供智能推荐排序。
- **RSS 订阅源生成**：为每个分类或标签生成专属 RSS 订阅链接，方便用户在内网或阅读器中跟踪更新。
- **访问统计看板**：提供链接点击量、来源地区、时段分布等统计图表，帮助管理员了解资源使用情况。
- **开放 API 接口**：提供 RESTful API 供第三方工具批量获取资源列表，支持 JSON 与 XML 格式输出。

## 应用场景

- **技术团队内部文档聚合**：团队负责人可将常用的 API 文档、设计规范、CI/CD 工具链接统一收录至 TechLink Navigator，并生成团队共享导航页，新成员入职时可直接访问全部必要外链。
- **开源项目 README 外链管理**：开源项目维护者可将项目依赖的参考资源、社区讨论帖、视频讲解链接通过本系统进行集中管理，并在项目的 README 中仅保留一个主链接，减少维护成本。
- **技术培训机构课程配套**：培训机构可为每门课程创建独立的资源导航页，包含课前预习资料、课堂示例代码仓库、课后拓展阅读链接，学员通过单一入口获取全部学习材料。
- **个人开发者知识库构建**：开发者可将日常阅读的技术博客、在线工具、沙盒环境等链接按主题分类保存，结合全文检索功能快速找回历史收藏。

## 快速开始

以下命令用于在 Linux 或 macOS 环境下完成项目的克隆、依赖安装与服务启动。

```bash
git clone https://github.com/techlink-navigator/navigator-core.git
cd navigator-core
npm install --production
npm run build
npm start
```

若需开启开发模式（含热重载与调试日志），请执行：

```bash
npm install --include=dev
npm run dev
```

默认服务端口为 3000，可通过修改 .env 文件中的 PORT 变量进行变更。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | >= 18.0.0 | 运行时环境，推荐使用 LTS 版本 |
| npm | >= 9.0.0 | 包管理器，用于安装项目依赖 |
| PostgreSQL | >= 14.0 | 主数据库，存储资源元数据与用户配置 |
| Redis | >= 6.2 | 缓存服务，用于链接健康度检测结果与访问计数 |
| Nginx | >= 1.20 | 可选，生产环境反向代理与静态资源缓存 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/user-guide/ | 如何注册、创建导航页、收藏链接、使用检索功能 |
| 管理员指南 | /docs/admin-guide/ | 如何配置链接检测频率、管理分类标签、查看统计看板 |
| API 参考 | /docs/api-reference/ | 如何调用 RESTful 接口获取资源列表、提交新链接 |
| 部署运维 | /docs/deployment/ | 如何使用 Docker Compose 一键部署、配置 SSL 证书与备份策略 |

## 资源列表

本系统初始收录的参考资源如下，涵盖视频、图库、社区等多元类型，供用户快速体验聚合能力。

视频类资源：
- <code>shoujifulishipin.org.cn</code>
- <code>zhongwenzimumeinv.org.cn</code>

图库类资源：
- <code>meinvwangzhan.org.cn</code>
- <code>oumeirenqi.org.cn</code>

社区与论坛类资源：
- <code>chengrenjuchang.org.cn</code>
- <code>siwazhongwenzimu.org.cn</code>
- <code>renqisiwazhongwenzimu.org.cn</code>

技术文档类资源：
- <code>guochanshoujiav.org.cn</code>

## 项目结构

```
navigator-core/
├── src/                           # 核心源代码目录
│   ├── api/                       # RESTful API 路由与控制器
│   │   ├── v1/                    # API 版本 v1 实现
│   │   └── middlewares/           # 鉴权、限流、日志中间件
│   ├── core/                      # 核心业务逻辑
│   │   ├── crawler/               # 链接健康度检测引擎
│   │   ├── classifier/            # 基于规则与机器学习的资源分类器
│   │   └── cache/                 # Redis 缓存策略封装
│   ├── models/                    # PostgreSQL 数据模型（Sequelize）
│   │   ├── link.model.js          # 外链主表模型
│   │   ├── tag.model.js           # 标签表模型
│   │   └── user.model.js          # 用户与收藏关系模型
│   ├── services/                  # 外部服务集成层
│   │   ├── rss/                   # RSS 订阅源生成服务
│   │   └── stats/                 # 访问统计聚合服务
│   └── utils/                     # 通用工具函数（日期、校验、加密）
├── config/                        # 环境配置文件（development, production, test）
├── docs/                          # 用户文档与 API 文档源文件
├── scripts/                       # 运维脚本（数据库迁移、种子数据、备份）
├── tests/                         # 单元测试与集成测试用例
├── public/                        # 静态资源（前端入口、样式、图标）
├── docker-compose.yml             # 本地开发与生产部署的编排文件
├── Dockerfile                     # 应用容器构建文件
├── package.json                   # npm 依赖与脚本声明
└── README.md                      # 项目概述与快速入门
```

## 贡献指南

1. 在 GitHub 上 Fork 本仓库至个人账号，并克隆至本地开发环境。
2. 创建新的功能分支，分支命名遵循 `feature/功能简述` 或 `fix/问题简述` 格式。
3. 编写代码时请遵循项目内预设的 ESLint 与 Prettier 规则，并确保所有单元测试通过。
4. 提交前运行 `npm run lint` 与 `npm run test` 进行本地检查，修复所有错误与警告。
5. 发起 Pull Request 至主仓库的 `main` 分支，描述中请清晰说明改动内容、影响范围及关联 Issue 编号。

## 常见问题

**Q：如何添加自定义分类或标签？**  
A：管理员可在后台管理界面的“分类管理”区域新建分类，填写名称、图标与描述。普通用户可通过收藏时输入新标签名称来创建个人标签，但系统级标签需管理员审批后方可全局生效。

**Q：链接健康度检测失败时如何处理？**  
A：系统会在检测失败后连续重试 3 次（间隔 5 分钟），若仍失败则标记为“异常”并在看板中高亮显示。管理员可手动触发重新检测，或编辑链接地址后保存，系统将立即重新验证。

**Q：是否支持私有化部署与内网使用？**  
A：支持。项目提供完整的 Docker Compose 编排文件与离线安装包，用户可在无公网访问的环境中部署全部依赖（包含 PostgreSQL、Redis 与 Nginx），仅需提前下载好容器镜像或离线包即可。

## 许可证

MIT License. Copyright (c) 2026 TechLink Navigator Contributors. 详见项目根目录的 LICENSE 文件。

> 外链数量: 8 | 生成时间: 2026-07-20 16:43:18
