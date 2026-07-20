# OpsLink Navigator

OpsLink Navigator 是一个面向技术运维人员、系统架构师与开源项目维护者的外链资源聚合与导航系统。项目定位为轻量级技术资源目录服务，旨在解决技术文档阅读过程中外部参考链接分散、关键资源检索效率低下的问题。通过结构化分类与标签索引机制，OpsLink Navigator 将高频使用的技术博客、数据看板、实时状态页与官方文档入口整合为单一可维护的知识库底座，适用于个人书签迁移、团队技术栈 onboarding 及开源项目外围参考链标准化等场景。

## 功能概览

- **资源分类目录树**：按技术领域、服务类型、数据时效性等维度对链接进行多级标签管理，支持自定义分类插件。

- **链接可用性探测**：内置基于 HTTP 状态码与响应时间的被动检测任务，对收录资源进行每日存活标记。

- **全文标签检索**：为每个资源条目附加关键词与描述元数据，支持模糊搜索与正则表达式过滤。

- **批量导入导出**：支持 CSV、JSON 及 Markdown 列表格式的链接批量迁移，便于与其他书签工具互通。

- **只读 API 接口**：提供 RESTful 风格的目录查询接口，允许第三方工具以 JSON 格式拉取全量或分类资源列表。

- **访问统计看板**：记录资源条目的点击频次与最后访问时间，辅助识别高价值参考源。

- **快照注释留存**：每条链接可附带独立于源站内容的本地注释块，用于记录个人阅读笔记或团队共识说明。

- **多用户权限分级**：支持管理员、编辑者、只读访客三种角色，适合团队内部知识库协作维护。

## 应用场景

- **团队技术文档标准化**：将项目依赖的第三方库主页、监控面板与日志系统入口统一收录，替换文档中零散的裸 URL，降低新成员环境搭建时的信息查找成本。

- **个人技术阅读工作流**：运维工程师可将日常关注的性能对比站点、竞品分析博客与官方更新公告归入同一导航页，配合标签检索快速定位历史参考信息。

- **开源项目外围参考链**：开源项目维护者可在 README 或 Wiki 中引用 OpsLink Navigator 生成的静态目录页，为贡献者提供统一的背景资料入口，避免在 issue 讨论中反复粘贴相同外部链接。

- **离线文档配套资源包**：在无外网环境的内部部署场景中，OpsLink Navigator 可导出资源清单配合本地镜像服务，形成可离线查阅的参考索引。

- **技术雷达调研素材库**：架构评审委员会可将候选技术组件的官网、性能基准测试报告与社区活跃度统计聚合管理，辅助选型决策。

## 快速开始

以下指令适用于 Linux 与 macOS 环境，Windows 用户建议通过 WSL2 或 Git Bash 执行。

```bash
# 克隆主仓库
git clone https://github.com/opslink-navigator/navigator-core.git
cd navigator-core

# 安装项目级依赖（使用 pip 虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化本地资源数据库并导入种子链接
python manage.py migrate
python manage.py loaddata seed_links.json

# 启动开发服务器（默认监听 8000 端口）
python manage.py runserver 0.0.0.0:8000
```

访问 `http://localhost:8000` 即可浏览默认资源目录。管理员初始账号密码为 `admin` / `navigator2025`，首次登录后请及时修改。

## 安装要求

| 依赖组件          | 必需版本        | 说明                                                                 |
|-------------------|-----------------|----------------------------------------------------------------------|
| Python            | 3.9 - 3.12      | 核心运行环境，低于 3.9 版本将无法解析类型注解与异步语法。             |
| Django            | 4.2.11          | 主框架版本，与当前 LTS 系列保持对齐，确保安全补丁更新通道。           |
| SQLite            | 3.35+           | 默认嵌入式数据库，生产环境建议切换至 PostgreSQL 14+。                 |
| Redis             | 6.2+            | 可选依赖，用于缓存 API 响应与会话存储，非必需但推荐。                 |
| requests          | 2.31.0          | 用于可用性探测的 HTTP 客户端库，需保持与 OpenSSL 兼容。               |
| beautifulsoup4    | 4.12.0          | 用于解析资源页面标题与描述元数据，增强导入智能化程度。               |
| gunicorn          | 21.2.0          | 生产环境 WSGI 服务器，开发阶段可使用内置 runserver 替代。            |
| nodejs            | 18.x+           | 前端静态资源构建工具链依赖，仅当需要重新编译 CSS 时安装。            |

## 文档导航

| 层面           | 目录/文件路径                | 回答的问题                                                         |
|----------------|------------------------------|--------------------------------------------------------------------|
| 用户手册       | `docs/user-guide/quick-start.md` | 如何首次启动、导入个人书签、配置目录显示偏好。                     |
| 管理员指南     | `docs/admin/directory-management.md` | 如何创建分类、批量编辑资源元数据、设置访问权限及清理失效链接。     |
| API 参考       | `docs/api/endpoints.md`       | 如何通过 REST API 获取分类列表、资源详情及统计信息，支持哪些过滤参数。 |
| 部署运维       | `docs/deployment/production-setup.md` | 如何配置 PostgreSQL、Redis、Gunicorn 与 Nginx 实现高可用生产部署。 |
| 贡献者文档     | `CONTRIBUTING.md`             | 代码风格规范、提交信息格式、测试用例编写要求与 PR 提交流程。       |
| 架构设计       | `docs/architecture/data-model.md` | 资源、标签、分类、用户四张核心表的关系模型与字段约束设计说明。     |

## 资源列表

