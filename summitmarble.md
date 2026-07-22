# LinkPilot Resource Navigator

LinkPilot Resource Navigator is a curated, high-availability technical resource aggregation platform designed for developers, data analysts, and technical researchers who require rapid access to specialized online information sources. The project addresses the fundamental challenge of resource fragmentation by providing a structured, maintainable, and version-controlled repository of domain-specific external links, accompanied by local tooling for availability monitoring, metadata tagging, and usage analytics. It is not a search engine nor a general-purpose bookmark manager; it is a purpose-built technical reference index tailored for project-specific external dependency tracking and team-shared knowledge base construction.

The primary target users include DevOps engineers maintaining integration endpoint inventories, data pipeline architects tracking upstream data sources, technical leads curating team documentation hubs, and researchers aggregating domain-specific portals. By treating external URLs as first-class project assets with versioned change logs, dependency declarations, and status probes, LinkPilot transforms an ad-hoc collection of bookmarks into an auditable, scriptable, and documentable subsystem of your broader infrastructure.

## 功能概览

- **Structured Resource Indexing** – Organize external URLs under hierarchical categories with support for tags, descriptions, and optional expiration dates, enabling systematic discovery rather than haphazard browsing.

- **Automated Availability Probing** – Built-in lightweight HTTP health checks that periodically validate each resource endpoint, flagging unreachable or slow-responding entries with configurable retry policies and alert hooks.

- **Metadata Enrichment Pipeline** – Extract and store Open Graph titles, description snippets, and content-type headers for each resource, providing contextual previews without requiring manual data entry.

- **Versioned Change Tracking** – Maintain a full audit history of all additions, removals, and metadata edits using Git-backed storage, allowing rollback, diff comparison, and blame attribution across team members.

- **Tag-Based Filtering and Query** – Support multi-dimensional tagging (e.g., "sports-data", "realtime-api", "backup-mirror") with boolean query expressions, enabling dynamic view generation for different project contexts.

- **Export Adapters** – Generate machine-readable output in JSON, YAML, or plain Markdown table formats for integration with CI/CD pipelines, static site generators, or monitoring dashboards.

- **Local Development Server** – Provide a lightweight web-based administration interface (optional) for browsing, searching, and editing the resource inventory, with read-only public view mode for stakeholders.

## 应用场景

- **Technical Documentation Hub Maintenance** – A development team maintaining a company-wide developer portal uses LinkPilot to centralize all external API documentation links, SDK repositories, and reference implementations. The availability probe automatically notifies the team when a critical Swagger endpoint becomes temporarily unreachable, allowing proactive documentation updates before user complaints arise.

- **Data Pipeline Source Tracking** – A data engineering team managing ETL workflows relies on multiple external data feeds from various sports statistics providers. LinkPilot tracks each source URL along with its expected update cadence, response format, and fallback mirrors. When the primary source changes its endpoint structure, the versioned change log helps the team correlate pipeline failures with specific URL modifications.

- **Academic Research Resource Aggregation** – A research group studying real-time information dissemination maintains a curated list of domain-specific portals and statistical dashboards. LinkPilot allows each researcher to propose new resources via pull requests, with automated preview generation showing title and description, reducing the overhead of manual verification before merging into the shared knowledge base.

- **Incident Response Runbook Integration** – An SRE team embeds LinkPilot resource references into their runbook playbooks. During an incident, engineers query the local index to quickly locate external status pages, vendor support portals, and emergency contact dashboards, with all URLs pre-validated to avoid dead links during critical moments.

- **Compliance and Audit Trail** – A regulated fintech startup uses LinkPilot to document all third-party data sources consumed by their risk models. The version history and metadata fields capture the business owner, approval date, and data classification for each URL, simplifying compliance reviews and external audit requests.

## 快速开始

The following sequence clones the repository, installs dependencies, and launches the local development server with a sample resource index.

```bash
git clone https://github.com/your-org/linkpilot-navigator.git
cd linkpilot-navigator
pip install -r requirements.txt
python manage.py migrate
python manage.py loaddata sample_resources.json
python manage.py runserver --port 8080
```

For production deployment, refer to the deployment guide in the `docs/` directory. The server binds to `127.0.0.1` by default; adjust `--host` and `--port` parameters as needed for your environment.

## 安装要求

The project is implemented in Python 3.10+ with optional PostgreSQL support. All core dependencies are listed in the requirements file. The table below summarizes the mandatory and optional components required for full functionality.

