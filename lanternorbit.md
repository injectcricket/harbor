# TechNav Resource Aggregator

TechNav is a lightweight, developer-oriented technical resource navigation and aggregation system designed for open-source communities, research teams, and individual developers who need to maintain curated lists of domain-specific external references. The project addresses the common pain point of scattered, unversioned, and hard-to-share bookmark collections by providing a structured, Git-tracked repository that organizes technical assets, event result feeds, and real-time data endpoints into a predictable, machine-readable format.

Target users include DevOps engineers building internal developer portals, technical writers maintaining external link directories, and system administrators who need to publish validated resource lists for their teams. TechNav does not host content itself but serves as a canonical index with validation hooks, change logging, and formatting enforcement to ensure every external reference meets your organization's accessibility and availability standards.

## 功能概览

- **Structured Resource Indexing** – Maintain a version-controlled catalog of external technical resources with mandatory metadata fields including category, status, and last-verified timestamp.

- **Automated URL Validation** – Run built-in health checks against all indexed endpoints to detect broken links, certificate expiration, or unexpected status code changes before they affect your users.

- **Markdown-to-HTML Rendering Pipeline** – Generate a static documentation site from your resource lists using a pure-Python renderer that preserves code-block formatting and table structures.

- **Batch Import/Export Utilities** – Import existing bookmark files (HTML, JSON, or plain-text lists) and export filtered subsets by category, tag, or verification status for integration with other tools.

- **Change Logging and Audit Trail** – Every addition, removal, or metadata update is recorded with a timestamp and optional commit message, enabling full traceability for compliance or team review.

- **Custom Tagging and Filtering** – Assign multiple tags to each resource and generate dynamic views that show only entries matching specific combinations (e.g., "production-ready" + "China region").

- **Slack/Webhook Notification Adapter** – Optionally broadcast verification failures or newly added high-priority resources to your team's communication channel via a simple pluggable hook system.

## 应用场景

- **Internal Developer Portal Maintenance** – Platform engineering teams use TechNav to publish a curated list of approved external data sources (e.g., live score feeds, tournament result APIs) for their internal service mesh. The validation cron job runs daily and alerts the on-call engineer if any endpoint becomes unreachable.

- **Technical Documentation Companion** – Open-source documentation projects embed TechNav's rendered HTML output as a "References" section, ensuring that all external links in the docs are automatically checked during the CI build. Broken links cause the build to fail, preventing stale references from reaching production docs.

- **Regional Service Aggregation** – Operations teams managing multi-region deployments use TechNav to maintain separate resource groups per geographic zone. The tagging system allows them to render zone-specific navigation pages while keeping a single source of truth for all endpoints.

- **Research Data Pipeline Configuration** – Data scientists use TechNav to version-control the external data endpoints their ETL pipelines depend on. When an upstream source changes its response schema, the validation script captures the difference, and the team reviews the impact before updating pipeline configurations.

## 快速开始

Clone the repository, install dependencies, and run the initial index build with the following commands. These steps assume Python 3.10 or later and a standard Unix-like shell environment.

```bash
git clone https://github.com/technav-oss/technav-core.git
cd technav-core
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py build --input ./data/resources.yaml --output ./dist/index.html
python manage.py validate --check-all
```

The first build will create a `dist/` directory containing the rendered navigation page. The validation command checks every resource listed in the YAML index and prints a summary report to the console. For continuous use, schedule the validation script via cron or a CI scheduled workflow.

## 安装要求

All dependencies are listed in the requirements.txt file and are compatible with Python 3.10 through 3.12. The following table details the core dependencies, their minimum versions, and their roles within the system.

| Dependency | Required Version | Purpose |
|------------|------------------|---------|
| Python | 3.10 or later | Runtime interpreter and standard library |
| PyYAML | 6.0 or later | Parsing the resource index YAML files |
| requests | 2.31.0 or later | HTTP validation checks and status polling |
| markdown | 3.4.0 or later | Rendering README and documentation pages |
| click | 8.1.0 or later | Command-line interface argument parsing |
| pytest | 7.4.0 or later | Unit and integration test suite (development only) |
| black | 23.0.0 or later | Code formatter (development only) |
| mypy | 1.0.0 or later | Static type checking (development only) |
| pre-commit | 3.0.0 or later | Git hook automation for pre-commit checks |
| tox | 4.0.0 or later | Multi-environment test runner (CI only) |

## 文档导航

The documentation is organized into four main layers, each targeting a specific audience and answering distinct sets of questions. Use the table below to quickly locate the appropriate guide for your current task.

| Layer | Directory | Questions Answered |
|-------|-----------|---------------------|
| User Guide | docs/user/ | How do I add a new resource? How do I run validation manually? What does the rendered output look like? |
| Administrator Guide | docs/admin/ | How do I configure the webhook adapter? How do I set up scheduled validation in Kubernetes? How do I migrate from an older version? |
| Developer Reference | docs/dev/ | What is the internal data model? How do I extend the validator with custom checks? How does the rendering pipeline work? |
| Contributor Handbook | docs/contrib/ | What are the coding standards? How do I submit a pull request? Which tests must pass before merging? |
| Integration Guide | docs/integration/ | How do I consume the rendered JSON output in other services? Can I embed the navigation widget in an existing site? |

## 资源列表

The following external resources are indexed and maintained by TechNav for the current release cycle. Each entry is listed exactly as provided by the upstream source, without any normalization of protocol, domain formatting, or path structure. These references are used in validation tests, rendering examples, and default configuration templates.

### China Regional Tournament Feeds

