# WebTech Navigator

WebTech Navigator 是一个面向技术决策者、架构师及独立开发者的高质量外链与资源导航系统。本项目不直接提供代码库或工具，而是通过人工筛选与社区投票机制，持续整理并呈现互联网上最具价值的技术文档、在线工具、数据看板及行业分析站点。其核心目标在于降低技术信息获取的噪声，帮助用户快速定位到与自身项目阶段、技术栈及业务场景相匹配的权威资源。

本项目适用于以下人群：正在调研技术选型的团队负责人、需要追踪实时赛事数据与积分排名的体育科技开发者、以及希望从特定领域（如足球数据分析、区域性赛事进程）获取结构化外链信息的工程师与产品经理。WebTech Navigator 本身不存储任何数据，所有展示信息均通过外链跳转至原始发布页面，确保内容的实时性与原始版权归属。

## 功能概览

- **精选外链聚合**：每一条收录链接均经过初始人工审核与社区标注，剔除失效或低质量站点，确保资源池的基础可用性。

- **分类标签体系**：支持按领域（如实时比分、积分榜、赛程管理、区域性联赛）进行多维度筛选，便于用户按上下文快速收敛关注范围。

- **社区投票与活跃度指示**：集成简单的点赞/点踩机制，展示每个外链的近7日访问热度与社区评分，辅助判断资源当前的实际价值。

- **RSS 订阅与变更监控**：为每个分类提供专属 RSS 输出，用户可订阅特定类别的更新，无需频繁手动访问页面即可获知新增或失效资源。

- **黑暗模式与阅读视图**：前端界面提供无障碍阅读选项，适配技术团队在夜间或低光环境下的长时间浏览习惯。

- **开放 API 接口**：提供只读的 JSON/RESTful API，允许第三方开发者将资源列表集成至自己的文档站、内部工具或 CLI 脚本中。

- **失效链接上报机制**：内置一键报告失效链接的功能，并辅以自动化的存活检测任务（每日执行），持续维护列表质量。

## 应用场景

- **技术选型调研期的资源聚合**：团队在决定使用某类中间件或数据可视化方案时，可通过本导航站快速定位到官方文档、社区实践案例及性能对比分析的原始链接，节省在通用搜索引擎中筛选的时间成本。

- **体育数据产品的竞品分析**：从事足球积分、实时比分或区域性赛事（如亚洲杯预选赛、地方性联赛）数据服务的开发者，可通过本导航站集中获取多个不同维度的数据源站点，便于进行数据字段对比与更新频率评估。

- **架构师月度技术简报素材收集**：架构师需要定期整理行业动态与技术趋势，本导航站中的外链分类（如积分榜、赛事进程、数据榜单）可作为固定的素材来源，结合 RSS 订阅实现低成本的持续跟踪。

- **开源项目 README 的“相关资源”附录生成**：独立开发者在编写自身项目的 README 或文档时，可参考本导航站的分类与链接格式，快速生成结构化的“相关生态”或“数据来源”章节。

## 快速开始

以下步骤指导您在本地环境快速运行 WebTech Navigator 的开发版本。本项目采用轻量级静态站点生成方案，依赖 Node.js 与 npm。

```bash
# 1. 克隆仓库至本地
git clone https://github.com/webtech-navigator/navigator.git
cd navigator

# 2. 安装项目依赖（包括构建工具与开发服务器）
npm install

# 3. 启动本地开发服务器，默认监听端口 3000
npm run dev
```

执行上述命令后，打开浏览器访问 `http://localhost:3000` 即可查看导航站首页。生产环境构建请使用 `npm run build`，产物默认输出至 `dist` 目录。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | >= 18.0.0 | 运行时环境，用于执行构建脚本与开发服务器 |
| npm | >= 9.0.0 | 包管理器，用于安装依赖及执行脚本命令 |
| Git | >= 2.30.0 | 版本控制工具，用于克隆仓库及后续更新 |
| 现代浏览器 | 最新两个主要版本 | 开发调试与最终访问界面，推荐 Chrome/Firefox/Edge |
| 网络连接 | 稳定宽带 | 首次启动需下载依赖包，且运行中需访问外链资源进行存活检测 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何使用分类筛选、RSS 订阅、上报失效链接；如何解读社区评分与热度指标 |
| 开发者指南 | `/docs/developer/` | API 接口的认证方式、请求限流策略、返回字段释义；如何为导航站添加新的外链分类 |
| 部署运维 | `/docs/ops/` | 生产环境静态资源部署建议（CDN、对象存储）；存活检测服务的配置参数与日志查看 |
| 社区治理 | `/docs/community/` | 外链收录的评审标准；社区投票权重算法；争议链接的处理流程与申诉渠道 |

## 资源列表

### 足球赛事与数据平台

<code>zuqiudsshoujiban.net.cn</code>

<code>ajiasaicheng.asia</code>

<code>baxizuqiujiajiliansai.asia</code>

<code>bijiasaicheng.asia</code>

### 积分榜与实时数据

<code>hanklianjifenbang.asia</code>

<code>jishibifenqiutan.asia</code>

<code>puchaojifenbang.asia</code>

<code>puchaozhugongbang.asia</code>

## 项目结构