| 依赖 | 必需 | 说明 |
|------|------|------|
| Python 3.10 or higher | 是 | Core runtime; type hints and async features require 3.10+ |
| pip 22.0+ | 是 | Package installer for resolving dependencies |
| SQLite 3.35+ (or PostgreSQL 13+) | 是 | Database backend; SQLite used by default for development, PostgreSQL recommended for production |
| Git 2.25+ | 是 | Required for versioned change tracking and repository cloning operations |
| curl 7.68+ or httpie 2.0+ | 否 | Used by the availability probe; falls back to urllib if not present |
| Redis 6.2+ | 否 | Optional cache backend for probe results; improves performance at scale |
| Docker 20.10+ | 否 | Containerized deployment; required only for production container builds |
| make 4.2+ | 否 | Used for running common development tasks; convenience layer, not mandatory |
| jq 1.6+ | 否 | Recommended for command-line JSON output parsing during scripting |

## 文档导航

The documentation is organized into four primary layers, each addressing a distinct set of user questions and operational concerns. Refer to the table below to locate the appropriate guide based on your current task.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| User Guide | `docs/user-guide/` | How do I add, edit, or remove a resource? How do I tag entries and perform advanced searches? What does the web interface show? |
| Administrator Guide | `docs/admin-guide/` | How do I configure the availability probe intervals? How do I set up email notifications for failed probes? How do I migrate from SQLite to PostgreSQL? |
| Developer Reference | `docs/dev-reference/` | What is the internal data model? How do I write a custom export adapter? What are the public API endpoints and their request/response schemas? |
| Contribution Handbook | `docs/contributing/` | What is the coding style guide? How do I run tests locally? What is the pull request review process? How do I propose a new resource category? |

## 资源列表

The following external resources are aggregated and maintained as part of the LinkPilot default index. These are provided for demonstration and testing purposes. Each URL is listed exactly as provided, with no modifications to protocol, domain, or path formatting.

**Domain Statistics and Real-time Data Portals**

<code>zuqiucaifujinrituijian.org.cn</code>

<code>qiutanbifenw.com.cn</code>

<code>500jishibifen.asia</code>

<code>500bifenzhibo.asia</code>

<code>500saiguo.asia</code>

<code>500yuce.asia</code>

<code>500shishibifen.asia</code>

## 项目结构

The source tree follows a modular, function-oriented layout. Each major subsystem resides in its own top-level directory, with clear separation between core logic, presentation layers, and operational tooling.

```
linkpilot-navigator/
├── core/                                 # Core domain models and business logic
│   ├── models/                           # Database models (Resource, Tag, ProbeResult)
│   │   ├── resource.py                   # Resource entity with metadata fields
│   │   ├── tag.py                        # Tagging model and many-to-many relations
│   │   └── probe.py                      # Health check history and status tracking
│   ├── services/                         # Service layer for external operations
│   │   ├── fetcher.py                    # HTTP metadata extraction service
│   │   ├── prober.py                     # Async availability checker with retry
│   │   └── exporter.py                   # JSON/YAML/Markdown serialization
│   └── utils/                            # Shared utilities and helpers
│       ├── validators.py                 # URL normalization and validation
│       └── timezone.py                   # Timezone-aware timestamp helpers
├── api/                                  # RESTful API endpoints (FastAPI/Flask)
│   ├── routes/                           # Route handlers grouped by resource type
│   │   ├── resources.py                  # CRUD endpoints for resource entries
│   │   ├── tags.py                       # Tag management endpoints
│   │   └── probes.py                     # Probe trigger and result retrieval
│   └── schemas/                          # Pydantic request/response schemas
│       ├── resource_schema.py            # Validation schemas for resources
│       └── probe_schema.py               # Validation schemas for probe payloads
├── web/                                  # Web administration interface (optional)
│   ├── templates/                        # Jinja2 HTML templates
│   │   ├── index.html                    # Dashboard and search landing page
│   │   └── detail.html                   # Single resource detail view
│   └── static/                           # CSS and minimal client-side JavaScript
│       ├── styles.css                    # Base layout and responsive design
│       └── dashboard.js                  # Live search and filter interactions
├── scripts/                              # Operational and maintenance scripts
│   ├── probe_runner.py                   # CLI script to run probes on demand
│   ├── import_legacy.py                  # Migrate old bookmark exports into LinkPilot
│   └── export_snapshot.py                # Generate static snapshot of the index
├── tests/                                # Unit and integration tests
│   ├── unit/                             # Isolated module tests
│   │   ├── test_models.py                # Model validation and relationships
│   │   └── test_services.py              # Service layer logic with mocks
│   └── integration/                      # End-to-end API and database tests
│       ├── test_api.py                   # API endpoint coverage
│       └── test_probe_live.py            # Live network probe tests (opt-in)
├── docs/                                 # Project documentation (see navigation table)
│   ├── user-guide/                       # End-user walkthroughs
│   ├── admin-guide/                      # Deployment and configuration
│   └── contributing/                     # Developer onboarding and workflows
├── config/                               # Environment-specific configuration
│   ├── development.py                    # Debug-friendly settings
│   ├── production.py                     # Production hardened settings
│   └── testing.py                        # Test suite configuration
├── data/                                 # Data storage (ignored in version control)
│   ├── resources.db                      # SQLite default database file
│   └── probe_cache/                      # Cached probe response payloads
├── requirements.txt                      # Production dependencies
├── requirements-dev.txt                  # Development and test dependencies
├── Makefile                              # Common tasks (test, lint, run, export)
├── README.md                             # This document
└── LICENSE                               # MIT License text
```

