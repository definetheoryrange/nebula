# NexusIndex

NexusIndex 是一个面向技术调研、学术辅助与数字档案管理领域的轻量化资源导航系统。项目定位为可自托管的精选外链聚合与元数据索引工具，主要服务于需要高频访问特定垂直领域公开信息源的研究者、内容审核员以及数据标注团队。系统核心解决三类问题：分散优质链接的集中化管理、链接可用性的基础监测、以及按业务语义对在线资源进行快速分类检索。NexusIndex 不提供任何实质内容存储或代理转发功能，仅作为公开互联网资源的索引层与访问入口，严格遵守 robots.txt 协议与相关法律法规。

## 功能概览

- **多级分类索引** 支持自定义分类树与多标签绑定，允许同一链接归属于不同业务视图，适应复杂调研场景的交叉归类需求。

- **链接存活探测** 内置异步 HTTP 探活机制，周期性检测收录链接的响应状态码与页面标题变化，辅助管理员及时清理失效条目。

- **全文元数据检索** 基于标题、描述、分类标签、收录时间等字段构建倒排索引，支持模糊匹配与精确筛选，响应时间控制在 300 毫秒以内。

- **批量导入导出** 提供 CSV 与 JSON 格式的批量链接导入接口，支持从现有书签文件或表格数据快速迁移，同时支持按分类导出完整索引报告。

- **访问统计看板** 记录每个链接的点击次数与最近访问时间，提供热点排行与低频条目提醒，帮助优化索引库的维护优先级。

- **只读只存架构** 系统不缓存任何目标页面内容，不转发流量，不解析页面正文，仅存储用户主动提交的链接元数据，从设计层面规避版权与合规风险。

- **多用户协作支持** 基于角色的访问控制（管理员、编辑者、只读观察员），允许团队协同维护索引库，操作日志完整可追溯。

- **开放 API 接口** 提供 RESTful 风格的查询与更新 API，便于与其他内部系统集成，支持 Webhook 通知机制用于链接状态变更告警。

## 应用场景

- **垂直领域信息聚合** 研究机构或行业分析团队可将 NexusIndex 用作内部知识库的入口层，将散落在各处的行业报告源、统计数据库、政策发布页统一收录，并按照地域、时间、主题等多维度打标，极大缩短调研前期信息搜集时间。

- **内容安全审核辅助** 内容审核团队可利用系统的分类索引与存活探测功能，快速定位需要定期复核的参考源列表，结合访问统计看板识别高频引用源，优化审核抽样策略。系统本身不接触任何审核对象，仅作为参考链接的管理工具。

- **数据标注任务支撑** 数据标注外包团队可通过 NexusIndex 为标注员提供统一的参考信息入口，将不同项目所需的规范文档、示例页面、术语库链接按项目隔离，并利用只读权限控制确保标注员仅能访问授权范围内的资源，降低信息泄露风险。

- **个人知识管理升级** 技术研究者或开源爱好者可将 NexusIndex 作为个人书签工具的高级替代方案，利用分类索引、全文检索与访问统计功能构建长期维护的个人信息资产库，摆脱浏览器书签的扁平化与不可移植性限制。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议通过 WSL2 或 Docker 方式运行。

```bash
# 1. 克隆项目仓库
git clone https://github.com/nexus-index/nexusindex.git
cd nexusindex

# 2. 安装依赖（使用 pip 与虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 3. 初始化配置文件与本地数据库
cp .env.example .env
python scripts/init_db.py

# 4. 启动开发服务器
python app.py --host 127.0.0.1 --port 8080
```

启动后访问 <code>http://127.0.0.1:8080</code> 即可进入系统首页，默认管理员账号为 <code>admin</code>，初始密码在首次启动时由初始化脚本输出至终端日志。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 - 3.11 | 核心运行环境，3.12 及以上版本暂未完成兼容性测试 |
| SQLite | 3.35.0+ | 内置数据库引擎，用于存储链接元数据与用户配置，无需额外安装 |
| Redis | 7.0+ | 可选依赖，用于提升会话管理与访问统计的性能，生产环境强烈建议部署 |
| Node.js | 18.x LTS | 仅用于前端静态资源构建，后端运行无需 Node 环境 |
| Nginx | 1.24+ | 生产环境推荐反向代理服务器，用于静态文件缓存与负载均衡 |
| Docker | 24.0+ | 可选，提供容器化一键部署方案，详见部署文档 |
| Git | 2.30+ | 版本控制工具，用于克隆仓库与后续更新同步 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 部署运维 | <code>/docs/deployment/</code> | 如何配置生产级 HTTPS、如何设置反向代理、如何备份数据库、如何使用 Docker Compose 一键拉起全套服务 |
| 用户手册 | <code>/docs/user-guide/</code> | 如何添加新链接、如何创建分类标签、如何导入导出数据、如何查看访问统计、如何进行全文检索 |
| 开发者文档 | <code>/docs/developer/</code> | API 接口定义是什么、插件扩展机制如何工作、如何自定义前端主题、单元测试如何编写与运行 |
| 管理指南 | <code>/docs/admin/</code> | 如何管理用户权限、如何调整探活策略、如何配置 Webhook 告警、如何清理历史日志数据 |

## 资源列表

