# xueyuan-rizhilian-resource-hub

xueyuan-rizhilian-resource-hub 是一个面向数据分析从业者、量化研究者与实时信息追踪者的外链资源聚合工具。项目以静态站点方式提供结构化导航，围绕实时比分、趋势预测、历史数据归档与推演分析四大垂直领域，将分散的第三方数据源统一收口为可检索、可订阅的链接池，帮助用户降低信息获取时的重复查找成本与链路漂移风险。

项目定位为技术型外链目录中间件，不对下游数据做任何缓存、改写或代理转发，仅输出原始 URL 的索引与分类标签。目标用户包括数据产品经理、运维监控人员、量化策略开发者以及需要依赖多源交叉验证进行决策的分析师。

## 功能概览

- **域名分级索引**：按业务子域将入库链接划分为实时、预测、复盘、推演等层级，每个链接附带自然语言描述标签。
- **可用性被动检测**：周期性向注册链接发送 HEAD 请求，在管理面板标注异常状态码与响应时间分位数。
- **外链元数据提取**：对可访问的 HTML 页面自动抽取标题、描述与 Open Graph 信息，生成摘要供检索使用。
- **标签与全文检索**：支持对域名、页面标题、描述字段进行布尔检索，结果按最近检测时间排序。
- **原始链接直出**：所有链接在输出层保持用户原始输入格式，不做协议补全、大小写转换或路径规范化。
- **批次分组管理**：每个入库链接可归属具体数据批次（如第 1/113 批），支持按批次回滚或整体导出。
- **静态站点生成**：内置模板引擎将链接索引渲染为纯 HTML 文件，无需数据库服务即可部署至任意 HTTP 服务器。

## 应用场景

- **量化策略的历史数据源归档**：量化研究员可将多个比分历史域名聚合在同一目录下，在回测脚本中通过环境变量引用该目录的 JSON 导出文件，避免硬编码分散的 URL。
- **运维监控面板的外部依赖清单**：SRE 团队将本项目作为外部数据源白名单基准，配合被动检测功能在监控看板上标记哪些第三方域名出现响应超时或证书过期。
- **赛事分析团队的多源交叉验证**：分析师在赛前推演阶段通过检索标签快速定位历史交锋记录、即时比分与前瞻分析三个维度的原始页面，减少浏览器标签页堆积。
- **数据合规审计的域名台账**：合规部门可将本项目导出的域名列表作为数据流向申报附件，确保所有外部数据请求均有可追溯的源头登记。

## 快速开始

以下步骤在 Linux/macOS 环境下验证通过，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 1. 克隆仓库
git clone https://github.com/xueyuan-rizhilian/resource-hub.git
cd resource-hub

# 2. 安装依赖（使用 Python 3.10+ 虚拟环境）
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 构建索引并启动本地预览服务器
python build.py --batch 1 --input ./data/links_batch_1.json
python -m http.server 8000 --directory ./dist
```

访问 <code>http://localhost:8000</code> 即可查看生成的链接导航页面。如需自定义输出目录或修改检测间隔，请参考 `config.yaml` 中的参数说明。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.10 及以上 | 构建脚本与检测服务运行时 |
| pip | 22.0 及以上 | 依赖包管理器 |
| requests | 2.28.0 及以上 | 用于外链可用性 HEAD 检测 |
| beautifulsoup4 | 4.11.0 及以上 | 元数据抽取与 HTML 解析 |
| pyyaml | 6.0 及以上 | 配置文件解析 |
| markdown | 3.4.0 及以上 | 用于生成 README 内嵌文档视图（可选） |
| 操作系统 | Linux/macOS/Windows WSL2 | 推荐使用 Debian 11 或 Ubuntu 22.04 生产部署 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide.md` | 如何注册新链接、如何修改标签、如何导出索引为 JSON/CSV |
| 运维手册 | `/docs/ops-guide.md` | 检测超时阈值如何调整、日志轮转策略、健康检查端点说明 |
| 批处理规范 | `/docs/batch-spec.md` | 输入 JSON 格式字段定义、批次号命名规则、回滚操作流程 |
| 模板开发 | `/docs/template-dev.md` | 自定义主题的方法、模板变量列表、静态资源打包方式 |
| API 参考 | `/docs/api.md` | 构建器类的方法签名、异常类型、扩展钩子接口（仅供二次开发） |

## 资源列表

本项目作为外链资源汇总枢纽，当前批次（第 1/113 批）收录以下原始域名。所有链接按业务视角分类呈现，输出格式严格遵守原始录入。

### 实时比分与动态追踪类