## 贡献指南

We welcome contributions from the community. Please follow the steps below to ensure a smooth contribution process. All contributions are reviewed with the same thoroughness as internal changes.

1. **Fork the repository and create a feature branch** – Fork the main repository to your personal account, then create a branch with a descriptive name (e.g., `feature/add-resource-category`, `fix/probe-timeout`) from the `main` branch. Avoid making changes directly on your fork's main branch to keep it synchronized with upstream.

2. **Set up the local development environment** – Run `make setup-dev` to install all development dependencies, pre-commit hooks, and a test SQLite database. Ensure all existing tests pass by executing `make test` before making any changes. This establishes a baseline for your modifications.

3. **Implement your changes with comprehensive tests** – Write code that adheres to the PEP 8 style guide and includes docstrings for all public functions. Add unit tests for new models or services, and integration tests for any API changes. Run `make lint` and `make test` iteratively during development to catch issues early.

4. **Update documentation and example entries** – If your contribution introduces new configuration options, API endpoints, or resource categories, update the relevant sections in the `docs/` directory. For new resource categories, add example entries to the sample data file used by `loaddata`.

5. **Submit a pull request with a detailed description** – Push your feature branch to your fork and open a pull request against the `main` branch of the primary repository. Include a clear summary of the changes, the motivation behind them, and any testing performed. Reference any related issues by number. The maintainers will respond within five business days with feedback or approval.

## 常见问题

**Q: The availability probe reports a timeout for a URL that is accessible in my browser. Why does this happen and how can I adjust it?**

A: The probe uses a default timeout of 5 seconds and follows redirects up to a maximum of 5 hops. If your target URL responds slowly due to server-side processing or heavy JavaScript rendering, the probe may time out prematurely. You can adjust the timeout and redirect limits globally in the `config/production.py` file under the `PROBE_SETTINGS` dictionary. For per-URL overrides, add a `probe_timeout` field (in seconds) to the resource metadata. Note that the probe does not execute JavaScript; it only checks HTTP status and response headers. For SPAs that require client-side rendering, consider using the `metadata_fallback` field to manually specify a description instead of relying on Open Graph extraction.

**Q: How do I migrate my existing bookmark collection (from browser exports or other tools) into LinkPilot without manual re-entry?**

A: The project includes an import script located at `scripts/import_legacy.py` that supports CSV, Netscape HTML bookmark format, and plain line-delimited URL lists. The script attempts to extract titles from the source format and maps common fields to LinkPilot's internal model. Run `python scripts/import_legacy.py --input bookmarks.html --format netscape --output resources.json` to generate a JSON file, then use `python manage.py loaddata resources.json` to import it. For custom formats, you can write a small adapter that outputs the expected JSON schema. Review the generated entries after import to ensure tag assignments and descriptions are correct.

**Q: Can I use LinkPilot as a read-only public resource index without the administration interface?**

A: Yes. The web interface includes a read-only public mode. Set `PUBLIC_READONLY = True` in your `config/production.py` file. In this mode, all write operations (add, edit, delete) are disabled through the web UI, and the API endpoints return 403 Forbidden for mutation requests. The search, filter, and detail views remain fully functional. This configuration is ideal for publishing your curated resource list to a wider audience without exposing administrative controls. For completely static deployment, use the export script to generate a standalone HTML snapshot that requires no server-side runtime.

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:30
