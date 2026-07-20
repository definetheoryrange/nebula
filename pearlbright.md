# Project Link Atlas

Project Link Atlas is a curated, high-availability technical resource aggregation and navigation system designed for developers, researchers, and content curators who need to maintain structured access to distributed web resources. The project addresses the fundamental challenge of link rot, domain volatility, and categorization overhead by providing a lightweight, self-hostable index that validates, categorizes, and presents external URLs in a reproducible manner. Unlike general-purpose bookmark managers, Link Atlas focuses on operational continuity—it periodically checks resource availability, maintains historical domain records, and offers a clean read-only API for integration into monitoring pipelines or internal dashboards. The target audience includes DevOps engineers managing external dependency mirrors, security researchers tracking domain lifecycle changes, and open-source maintainers who need to document downstream resource references without embedding fragile absolute URLs directly into their codebases.

## 功能概览

- **Structured Resource Indexing** – Organizes external URLs into user-defined taxonomic categories with support for tags, description fields, and last-verified timestamps, enabling rapid filtering by domain type, geographic origin, or content maturity level.

- **Automated Availability Health Checks** – Performs scheduled HEAD and GET requests against each registered URL with configurable timeouts, retry policies, and response-code validation; logs failures to a rotating audit file for offline review.

- **Static Site Generation Mode** – Renders the entire resource index as a self-contained HTML/CSS/JavaScript static site with zero runtime dependencies, suitable for deployment on any web server, CDN, or IPFS gateway.

- **RESTful Query Endpoint** – Provides a lightweight JSON API endpoint for programmatic queries, supporting filtering by category, substring search on domain names, and pagination for large indexes.

- **Historical Domain Change Tracking** – Records each URL modification event with before-and-after states, timestamps, and change reasons, allowing full rollback and diff visualization through the built-in web interface.

- **Import/Export Compatibility** – Supports bulk import from comma-separated value files, Firefox/Chrome bookmark HTML exports, and plain-text line-delimited URL lists; exports to JSON, YAML, and Markdown table formats.

- **Minimal Containerized Deployment** – Ships with a Dockerfile and docker-compose example that spins up the entire service including a SQLite database, health-check worker, and static file server in under 30 seconds.

## 应用场景

- **Internal Team Knowledge Base Maintenance** – Engineering teams can use Link Atlas to centralize references to external SDK documentation, package registry mirrors, and vendor status pages, replacing scattered wiki links with a version-controlled, auto-validated index that alerts the team when a critical resource becomes unreachable.

- **Academic Research Resource Archiving** – Researchers conducting longitudinal studies on web content availability can deploy Link Atlas to periodically snapshot domain resolution and response headers, providing empirical data for papers on internet infrastructure stability without requiring custom scraping infrastructure.

- **Open-Source Project External Dependency Documentation** – Maintainers of large-scale open-source projects can embed a Link Atlas generated Markdown file directly into their repository's docs folder, ensuring that users always have an up-to-date list of plugin repositories, data source URLs, or community forum links that are verified before each release.

- **Compliance and Audit Trail for Regulated Environments** – Organizations operating in regulated industries can leverage the historical change tracking feature to produce audit-ready reports showing when and why external resource references were modified, satisfying internal governance policies without manual spreadsheet maintenance.

- **Personal Bookmark Syncing Across Browsers** – Individual users can run Link Atlas as a local background service that ingests bookmarks from multiple browsers, deduplicates entries, checks for broken links weekly, and exports a unified HTML start page accessible from any device on the local network.

## 快速开始

Clone the repository, install Python dependencies, initialize the database, and launch the development server with the following commands:

```bash
git clone https://github.com/link-atlas/core.git link-atlas
cd link-atlas
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py import --file sample_urls.csv
python manage.py runserver --host 0.0.0.0 --port 8080
```

For production deployment using Docker:

