# Rensource Aggregator

Rensource Aggregator is a lightweight, community-driven technical resource indexing system designed for developers, researchers, and content curators who need to organize, validate, and distribute high-volume external link collections in a structured and reproducible manner. The project addresses the common problem of link rot, category drift, and manual maintenance overhead in curated resource lists by providing a simple static site generation pipeline with built-in health checks, metadata tagging, and version-controlled change tracking.

Target users include open-source documentation maintainers, technical educators, bootcamp curriculum designers, and engineering teams building internal developer portals. Rensource Aggregator does not host or proxy content; it provides a deterministic build process that transforms raw URL lists into navigable, searchable, and monitorable resource catalogs with minimal operational overhead.

## 功能概览

- **Bulk URL Ingestion** – Accepts plain-text URL lists, CSV exports, or markdown-formatted link sections and normalizes them into a unified internal data model with deduplication and protocol validation.

- **Automated Accessibility Checking** – Performs configurable HEAD/GET probes against each indexed resource to detect HTTP status anomalies, certificate expiration, and response time degradation, surfacing warnings in the build log.

- **Categorical Tagging Engine** – Applies rule-based or manual taxonomy tags to each entry, enabling multi-dimensional filtering by domain type, content format, language, or regional availability.

- **Static Site Generator Integration** – Renders the indexed resource collection as a lightweight HTML dashboard, JSON API endpoint, or markdown table suitable for embedding in existing documentation portals.

- **Versioned Snapshot History** – Stores each build output in a timestamped archive, allowing consumers to compare link sets across releases and review added, removed, or changed entries.

- **Custom Metadata Attachment** – Supports user-defined fields per URL (e.g., maintainer notes, internal priority scores, replacement candidates) without modifying the source data format.

- **CLI and CI/CD Friendly** – Exposes a command-line interface with subcommands for validate, build, check, and export, making it straightforward to integrate into GitHub Actions, GitLab Pipelines, or Jenkins.

- **Pluggable Output Adapters** – Provides reference implementations for JSON, YAML, HTML, and plain-text table outputs, with a documented interface for custom formatters.

## 应用场景

- **Documentation Team Link Audits** – A technical writing team managing a 500+ link knowledge base uses Rensource Aggregator to run weekly health checks, automatically flagging broken external references before the monthly documentation release cycle.

- **Curriculum Resource Bundling** – An online coding bootcamp maintains a curated list of supplementary reading materials across eight course modules. The aggregator generates module-specific markdown tables that are directly included in the course Git repository, ensuring consistency between the syllabus and the live resource list.

- **Internal Developer Portal Curation** – A platform engineering group aggregates internal tool documentation, monitoring dashboards, and runbook links from multiple team repositories. The aggregator combines these sources into a single searchable catalog, with build-time validation that prevents stale internal endpoints from reaching production.

- **Open-Source Newsletter Backend** – A community newsletter editor collects submission links from contributors via a public Google Sheet. The aggregator pulls the sheet data nightly, validates each link, and produces a cleaned JSON feed consumed by the newsletter generation pipeline, eliminating manual copy-paste errors.

- **Compliance Reference Tracking** – A regulated industry working group maintains a list of external standards documents and regulatory portals. The aggregator logs every build output to an immutable audit trail, providing evidence of periodic review cycles for internal compliance officers.

## 快速开始

Clone the repository, install dependencies, and run the initial build within five minutes.

```bash
git clone https://github.com/rensourceteam/rensaggregator.git
cd rensaggregator
pip install -r requirements.txt
python -m rensaggregator.cli validate --source samples/url_list.txt
python -m rensaggregator.cli build --source samples/url_list.txt --output ./build
python -m rensaggregator.cli serve --dir ./build --port 8080
```

The `validate` command performs syntax and reachability checks without generating output files. The `build` command produces the static catalog. The `serve` command starts a minimal HTTP server for local preview.

## 安装要求

| Dependency | Required Version | Purpose / Notes |
|------------|------------------|------------------|
| Python | 3.9 or higher | Core runtime; type hints and dataclasses require modern interpreter. |
| pip | 21.0+ | Package installer for resolving PyPI dependencies. |
| requests | 2.28.0+ | HTTP client used for accessibility checking and HEAD/GET probes. |
| pyyaml | 6.0+ | YAML parser for configuration files and custom metadata loaders. |
| jinja2 | 3.1.0+ | Templating engine for HTML and markdown output generation. |
| click | 8.1.0+ | CLI framework providing subcommand structure and argument parsing. |
| pytest | 7.0.0+ | Development-only dependency for running the test suite. |
| flake8 | 6.0.0+ | Development-only linting tool for code style enforcement. |
| mypy | 1.0.0+ | Development-only static type checker for maintaining internal quality. |

## 文档导航

| Layer | Directory / Resource | Questions Answered |
|-------|----------------------|---------------------|
| User Manual | `docs/usage.md` | How do I prepare my URL list? Which commands do I run daily? How do I customize output formats? |
| Configuration Reference | `docs/config.md` | What are all the available settings in `rensconfig.yaml`? How do I set timeouts, retries, and tag rules? |
| Developer Guide | `docs/development.md` | How is the codebase structured? How do I add a new output adapter or a new validation rule? |
| API Specification | `docs/api.md` | What JSON schema does the build output follow? How can external scripts consume the generated catalog programmatically? |
| Deployment Examples | `docs/deployment.md` | How do I run this in a Docker container? What are the recommended CI/CD workflows for automated nightly builds? |