<code>ajiajishibifen.org.cn</code>

<code>ajiasaicheng.org.cn</code>

<code>ajiabisaijieguo.org.cn</code>

<code>ruidianchaobifen.org.cn</code>

<code>danchaobisaijieguo.org.cn</code>

<code>danchaobifen.org.cn</code>

<code>fenchaobisaijieguo.org.cn</code>

## 项目结构

The project follows a modular layout that separates source code, test suites, configuration templates, and user-facing documentation. Each top-level directory has a specific responsibility, and the internal module organization mirrors the command structure.

```
technav-core/
├── src/                              # Main application source code
│   ├── technav/                      # Core package namespace
│   │   ├── __init__.py               # Package version and exports
│   │   ├── cli/                      # Command-line interface modules
│   │   │   ├── build.py              # Render and build commands
│   │   │   ├── validate.py           # Health-check and validation logic
│   │   │   └── import_export.py      # Batch import/export utilities
│   │   ├── models/                   # Data models for resources and tags
│   │   │   ├── resource.py           # Resource entity class and serialization
│   │   │   └── index.py              # Index collection and query methods
│   │   ├── renderer/                 # Markdown and HTML rendering engines
│   │   │   ├── markdown_ext.py       # Custom markdown extensions
│   │   │   └── html_generator.py     # Static site generator core
│   │   └── validators/               # Pluggable validation adapters
│   │       ├── http_validator.py     # HTTP status and certificate checks
│   │       └── schema_validator.py   # JSON/YAML schema compliance
│   └── hooks/                        # Webhook and notification adapters
│       ├── slack_hook.py             # Slack incoming webhook integration
│       └── stdout_hook.py            # Console logger for debugging
├── tests/                            # Unit and integration test suite
│   ├── unit/                         # Isolated module tests
│   ├── integration/                  # End-to-end workflow tests
│   └── fixtures/                     # Sample YAML indices and mock responses
├── docs/                             # User, admin, and developer documentation
│   ├── user/                         # End-user guides and tutorials
│   ├── admin/                        # Deployment and maintenance manuals
│   ├── dev/                          # API reference and extension guides
│   └── contrib/                      # Contributor onboarding and style guides
├── data/                             # Default resource index templates
│   ├── resources.yaml                # Primary resource catalog (user-editable)
│   └── schema.yaml                   # JSON schema for resource validation
├── scripts/                          # Utility scripts for CI and local dev
│   ├── pre-commit.sh                 # Pre-commit hook runner
│   └── daily-validate.sh             # Scheduled validation wrapper
├── config/                           # Environment-specific configuration
│   ├── dev.conf                      # Development settings
│   └── prod.conf                     # Production overrides (secrets excluded)
├── requirements.txt                  # Production dependency list
├── requirements-dev.txt              # Development and test dependencies
├── setup.py                          # Package installation metadata
├── README.md                         # This document
└── LICENSE                           # MIT license text
```

## 贡献指南

We welcome contributions of all types, including bug reports, documentation improvements, new validators, and additional renderer backends. Please follow the steps below to ensure a smooth review process.

1. Fork the main repository and create a feature branch with a descriptive name that references the issue number if applicable. Use `git checkout -b feature/your-feature-name` to start your work.

2. Install the development dependencies by running `pip install -r requirements-dev.txt` and activate the pre-commit hooks with `pre-commit install`. This ensures code style and basic static checks run automatically before each commit.

3. Write or adapt tests for your changes in the `tests/` directory. All new validators must include at least three test cases: a successful validation, a failure scenario, and an edge case. Run the full test suite with `pytest` and confirm all tests pass.

4. Update the relevant documentation files under `docs/` to reflect your changes. If you add a new CLI command, include a usage example in the user guide. If you modify the data model, update the developer reference accordingly.

5. Submit a pull request to the main branch. In the PR description, reference the issue number, list the changes made, and provide a screenshot or log snippet that demonstrates the new functionality. The maintainers will review your submission within five business days.

## 常见问题

**Q: The validation script reports a timeout for an endpoint that is accessible in my browser. Why does this happen?**

A: The validator uses a default timeout of 3 seconds and does not follow browser-level redirects beyond the first hop. Some endpoints implement geo-aware routing or JavaScript-based handshakes that are not fully simulated by the simple HTTP GET request. To adjust the timeout, pass the `--timeout` parameter to the validate command. For endpoints that require custom headers, add the `headers` field to the resource entry in the YAML index.

**Q: Can I use TechNav to index internal endpoints that are not accessible from the public internet?**

A: Yes. The validation step can be skipped for internal resources by setting the `verify: false` flag in the resource metadata. The renderer will still include the resource in the navigation output but mark it as "internal-only" with a distinct visual indicator. For production use, we recommend running the validation from within your internal network or using a VPN tunnel to avoid false failures.

**Q: How do I upgrade from an older version without losing my custom resource index?**

A: The resource index is stored separately in the `data/resources.yaml` file, which is excluded from version upgrades through the `.gitignore` and upgrade scripts. When running `git pull` to fetch the latest release, your index file will not be overwritten. However, if the schema changes between releases, run the migration script `python manage.py migrate --index data/resources.yaml` to automatically update the format. Always back up your index before running major version upgrades.

## 许可证

This project is distributed under the MIT License, which permits unrestricted use, modification, distribution, and sublicensing for both commercial and non-commercial purposes, provided that the original copyright notice and permission notice are retained in all copies or substantial portions of the software. The full license text is available in the LICENSE file included in the repository root.

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:24
