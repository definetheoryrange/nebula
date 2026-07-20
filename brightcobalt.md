# Leisu Sports Data Aggregator

Leisu Sports Data Aggregator is a community-driven, open-source information aggregation platform designed for sports data analysts, betting researchers, and sports enthusiasts who require structured access to a curated network of sports analytics resources. The project solves the problem of fragmented, hard-to-discover, and poorly documented sports statistics and prediction sources by providing a central, maintainable index of high-quality external data endpoints, along with a lightweight local toolkit for fetching, normalizing, and displaying data from these sources in a consistent format.

Target users include data scientists building predictive models for match outcomes, journalism students researching sports performance trends, and hobbyists who want to track multiple leagues without manually visiting dozens of websites. The project is not a data provider itself; rather, it is a gateway and a set of utilities that make external data more accessible and easier to integrate into custom workflows.

## 功能概览

- **Unified Resource Indexing** – Maintains a version-controlled, machine-readable catalog of sports data URLs, each tagged by sport type, region, and data category (e.g., odds, live scores, historical results).

- **Automated Health Checks** – Periodically tests each external endpoint for availability and response time, logging failures and generating a daily status report for operators.

- **Normalized Data Schema** – Provides a set of Python dataclasses and JSON schemas that map disparate external data formats into a unified structure for scores, fixtures, and team statistics.

- **CLI Query Tool** – Offers a command-line interface to search the resource index by league name, date range, or data type, returning the most relevant external links with contextual metadata.

- **Webhook Alert System** – Allows users to configure custom webhooks (e.g., Discord, Slack, or generic HTTP endpoints) that receive notifications when a monitored source changes its API structure or goes offline.

- **Dockerized Deployment** – Ships with a multi-arch Docker image and docker-compose stack for one-command deployment, including a PostgreSQL instance for caching and a Prometheus exporter for metrics.

- **Extensible Plugin System** – Supports user-contributed parsers and transformers via a drop-in `plugins/` directory, enabling the community to quickly adapt to new data sources without forking the core.

- **Local Cache Layer** – Implements a configurable Redis-backed cache with TTL policies to reduce redundant network requests and improve response times for frequently accessed endpoints.

## 应用场景

- **Sports Betting Research** – Analysts can use the aggregator to compile a daily digest of odds movements from multiple bookmaker-affiliated sources, reducing the time spent on manual tabulation and allowing more focus on statistical modeling.

- **Academic Performance Analysis** – University researchers studying team performance over multiple seasons can leverage the normalized schema to ingest historical match data from several endpoints simultaneously, ensuring consistent field names and data types for their regression models.

- **Hobbyist Dashboard Builders** – Individuals building personal dashboards with tools like Grafana or Streamlit can use the CLI query tool to fetch a curated list of live score URLs, embedding them directly into iframe panels or using the raw JSON output for custom visualizations.

- **DevOps Monitoring for Sports APIs** – Site reliability engineers responsible for internal sports data pipelines can integrate the health check system into their existing monitoring stack (e.g., Prometheus + Alertmanager) to receive early warnings about external dependency degradation.

- **Educational Workshops on Web Scraping** – Instructors teaching ethical web scraping and data ethics can use the project’s plugin system as a safe, sandboxed environment to demonstrate parser design, rate limiting, and robots.txt compliance, without requiring students to discover their own endpoints.

## 快速开始

Prerequisites: Git, Python 3.10+, and Docker (optional).

```bash
# Clone the repository
git clone https://github.com/leisu-dev/leisu-aggregator.git
cd leisu-aggregator

# Create a virtual environment and install dependencies
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt

# Copy the example environment file and adjust settings
cp .env.example .env

# Initialize the local SQLite database and load the default resource index
python manage.py migrate
python manage.py load_index --source resources/default_index.yaml

# Run the health check once to verify connectivity
python manage.py health_check --all

# Start the development server (API and dashboard)
python manage.py runserver --host 0.0.0.0 --port 8000
```

For production using Docker Compose:

```bash
docker-compose up -d --build
```

## 安装要求

| Dependency | Version / Type | Description |
|------------|---------------|-------------|
| Python | >= 3.10, < 3.13 | Core runtime; type hints and async features require modern version. |
| PostgreSQL | >= 14 | Primary database for storing resource metadata, cache entries, and health logs. |
| Redis | >= 6.2 | Optional but recommended; used for high-performance caching and rate-limiting counters. |
| Docker & Docker Compose | >= 20.10, >= 2.0 | Required only for containerized deployment; development can use SQLite and in-memory cache. |
| Git | >= 2.30 | Necessary for cloning the repository and for versioning custom plugins. |
| make | >= 4.0 | Used internally for task automation (e.g., linting, testing, migration generation). |
| gcc / build-essential | System-level | Needed for compiling certain Python native extensions (e.g., uvloop, orjson). |
| curl / wget | System-level | Utilized by the health check module for endpoint probing. |
| jq | >= 1.6 | Recommended for formatting JSON outputs in CLI scripts. |

## 文档导航

| Layer | Section | Questions Answered |
|-------|---------|-------------------|
| User Guide | Getting Started | How do I install the tool? What are the minimal configuration steps to start querying resources? |
| User Guide | CLI Reference | What commands are available? How do I filter resources by sport or region? |
| Contributor Guide | Plugin Development | How do I write a custom parser for a new data source? What is the expected interface? |
| Contributor Guide | Testing & CI | How are contributions validated? What test suites must pass before merging a pull request? |
| Operations Guide | Deployment | How do I deploy with Docker? What environment variables are required for production? |
| Operations Guide | Monitoring | How do I set up Prometheus alerts for endpoint failures? What metrics are exported? |
| API Reference | REST Endpoints | What internal API endpoints exist for programmatic access to the resource index? |
| Architecture | Design Decisions | Why was SQLite chosen for development? How does the plugin system achieve isolation? |