```
navigator/
├── public/                         # 静态资源目录，不经过构建工具处理
│   ├── favicon.ico                 # 站点图标
│   └── robots.txt                  # 搜索引擎爬虫规则，允许全站索引
├── src/
│   ├── assets/                     # 样式、图片、字体等前端资源
│   │   ├── css/                    # 全局基础样式与主题变量（亮色/暗色）
│   │   └── images/                 # 分类图标与默认占位图
│   ├── components/                 # 可复用的 UI 组件（卡片、筛选栏、投票按钮）
│   │   ├── LinkCard/               # 单个外链展示卡片，包含标题、描述、标签、操作按钮
│   │   ├── FilterBar/              # 多级分类筛选组件，支持标签多选
│   │   └── ReportModal/            # 失效链接上报弹窗，包含验证码与提交逻辑
│   ├── data/                       # 外链数据的本地存储（JSON 格式），作为静态源
│   │   ├── categories.json         # 分类层级定义与显示名称映射
│   │   └── links.json              # 所有外链的完整列表（含 URL、标题、描述、标签、添加时间）
│   ├── layouts/                    # 页面布局模板（含头部导航、侧边栏、底部版权）
│   ├── pages/                      # 各路由对应的页面组件
│   │   ├── index.jsx               # 首页，展示热门链接与分类快速入口
│   │   ├── category.jsx            # 分类详情页，展示该分类下的所有链接及排序选项
│   │   └── about.jsx               # 项目介绍、收录标准、团队信息
│   ├── services/                   # 与外部 API 交互的服务层（存活检测、投票同步）
│   │   ├── healthCheck.js          # 定时批量检测链接可达性的核心逻辑
│   │   └── voteService.js          # 社区投票数据的读写接口封装
│   ├── utils/                      # 通用工具函数（日期格式化、URL 标准化、去重）
│   └── app.jsx                     # 应用入口，配置路由与全局状态提供者
├── tests/                          # 单元测试与集成测试用例
│   ├── components/                 # 组件级别的渲染与交互测试
│   └── services/                   # 服务层函数的模拟与边界条件测试
├── .env.example                    # 环境变量模板（包含 API 基础地址、检测间隔配置）
├── package.json                    # 项目依赖清单与脚本命令定义
├── vite.config.js                  # 构建工具 Vite 的配置文件（含代理与别名）
└── README.md                       # 项目总体说明文档（即本文档）
```

## 贡献指南

我们欢迎社区成员以多种方式参与本项目，包括但不限于推荐新链接、报告失效链接、完善文档或改进界面交互。请遵循以下步骤：

1. **查阅现有资源列表与分类**：在提交新链接推荐前，请先通过首页或分类页确认该链接尚未被收录，且内容定位与现有分类体系匹配。若建议新增分类，需在 issue 中附上至少 3 个同类别的高质量链接作为依据。

2. **通过 GitHub Issue 提交提议**：所有链接增删改操作均需通过 GitHub Issue 发起，标题请使用 `[Link Request]` 或 `[Broken Report]` 前缀，并在正文中按模板填写链接 URL、推荐理由或失效证据（如 HTTP 状态码截图）。

3. **分支开发与拉取请求**：若您希望直接修改代码或数据文件，请从 `main` 分支拉出 `feature/xxx` 或 `fix/xxx` 类型的分支进行开发。完成本地测试后，提交 Pull Request 并关联对应的 Issue。PR 需至少通过单元测试与构建检查。

4. **文档与注释同步更新**：任何涉及数据结构变更（如 `links.json` 字段调整）或新增环境变量的修改，必须同步更新本 README 及 `/docs` 下的相关手册，确保文档与代码处于一致状态。

5. **遵守行为准则**：参与本项目即视为同意遵守《贡献者公约》第 2.0 版。请保持友好、专业且尊重他人的沟通方式，禁止提交低质量、重复或广告性质的链接。

## 常见问题

**问：导航站中的外链出现无法访问或内容变更，我应该如何处理？**

答：您可以通过每个链接卡片右下角的“报告失效”按钮提交反馈，系统会自动记录您的上报信息并触发一次即时存活检测。若检测确认失效，该链接将被移入待复核队列，并在 48 小时内由维护团队人工审核。在审核期间，该链接仍会显示但会被标记为“疑似失效”状态。

**问：我能否将本导航站的分类或链接列表用于我自己的项目中？**

答：可以。本项目的核心外链数据（即 `src/data/links.json` 中的 URL 与标签信息）采用公共领域贡献声明（CC0 1.0）发布，您可将其用于任何商业或非商业目的，无需注明出处。但请注意，各外链指向的原始站点受其自身版权与使用条款约束，与本项目无关。同时，本项目的界面代码（React 组件、样式等）仍遵循 MIT 许可证。

**问：导航站多久更新一次外链列表？新增链接的审核周期是多久？**

答：系统每日凌晨 2:00（UTC+8）会执行一次全量存活检测，并自动标记失活链接。新增链接的人工审核通常在每周二和周四进行，平均审核周期为 3 个工作日。紧急情况下（如链接指向的安全公告或关键补丁），可通过项目维护组邮箱 `ops@webtech-nav.example` 申请快速通道。

## 许可证

MIT License

Copyright (c) 2026 WebTech Navigator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 8 | 生成时间: 2026-07-20 16:41:54