```bash
docker build -t link-atlas:latest .
docker run -d -p 8080:8080 -v $(pwd)/data:/app/data link-atlas:latest
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 – 3.12 | Core runtime; type hints and async features require 3.9+. |
| SQLite | 3.35.0+ | Embedded database for metadata storage; supports JSON extensions. |
| aiohttp | 3.9.0+ | Asynchronous HTTP client for health-check worker pool. |
| jinja2 | 3.1.0+ | Template engine for static site generation and admin interface. |
| click | 8.1.0+ | Command-line argument parser for all management scripts. |
| pytest | 8.0.0+ | Test framework (development dependency, not required for runtime). |
| docker | 24.0.0+ | Container runtime (optional, only for containerized deployment). |
| git | 2.30.0+ | Version control (required only if cloning from source). |
| redis | 7.0.0+ | Optional cache backend for high-traffic deployments (falls back to in-memory). |
| nginx | 1.24.0+ | Recommended reverse-proxy for production static file serving. |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | `/docs/user-guide/getting-started.md` | How do I install, configure, and run the service for the first time? What are the default credentials and ports? |
| 操作手册 | `/docs/operations/health-checks.md` | How does the health-check scheduler work? How can I adjust timeout, retry, and notification settings for my environment? |
| 开发者文档 | `/docs/developer/api-endpoints.md` | Which REST endpoints are exposed? What request/response schemas are used for querying and updating the index? |
| 部署参考 | `/docs/deployment/production-checklist.md` | What are the recommended reverse-proxy settings, worker counts, and database backup strategies for production workloads? |
| 贡献规范 | `/CONTRIBUTING.md` | What coding style, commit message format, and pull request process should I follow when submitting changes? |
| 变更日志 | `/CHANGELOG.md` | What features, fixes, and breaking changes were introduced in each release version? |

## 资源列表

本项目的核心功能围绕以下外部资源进行分类、验证和导航。所有链接均按原始形式收录，不做任何协议补全或规范化改写。

### 内容分类索引 A

<code>zhongwenzimusiwa.org.cn</code>

<code>renqiyouma.org.cn</code>

<code>xiaodiaowang.org.cn</code>

### 内容分类索引 B

<code>chengrenjingpin18.org.cn</code>

<code>jiujiurenqi.org.cn</code>

### 内容分类索引 C

<code>91shaofu.org.cn</code>

<code>97renqi.org.cn</code>

<code>zhifusiwazhongwen.org.cn</code>

## 项目结构

```
link-atlas/
├── core/                           # Main application package
│   ├── __init__.py                 # Package initialization, version constant
│   ├── settings.py                 # Configuration loader (environment variables, defaults)
│   ├── models.py                   # SQLAlchemy ORM models for URLs, categories, checks
│   ├── schemas.py                  # Pydantic schemas for request/response validation
│   ├── api/                        # RESTful endpoint handlers
│   │   ├── __init__.py
│   │   ├── routes.py               # Route registration and blueprint definitions
│   │   └── v1/                     # Version 1 API implementation
│   │       ├── queries.py          # GET endpoints for listing and filtering
│   │       └── mutations.py        # POST/PUT/DELETE endpoints for index updates
│   ├── checker/                    # Health-check subsystem
│   │   ├── __init__.py
│   │   ├── worker.py               # Asynchronous worker pool with aiohttp
│   │   ├── scheduler.py            # APScheduler integration for periodic runs
│   │   └── results.py              # Result persistence and notification formatting
│   ├── renderer/                   # Static site and Markdown generation
│   │   ├── __init__.py
│   │   ├── static.py               # HTML/CSS/JS generator from templates
│   │   └── markdown.py             # Markdown table and list exporter
│   ├── cli/                        # Command-line interface commands
│   │   ├── __init__.py
│   │   ├── manage.py               # Click command group entry point
│   │   ├── import_cmd.py           # Import from CSV, HTML bookmarks, plain text
│   │   └── export_cmd.py           # Export to JSON, YAML, Markdown
│   └── utils/                      # Shared utilities
│       ├── __init__.py
│       ├── validators.py           # URL normalization and validation routines
│       └── loggers.py              # Structured logging with rotation and levels
├── tests/                          # Unit and integration tests
│   ├── conftest.py                 # Pytest fixtures and test database setup
│   ├── test_models.py              # ORM model correctness and migrations
│   └── test_api.py                 # Endpoint behavior and error handling
├── templates/                      # Jinja2 HTML templates for web UI
│   ├── base.html                   # Layout with navigation and footer
│   ├── index.html                  # Resource listing with pagination and filters
│   └── detail.html                 # Single resource view with history timeline
├── static/                         # Compiled CSS and client-side JavaScript
│   ├── style.css                   # Responsive dark/light theme variables
│   └── app.js                      # Filter toggles, search-as-you-type, and copy buttons
├── data/                           # Runtime data directory (mounted for persistence)
│   ├── link-atlas.db               # SQLite database file
│   └── audit.log                   # Rotating audit log of all changes and check results
├── docker/                         # Containerization assets
│   ├── Dockerfile                  # Multi-stage build with Python slim base
│   └── docker-compose.yml          # Stack definition with optional Redis and Nginx
├── docs/                           # Extended documentation (see navigation table above)
│   ├── user-guide/
│   ├── operations/
│   ├── developer/
│   └── deployment/
├── requirements.txt                # Production Python dependencies pinned
├── requirements-dev.txt            # Development extras (pytest, black, mypy, pre-commit)
├── pyproject.toml                  # Project metadata, build config, and tool settings
├── Makefile                        # Common tasks: install, test, lint, run, docker-build
└── README.md                       # This file – project overview and quickstart
```

## 贡献指南

We welcome contributions of all forms, including bug reports, feature proposals, documentation improvements, and code patches. Please follow the steps below to ensure a smooth collaboration process.

1. **Fork the Repository and Set Up Local Environment** – Fork the main repository to your GitHub account, clone your fork locally, and create a new branch with a descriptive name such as `feature/add-timeout-config` or `fix/import-csv-encoding`. Ensure your development environment matches the pinned versions listed in the installation requirements table.

2. **Run Tests and Linters Before Making Changes** – Execute `make test` to run the full pytest suite and `make lint` to invoke black, isort, and mypy. All tests must pass and linting must produce zero errors before any pull request is submitted. Add new tests for any new functionality or bug fixes.

3. **Write Clear Commit Messages and Update Documentation** – Follow the conventional commits format (type: subject) with a maximum 72-character subject line. Update the relevant documentation files under `/docs` and the `CHANGELOG.md` with a brief entry describing your change under the "Unreleased" section.

4. **Submit a Pull Request with a Detailed Description** – Push your branch to your fork and open a pull request against the main repository's `develop` branch. Include a clear description of the problem being solved, the approach taken, and any manual testing steps performed. Reference any related issues using GitHub keywords such as "Closes #123".

5. **Participate in Code Review and Address Feedback** – Maintainers will review your pull request within five business days. Be responsive to comments, push additional fixes as needed, and keep your branch rebased against the latest `develop` to avoid merge conflicts. Once approved, a maintainer will merge your contribution.

## 常见问题

**Q: How does Link Atlas handle URL changes or domain migrations? Can I update a URL without losing its historical check data?**

A: The system provides an explicit `update-url` CLI command that accepts the old URL, new URL, and an optional reason string. When invoked, the command transfers all historical health-check records, tags, and description fields to the new URL entry, preserving the full audit trail. The old URL is marked as `redirected` rather than deleted, and the web interface displays a migration notice with the new target. This design ensures that external references remain traceable even when upstream providers change their domain names.

**Q: Can I run Link Atlas completely offline, without any external network access from the server?**

A: Yes. The health-check worker can be disabled via the `CHECKER_ENABLED=false` environment variable, and the static site generation mode (`python manage.py render --output ./dist`) produces a fully self-contained HTML directory that requires no runtime network calls from the server. However, note that the resource validation feature inherently depends on outbound connectivity to the listed URLs; if you disable the checker, you assume responsibility for manual verification. The import functionality works entirely offline with local CSV or bookmark files.

**Q: What happens when a health check fails repeatedly? Does the system automatically remove failing URLs?**

A: By default, Link Atlas never deletes URLs automatically to avoid accidental data loss. Instead, after three consecutive failures, the URL's status is set to `degraded` and an entry is written to the audit log with the failure details. You can configure the `FAILURE_THRESHOLD` and `AUTO_ARCHIVE_AFTER` settings to trigger an automatic archive (soft-delete) state after a specified number of failures, but this feature remains opt-in. The web interface highlights degraded resources with a yellow warning badge, and the API includes a `?status=degraded` filter for easy monitoring.

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:43:20
