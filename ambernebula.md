# ResourceForge

ResourceForge 是一个专注于互联网技术资源与优质外链聚合的开源导航工具。项目面向开发者、技术研究人员以及信息架构师，旨在解决海量技术站点分散、检索成本高、可信来源难以甄别的问题。通过人工筛选与社区投票机制，ResourceForge 构建了结构化的技术资源索引，帮助用户快速定位高价值外部信息，降低知识获取的摩擦成本。

项目本身不存储或托管任何外部内容，仅提供元数据与链接跳转服务。其核心引擎支持动态分类、关键词过滤与可用性监控，可作为个人或团队内部的知识门户基础组件。ResourceForge 遵循开放数据原则，所有资源条目以 JSON 格式存储，便于二次开发与数据迁移。

## 功能概览

- **多维度资源分类**：支持按技术领域、应用场景、资源形态（文档、工具、社区、视频）进行交叉筛选，提供超过 20 个预定义标签体系。
- **外链健康度监控**：后台定时任务每日检测收录链接的 HTTP 状态码，自动标记失效或重定向条目，确保资源可达性。
- **社区投票与评分**：注册用户可对资源进行投票与短评，系统依据综合评分动态调整资源排序，优质站点自然上浮。
- **自定义数据源导入**：支持通过 CSV 或 JSON 格式批量导入私有链接库，并合并至本地索引，适合企业内部知识库建设。
- **RESTful API 接口**：提供完整的只读 API，支持按分类、关键词、标签查询资源列表，便于与其他系统集成。
- **暗色主题与响应式布局**：前端适配桌面与移动终端，提供舒适的长文阅读与浏览体验。
- **资源变更历史记录**：每次编辑或状态变更均写入操作日志，支持版本回退与变更审计。

## 应用场景

- **技术团队内部知识导航**：研发团队可将常用开发文档、CI/CD 工具、日志平台、监控面板等链接统一纳入 ResourceForge，替代浏览器混乱的书签栏，新成员入职时一键获取全部基础设施入口。
- **开源项目 README 外链托管**：开源维护者可将项目依赖的参考文献、数据源、姊妹项目等链接整理为 ResourceForge 数据包，通过嵌入 iframe 或调用 API 的方式集成到项目主页，避免 README 过度臃肿。
- **技术博主资源清单管理**：内容创作者在撰写系列教程时，可将每篇文章涉及的参考资料、工具站点、在线编译器链接集中存于 ResourceForge，并为每篇教程生成独立的分类视图，方便读者按图索骥。
- **高校实验室研究资料聚合**：科研人员可围绕特定课题（如自然语言处理、计算机视觉）建立专属资源库，收录论文预印本网站、开源代码仓库、数据集发布页等，支持多人协同维护。
- **个人学习路线知识库**：自学者可按阶段（入门、进阶、实战）整理在线课程、互动练习、技术社区问答精华帖，构建个人成长的知识地图，并随时调整分类权重。

## 快速开始

以下步骤将引导您在本地环境快速启动 ResourceForge 开发实例。

```bash
# 1. 克隆项目仓库
git clone https://github.com/resourceforge/resourceforge.git
cd resourceforge

# 2. 安装依赖（使用 npm）
npm install

# 3. 复制环境变量模板并填充本地配置
cp .env.example .env

# 4. 初始化本地 SQLite 数据库并导入预置资源条目
npm run db:init

# 5. 启动开发服务器（默认监听端口 3000）
npm run dev
```

访问 `http://localhost:3000` 即可进入本地实例。管理员默认账号为 `admin@resourceforge.local`，密码在首次启动时打印于终端日志中。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | >= 18.17.0 | 运行时环境，推荐使用 LTS 版本 |
| npm | >= 9.6.0 | 包管理器，用于安装依赖与执行脚本 |
| SQLite3 | >= 3.38.0 | 嵌入式数据库，无需额外部署服务 |
| Git | >= 2.30.0 | 版本控制工具，用于克隆仓库与提交贡献 |
| 操作系统 | Linux / macOS / Windows (WSL2) | 支持主流开发环境，Windows 下建议使用 WSL2 以获得最佳性能 |
| 内存 | 建议 >= 512MB | 运行时内存占用约 200MB，索引重建期间峰值约 400MB |
| 存储 | 建议 >= 1GB | 数据库与日志文件长期存储空间，资源条目数每增加 1 万条约占用 50MB |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | `docs/getting-started/` | 如何安装、配置、首次启动与登录；管理员初始化流程；默认数据说明。 |
| 数据管理 | `docs/data-management/` | 如何添加/编辑/删除资源链接；分类标签的增删改；批量导入导出的数据格式规范。 |
| API 参考 | `docs/api-reference/` | 所有 RESTful 端点的请求方法、参数说明、响应示例；认证方式与速率限制策略。 |
| 运维手册 | `docs/operations/` | 生产环境部署建议（Nginx 反向代理、PM2 守护）；日志轮转方案；数据库备份与恢复命令。 |