以下为 NexusIndex 项目收录的精选公开信息源索引，所有链接按原始格式原样列出，不作任何协议补全或域名规范化处理。

**影视与娱乐类**

<code>guochanjiatingyingyuan.org.cn</code>

<code>zhongwenyouma.org.cn</code>

<code>zhongwenrenqi.org.cn</code>

<code>renqishaofu.org.cn</code>

<code>bajiaoshipinapp.org.cn</code>

<code>jiujiuyeye.org.cn</code>

<code>zhongwenzimusiwa.org.cn</code>

<code>renqiyouma.org.cn</code>

## 项目结构

```
nexusindex/
├── app/                                 # 核心应用目录
│   ├── __init__.py                      # 应用工厂与配置加载
│   ├── routes/                          # 路由层，处理 HTTP 请求与响应
│   │   ├── index.py                     # 首页与分类浏览路由
│   │   ├── search.py                    # 全文检索与筛选路由
│   │   ├── link.py                      # 链接增删改查路由
│   │   └── api/                         # RESTful API 版本路由
│   ├── services/                        # 业务逻辑层
│   │   ├── crawler.py                   # 链接存活探测与元数据抓取服务
│   │   ├── indexer.py                   # 倒排索引构建与查询服务
│   │   └── stats.py                     # 访问统计与排行计算服务
│   ├── models/                          # 数据模型层，ORM 定义与数据库迁移
│   │   ├── link.py                      # 链接实体模型
│   │   ├── category.py                  # 分类与标签模型
│   │   └── user.py                      # 用户与权限模型
│   └── utils/                           # 通用工具函数
│       ├── validator.py                 # URL 格式校验与规范化工具
│       ├── http.py                      # 异步 HTTP 请求封装
│       └── logger.py                    # 日志配置与格式化输出
├── frontend/                            # 前端静态资源源码
│   ├── src/                             # Vue 组件与样式源码
│   ├── dist/                            # 构建输出目录，由 Nginx 托管
│   └── package.json                     # 前端依赖管理
├── scripts/                             # 运维与开发辅助脚本
│   ├── init_db.py                       # 数据库初始化脚本
│   ├── backup.py                        # 数据备份脚本
│   └── migrate.py                       # 数据库结构迁移脚本
├── tests/                               # 单元测试与集成测试
│   ├── unit/                            # 单元测试用例
│   └── integration/                     # 接口集成测试
├── docs/                                # 完整项目文档，按层面分目录
├── .env.example                         # 环境变量配置模板
├── requirements.txt                     # Python 依赖清单
├── docker-compose.yml                   # Docker Compose 编排文件
└── README.md                            # 项目说明文档（当前文件）
```

## 贡献指南

1. 阅读项目行为准则与贡献者许可协议，确认您的贡献符合开源精神与法律要求。所有代码提交需附带签署 <code>CLA</code>（贡献者许可协议），具体签署方式见 <code>/docs/contributing/cla.md</code>。

2. 从 <code>develop</code> 分支创建个人功能分支，分支命名遵循 <code>feature/功能简述</code> 或 <code>fix/问题简述</code> 格式。禁止直接向 <code>main</code> 或 <code>develop</code> 分支推送代码。

3. 编写或更新单元测试，确保测试覆盖率达到 80% 以上。新增功能必须附带对应的测试用例，缺陷修复需提供回归测试用例以验证修复有效性。

4. 提交 Pull Request 前执行本地完整测试套件 <code>pytest tests/</code> 与代码风格检查 <code>flake8 app/</code>，确保全部通过且无新增警告。

5. 提交 Pull Request 后等待至少一位核心维护者的 Code Review。审核通过后由维护者负责合并至 <code>develop</code> 分支，定期随版本发布同步至 <code>main</code> 分支。

## 常见问题

**问：NexusIndex 是否会缓存或存储目标链接的页面内容？**

答：不会。NexusIndex 仅存储用户主动提交的链接标题、描述、分类标签等元数据，以及基础的链接存活状态（HTTP 状态码）。系统不会抓取、缓存、代理或转发目标页面的任何正文内容，所有访问均由用户浏览器直接发起。存活探测仅发送 HEAD 请求获取响应头信息，不下载页面主体。

**问：系统能否用于商业环境或生产级高并发场景？**

答：可以。NexusIndex 采用无状态应用设计，前端静态资源与后端 API 可分离部署，配合 Redis 会话共享与 Nginx 负载均衡，可水平扩展至多实例。系统默认使用 SQLite 适用于中小规模索引库（链接数量 10 万以内），若链接量级超过此范围，建议迁移至 PostgreSQL 或 MySQL，项目提供完整的数据库迁移支持。具体的生产部署调优参数请参考 <code>/docs/deployment/scaling.md</code>。

**问：如何从其他书签工具或浏览器导出数据迁移至 NexusIndex？**

答：系统提供通用的 CSV 导入模板，用户只需将浏览器导出的 HTML 书签文件或第三方工具导出的 JSON 数据按照模板字段映射后，通过管理后台的批量导入功能上传即可。对于常见的浏览器书签格式（Chrome、Firefox、Edge），社区维护了专用的转换脚本，位于 <code>/scripts/convert_bookmarks.py</code>，可将书签文件直接转换为 NexusIndex 的导入格式。详细步骤参考 <code>/docs/user-guide/migration.md</code>。

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:43:13