## 资源列表

This project aggregates and references the following external sports data resources. Each link is preserved exactly as provided by the community.

### Football Match Analysis & Predictions

- <code>leisuzuqiufenxi.asia</code>
- <code>leisuzuqiutuijian.asia</code>
- <code>leisuzuqiuyuce.asia</code>

### Live Scores & In-Play Data

- <code>leisuwanchangbifen.asia</code>
- <code>leisuzuqiubifenwang.asia</code>

### Daily Recommendations & Trends

- <code>leisujinrituijian.asia</code>

### College / Academy Sports

- <code>xueyuanyuansaiguo.asia</code>
- <code>xueyuanyuanzuqiubifenwang.asia</code>

## 项目结构

```
leisu-aggregator/
├── docker/                         # Dockerfiles and compose overlays
│   ├── dev/                        # Development Dockerfile with hot-reload
│   └── prod/                       # Production Dockerfile (slim, multi-stage)
├── docs/                           # Full documentation (MkDocs source)
│   ├── guides/                     # User and contributor guides
│   └── api/                        # Auto-generated API reference from docstrings
├── src/                            # Main application source code
│   ├── aggregator/                 # Core package
│   │   ├── core/                   # Config, exceptions, base classes
│   │   ├── models/                 # SQLAlchemy ORM models for resources and logs
│   │   ├── schemas/                # Pydantic schemas for normalization
│   │   ├── fetcher/                # Async HTTP client, retry logic, user-agent rotation
│   │   ├── parser/                 # Base parser interface and built-in transformers
│   │   ├── cache/                  # Redis and in-memory cache adapters
│   │   ├── health/                 # Health check orchestrator and probe implementations
│   │   ├── cli/                    # Click-based CLI commands (manage.py entrypoint)
│   │   └── plugins/                # Plugin discovery and sandboxing utilities
│   ├── plugins/                    # User-writable directory for custom parsers
│   │   └── examples/               # Sample plugins with detailed comments
│   └── scripts/                    # Maintenance scripts (DB backup, index sync)
├── tests/                          # Pytest suite (unit, integration, and contract tests)
│   ├── unit/                       # Tests for models, schemas, and parsers
│   ├── integration/                # Tests that require external network (mock by default)
│   └── fixtures/                   # JSON/YAML sample data for test cases
├── resources/                      # YAML resource index files and data dictionaries
│   ├── default_index.yaml          # Primary resource catalog with all 8 links
│   └── schemas/                    # JSON Schema definitions for validation
├── scripts/                        # Shell scripts for CI, deployment, and cron jobs
├── .env.example                    # Environment variable template
├── docker-compose.yml              # Production stack (app, postgres, redis, prometheus)
├── docker-compose.dev.yml          # Development stack with volume mounts
├── Makefile                        # Common tasks (install, test, lint, migrate)
├── pyproject.toml                  # PEP 621 project metadata and dependencies
└── README.md                       # This file
```

## 贡献指南

We welcome contributions of all forms, from bug reports and documentation improvements to new plugins and parser implementations. Please follow these steps to ensure a smooth collaboration.

1. **Fork the Repository and Set Up Development Environment** – Create a personal fork on GitHub, clone it locally, and run `make install-dev` to set up the virtual environment with all development dependencies (pytest, black, mypy, ruff).

2. **Choose an Issue or Propose a New Feature** – Browse the issue tracker for open tasks tagged `good-first-issue` or `help-wanted`. For significant changes, open a discussion thread first to align with the maintainers on design and scope.

3. **Write Tests and Update Documentation** – Every new parser or CLI command must include corresponding unit tests and integration stubs. Update the `docs/` directory with any user-facing changes, using the existing MkDocs structure.

4. **Run the Quality Checks** – Execute `make lint` and `make test` locally. The CI pipeline runs the same checks; ensure all tests pass and code coverage does not decrease. Use `make format` to apply automatic code style fixes.

5. **Submit a Pull Request** – Open a pull request against the `main` branch with a clear title and description referencing the related issue. Include a checklist of completed items. Maintainers will review within 5 business days and may request changes.

## 常见问题

**Q: How often should I run the health check against the external resources?**

A: The recommended interval depends on your use case. For production monitoring, we suggest running the full health check every 15 minutes using a cron job or the included systemd timer. For development or casual use, once daily is sufficient. The CLI supports `--interval` flags for custom scheduling. Be mindful of rate limits – the tool uses exponential backoff and respects `Retry-After` headers when present.

**Q: Can I add my own external URLs not listed in the default index?**

A: Yes. You can maintain a private YAML file and load it using `python manage.py load_index --source my_private_index.yaml`. The file format matches the schema defined in `resources/schemas/index_schema.json`. Additionally, you can extend the plugin system to include custom authentication headers or token refresh logic for protected endpoints. Note that the project does not store or share any user-added URLs; they remain local to your deployment.

**Q: What happens if an external resource changes its API structure or moves to a new domain?**

A: The health check system will detect HTTP 404 or unexpected response payloads and log a critical alert. You can then update the corresponding entry in your index file and rerun `load_index` to refresh the metadata. For plugin-based parsers, you can modify the transformation logic in the `plugins/` directory without restarting the server – the plugin loader picks up changes on the next request cycle. We also maintain a community-driven `known_changes.md` file where contributors can share migration notes for popular endpoints.

## 许可证

This project is licensed under the MIT License. You are free to use, modify, distribute, and sublicense the software for commercial and non-commercial purposes, provided that the original copyright notice and permission notice are included in all copies or substantial portions of the software. The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

> 外链数量: 8 | 生成时间: 2026-07-20 16:42:44