### 实时比分与数据参考类

- <code>ouguanbifen.org.cn</code>
- <code>jishibifenxueyuanyuangw.org.cn</code>
- <code>zuqiubifenhupuzuqiu.org.cn</code>

### 学院数据与竞猜分析类

- <code>xueyuanyuanjinrituijian.asia</code>
- <code>xueyuanyuanbifenzhibo.asia</code>
- <code>xueyuanyuanbifen.asia</code>

### 历史数据与前瞻参考类

- <code>rizhilianzhugongbang.asia</code>
- <code>rizhilianqianzhan.asia</code>

## 项目结构

```
navigator-core/
├── manage.py                      # Django 项目入口脚本，集成迁移、运行与测试命令
├── requirements.txt               # Python 依赖清单，锁定所有第三方库版本
├── .env.example                   # 环境变量模板，包含数据库连接串与密钥占位
├── core/                          # 项目配置根目录
│   ├── settings.py               # 主配置文件，含 INSTALLED_APPS、中间件与静态路径
│   ├── urls.py                   # 根路由分发，映射 API 与后台管理入口
│   └── wsgi.py                   # 生产级 WSGI 应用加载点
├── apps/                          # 功能模块容器目录
│   ├── directory/                # 目录管理应用，处理资源增删改查与分类树
│   │   ├── models.py             # 定义 Resource、Category、Tag 实体
│   │   ├── views.py              # 实现列表展示、详情页与搜索视图
│   │   └── utils.py              # 包含 URL 解析、元数据提取工具函数
│   ├── probe/                    # 可用性探测任务模块，包含定时调度逻辑
│   │   ├── checker.py            # 并发 HTTP 检测器，记录响应状态与延迟
│   │   └── tasks.py              # Celery 或 cron 风格的周期性任务定义
│   └── accounts/                 # 用户认证与权限控制模块
│       ├── models.py             # 扩展 Django User 模型，增加角色字段
│       └── permissions.py        # 基于角色的权限校验装饰器与 mixin
├── static/                        # 前端静态资源目录（CSS、JS、图片）
│   ├── css/
│   └── js/
├── templates/                     # Django 模板文件目录
│   ├── base.html                 # 全局基础布局模板，包含导航栏与页脚
│   └── directory/                # 目录相关页面模板
├── fixtures/                      # 种子数据加载目录
│   └── seed_links.json           # 初始资源条目及分类关系示例数据
├── tests/                         # 单元测试与集成测试用例目录
│   ├── test_models.py
│   ├── test_api.py
│   └── test_probe.py
└── docs/                          # 完整文档源文件（用户手册与运维指南）
    ├── user-guide/
    ├── admin/
    ├── api/
    └── deployment/
```

## 贡献指南

1. 派生主仓库至个人账户，并克隆派生副本到本地开发环境。创建新分支时请遵循 `feature/功能简述` 或 `fix/问题简述` 的命名规则。

2. 安装开发依赖组 `pip install -r requirements-dev.txt`，该文件包含代码格式化工具 black、静态检查 flake8 及测试框架 pytest。运行 `pre-commit install` 启用提交前自动格式化。

3. 编写或修改代码后，需补充对应的单元测试用例，确保测试覆盖度不低于 80%。所有测试通过 `pytest tests/` 执行，且不应产生警告信息。

4. 提交变更时，遵循 Conventional Commits 规范（如 `feat: 添加批量导入接口`、`fix: 修复标签检索大小写敏感问题`）。提交信息必须包含变更说明及关联 issue 编号（若有）。

5. 发起 Pull Request 至主仓库的 `main` 分支，描述中需简要说明变更目的、影响范围及测试验证结果。至少一位项目维护者审阅通过后合并。

## 常见问题

**Q：如何将现有浏览器书签（如 Chrome 导出的 HTML）批量导入 OpsLink Navigator？**

A：系统未直接支持 HTML 书签解析，但提供 JSON 与 CSV 模板导入。用户可先通过浏览器书签管理器将收藏夹导出为 CSV 格式（或使用第三方转换工具），然后对照 `fixtures/seed_links.json` 的结构将标题、URL、分类名称映射至导入模板。管理员界面中提供“批量导入”入口，上传符合格式的 JSON 文件即可自动创建资源条目。若分类不存在，系统将自动创建新分类。

**Q：可用性探测任务产生误报（如目标站防爬或临时维护），应如何调整策略？**

A：检测器默认配置为 5 秒超时、仅关注 HTTP 200/301/302 响应码，并忽略非 2xx 状态。若目标站点存在反爬机制，可在 `apps/probe/checker.py` 中自定义 `headers` 参数（如增加 User-Agent）。同时支持通过环境变量 `PROBE_TIMEOUT` 与 `PROBE_RETRY` 调整超时与重试次数。对于已知的临时维护窗口，管理员可在后台对特定资源条目单独设置“暂停检测”标记，避免告警噪音。

**Q：生产环境中如何从 SQLite 迁移至 PostgreSQL，现有数据是否会丢失？**

A：项目内置 Django 的数据库迁移机制，迁移步骤为：安装 `psycopg2-binary` 依赖，在 `settings.py` 中修改 `DATABASES` 配置指向 PostgreSQL 连接串，然后执行 `python manage.py dumpdata > backup.json` 导出 SQLite 数据，更换配置后运行 `python manage.py migrate` 初始化 PostgreSQL 表结构，最后执行 `python manage.py loaddata backup.json` 恢复数据。建议在迁移前进行完整备份，并在低峰期操作。

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:42:42