- <code>xueyuanyuanjinrituijian.asia</code>
- <code>xueyuanyuanbifenzhibo.asia</code>
- <code>xueyuanyuanbifen.asia</code>

### 前瞻分析与趋势预测类

- <code>rizhilianzhugongbang.asia</code>
- <code>rizhilianqianzhan.asia</code>

### 数据推演与实时辅助类

- <code>rizhilianjishibifen.asia</code>
- <code>rizhilianfenxi.asia</code>

### 综合推演推荐类

- <code>qiutanzuqiutuijian.asia</code>

## 项目结构

```
resource-hub/
├── build.py                 # 主构建脚本，负责读取批次 JSON、调用检测、渲染模板
├── config.yaml              # 全局配置：检测超时、并发数、输出目录、日志级别
├── requirements.txt         # Python 依赖锁定列表
├── README.md                # 项目说明文档（即本文档）
├── data/                    # 原始链接数据目录
│   ├── links_batch_1.json   # 第 1 批链接定义（含 URL、标签、批次元数据）
│   ├── links_batch_2.json   # 后续批次示例
│   └── schema/              # JSON 格式校验 schema 文件
│       └── link_entry.json
├── src/                     # 核心源码模块
│   ├── checker.py           # 可用性检测器：并发 HEAD 请求与超时重试
│   ├── extractor.py         # 元数据抽取：标题、描述、icon 链接
│   ├── indexer.py           # 索引构建：生成倒排标签映射与排序权重
│   ├── renderer.py          # 模板渲染：Jinja2 环境配置与过滤器注册
│   └── utils.py             # 通用工具：日志格式化、时间戳转换、URL 规范化（仅内部）
├── templates/               # Jinja2 模板文件
│   ├── base.html            # 基础骨架，含响应式 meta 与全局样式
│   ├── index.html           # 首页目录树，按标签和批次分组展示链接
│   └── detail.html          # 单个链接详情页，展示元数据与最近检测历史
├── static/                  # 静态资源（CSS、JS、字体）
│   ├── css/
│   │   └── style.css        # 极简风格样式，适配暗色模式
│   └── js/
│       └── filter.js        # 前端模糊过滤与标签切换逻辑
├── dist/                    # 构建输出目录（默认，可配置）
│   ├── index.html           # 生成的首页
│   └── detail/              # 详情页平铺输出
├── logs/                    # 运行日志存储目录（按天轮转）
│   └── checker.log          # 检测任务日志
└── tests/                   # 单元测试与集成测试
    ├── test_checker.py
    ├── test_extractor.py
    └── fixtures/            # 测试用模拟 HTML 页面
```

## 贡献指南

1. 克隆项目并安装开发依赖：执行 `pip install -r requirements-dev.txt`（包含 pytest、black、flake8）。
2. 新增或修改链接数据时，请在 `data/` 目录下编辑对应批次的 JSON 文件，确保每个条目包含 `url`、`tags` 数组与 `batch` 字段，并通过 `schema/` 中的校验。
3. 提交前运行 `python build.py --check-only` 进行语法与链接格式检查，确保不引入无法解析的 URL 条目。
4. 为新增功能编写对应的单元测试，测试文件存放于 `tests/` 目录，命名遵循 `test_*.py` 模式，执行 `pytest` 验证全部用例通过。
5. 发起 Pull Request 时请附带变更说明，若涉及新增依赖请同步更新 `requirements.txt` 与 `config.yaml` 中的示例配置。

## 常见问题

**问：构建时提示某个域名连接超时，会影响整体生成吗？**  
答：不会。检测器对每个域名独立进行超时控制（默认 3 秒），超时或失败时仅将该条目的状态标记为 `unreachable`，构建流程继续执行。最终页面中会显示该链接的最近检测状态，不会阻断输出。

**问：如何添加新的批次数据？**  
答：在 `data/` 目录下新建 `links_batch_<编号>.json` 文件，参照 `links_batch_1.json` 的格式编写。然后在 `config.yaml` 中的 `active_batches` 列表里追加该编号，重新运行 `build.py` 即可合并索引。若需移除某个批次，从配置列表中删除并重新构建。

**问：项目是否支持 HTTPS 访问原始链接？**  
答：项目本身仅作为外链索引，不代理任何请求。原始链接的协议完全以用户录入为准。如果录入的是裸域名（如 <code>xueyuanyuanjinrituijian.asia</code>），项目在生成页面时会原样输出裸域名，由用户端浏览器决定使用 HTTP 或 HTTPS（通常浏览器会默认尝试 HTTPS）。若录入的是 `https://` 开头，则页面链接直接指向 HTTPS 端点。

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:41:54