## 资源列表

### 综合信息类
- <code>jiujiurenqi.org.cn</code>
- <code>97renqi.org.cn</code>

### 专项内容类
- <code>91shaofu.org.cn</code>
- <code>zhifusiwazhongwen.org.cn</code>
- <code>jiujiulunli.org.cn</code>

### 多媒体与素材类
- <code>zhongwenzimuzhifusiwa.org.cn</code>
- <code>shoujifulishipin.org.cn</code>
- <code>zhongwenzimumeinv.org.cn</code>

## 项目结构

```
resourceforge/
├── src/                           # 核心源代码目录
│   ├── api/                       # RESTful API 路由层（控制器与中间件）
│   ├── core/                      # 核心业务引擎（分类引擎、评分计算、健康检查）
│   ├── db/                        # 数据库连接池与 SQL 构建器（支持 SQLite/PostgreSQL 适配）
│   ├── models/                    # 数据模型定义（Resource、Category、User、Vote、HistoryLog）
│   ├── services/                  # 外部服务集成（邮件通知、站点点位监控）
│   └── utils/                     # 通用工具函数（URL 规范化、日期格式化、校验器）
├── public/                        # 前端静态资源根目录
│   ├── assets/                    # 图片、字体、样式源文件（SCSS）
│   ├── scripts/                   # 前端 JavaScript 模块（分类树渲染、表格排序、深色主题切换）
│   └── index.html                 # 主页面模板
├── tests/                         # 单元测试与集成测试（基于 Jest + Supertest）
│   ├── unit/                      # 模型层与工具函数独立测试
│   └── integration/               # API 端到端测试与数据库事务回滚测试
├── docs/                          # 完整文档源文件（Markdown 格式，含插图与架构图）
├── scripts/                       # 运维与工具脚本（数据库初始化、种子数据生成、备份压缩）
├── config/                        # 环境配置目录（包含不同环境的配置文件模板）
├── logs/                          # 应用运行日志存储目录（自动按日期切割）
├── .env.example                   # 环境变量参考文件（含全部可配置项注释）
├── package.json                   # npm 项目清单（依赖声明与脚本入口）
├── README.md                      # 项目说明文件（本文件）
└── LICENSE                        # MIT 许可证全文
```

## 贡献指南

1.  **问题讨论**：在提交任何代码变更之前，请先在 Issues 区域打开一个讨论议题，说明您希望修复的问题或新增的功能。核心维护者会在 48 小时内反馈可行性方向。

2.  **复刻与分支**：将本项目复刻至您的个人账户，并基于 `develop` 分支创建新的特性分支（命名格式：`feature/描述` 或 `fix/描述`）。请确保分支名称简洁且语义明确。

3.  **编码规范**：遵循项目内置的 ESLint 与 Prettier 配置。提交前请运行 `npm run lint` 与 `npm run test` 确保代码风格一致且所有现有测试用例通过。新增功能必须附带对应的单元测试或集成测试。

4.  **提交消息**：提交信息请使用约定式提交格式（Conventional Commits），例如 `feat: 增加按标签过滤 API` 或 `fix: 修复健康检查超时导致的空指针异常`。提交消息需使用英文。

5.  **发起请求**：将您的特性分支推送至复刻仓库，然后向本项目的 `develop` 分支发起 Pull Request。PR 描述中请关联对应的 Issue 编号，并简要说明实现思路与测试覆盖情况。核心维护者将进行代码审查，并在通过 CI 流水线后合并。

## 常见问题

**问：ResourceForge 本身会缓存或存储外部站点的内容吗？**

答：不会。ResourceForge 只存储 URL 地址、标题、分类标签和简短描述等元数据。所有外部内容均通过用户浏览器直接请求原始站点，项目服务器不担任代理或缓存角色。健康检查功能仅发送 HTTP HEAD 请求验证连通性，不下载响应体。

**问：如何将现有的浏览器书签（HTML 导出格式）批量导入到 ResourceForge？**

答：项目提供了一个社区贡献的转换脚本 `scripts/import-bookmarks.js`，支持解析 Chrome / Firefox 导出的书签 HTML 文件。您可以将文件放置于 `./data/` 目录下，执行 `npm run import:bookmarks -- --file=bookmarks.html --category=默认分类`，系统将自动提取链接与文件夹层级关系并生成对应的资源条目。

**问：生产部署时推荐使用 SQLite 还是 PostgreSQL？**

答：对于资源条目数在 2 万条以内、日均请求低于 5000 的场景，SQLite 完全足够且运维成本极低。若条目数超过 5 万条或需要多实例读写分离，则建议切换到 PostgreSQL。您只需修改 `.env` 文件中的 `DB_TYPE` 和连接字符串即可切换，无需更改业务代码。

## 许可证

MIT License

Copyright (c) 2026 ResourceForge Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 8 | 生成时间: 2026-07-20 16:43:25