## 资源列表

The following external resources are referenced in the current catalog build. They are presented exactly as provided by the user without modification.

General Domain Categories:

- <code>zhongwenrenqi.org.cn</code>
- <code>renqishaofu.org.cn</code>
- <code>bajiaoshipinapp.org.cn</code>
- <code>jiujiuyeye.org.cn</code>
- <code>zhongwenzimusiwa.org.cn</code>
- <code>renqiyouma.org.cn</code>
- <code>xiaodiaowang.org.cn</code>
- <code>chengrenjingpin18.org.cn</code>

## 项目结构

```
rensaggregator/
├── cli/                           # Command-line interface entry points
│   ├── __init__.py                # Module exports and version string
│   ├── main.py                    # Click command group with subcommand registrations
│   └── commands/                  # Individual subcommand implementations
│       ├── validate.py            # Syntax check, reachability probe, and report generation
│       ├── build.py               # Full build pipeline orchestration
│       ├── serve.py               # Local static file server with live reload
│       └── export.py              # Output adapter selection and format conversion
├── core/                          # Domain models and business logic
│   ├── models.py                  # Dataclasses for Link, Tag, Metadata, and BuildReport
│   ├── loader.py                  # URL list parsers: plain text, CSV, markdown extraction
│   ├── checker.py                 # HTTP prober with configurable retry and timeout policies
│   ├── tagger.py                  # Rule-based and dictionary-driven tag assignment engine
│   └── registry.py                # In-memory catalog builder with deduplication and conflict resolution
├── outputs/                       # Output adapters for different target formats
│   ├── json_adapter.py            # Generates structured JSON with full metadata and timestamps
│   ├── html_adapter.py            # Renders a responsive dashboard using Jinja2 templates
│   ├── markdown_adapter.py        # Produces markdown tables for documentation inclusion
│   └── yaml_adapter.py            # Exports YAML suitable for further processing in CI pipelines
├── templates/                     # Jinja2 HTML templates for the web dashboard
│   ├── base.html                  # Common layout with navigation and footer
│   ├── index.html                 # Home page showing summary stats and recent build timestamps
│   └── detail.html                # Individual link detail view with metadata and health status
├── tests/                         # Unit and integration tests
│   ├── test_loader.py             # Validates parsers against various input formats
│   ├── test_checker.py            # Mock-based HTTP probe tests with edge case coverage
│   ├── test_tagger.py             # Verifies tag assignment rules and priority ordering
│   └── fixtures/                  # Static test data including sample URL lists and configs
├── docs/                          # User and developer documentation (see navigation table)
├── samples/                       # Example input files and configuration templates
│   ├── url_list.txt               # Minimal plain-text URL list for quick testing
│   └── rensconfig.yaml            # Reference configuration with commented parameters
├── requirements.txt               # Production dependencies pinned to known-good versions
├── requirements-dev.txt           # Development dependencies including linters and type checkers
├── setup.py                       # Setuptools configuration for installable package
└── README.md                      # This document
```

## 贡献指南

We welcome contributions of all forms, including new output adapters, improved accessibility checkers, and documentation enhancements. Please follow these steps to ensure a smooth review process.

1. **Open an issue first** – Describe the problem or feature proposal with clear use cases and acceptance criteria. Wait for maintainer feedback before investing significant implementation effort.

2. **Fork and branch** – Fork the main repository and create a feature branch with a descriptive name, e.g., `feature/add-csv-output-adapter` or `fix/timeout-handling`.

3. **Run the local validation suite** – Execute `pytest tests/` and `flake8 .` to ensure all existing tests pass and code style is consistent. Add new tests for any changed behavior.

4. **Update documentation** – Modify the relevant `docs/` files to reflect your changes. If you introduce a new command-line flag, document it in the usage manual. If you add an output adapter, include an example in the configuration reference.

5. **Submit a pull request** – Provide a clear PR description referencing the original issue, include before-and-after test outputs if applicable, and ensure your commit history is clean (no merge commits, rebase onto main branch).

## 常见问题

**Q: How does the aggregator handle private or authentication-protected resources?**

A: By default, the checker performs public HEAD requests without credentials. For internal resources behind VPN or basic authentication, you can configure custom request headers in `rensconfig.yaml` under the `checker.headers` section. For resources that require interactive login, we recommend marking them with a special tag `internal-restricted` and using the `--skip-auth` flag during validation to avoid false-positive failure logs.

**Q: Can I use Rensource Aggregator with a URL list that contains duplicate entries?**

A: Yes. The `registry` module deduplicates entries based on normalized URLs (lowercase scheme and hostname, stripped trailing slashes). The build log reports the total number of unique links after deduplication and lists any duplicates that were merged. You can also enable strict mode via `--strict-duplicates` to treat duplicates as a validation error.

**Q: What is the recommended frequency for running the accessibility checker against large catalogs?**

A: For catalogs with fewer than 500 links, a daily check is appropriate. For catalogs exceeding 2000 links, we recommend weekly runs with the `--concurrency 10` flag to limit parallel requests and avoid rate-limiting issues. The checker respects the `Retry-After` header when servers respond with 429 status codes.

## 许可证

MIT

> 外链数量: 8 | 生成时间: 2026-07-20 16:42:56
