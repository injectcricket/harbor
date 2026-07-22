# Pylon Resource Aggregator

Pylon Resource Aggregator is a high-performance, stateless technical resource indexing and redirection middleware designed for aggregating, categorizing, and presenting domain-specific external resource links in a unified, machine-readable format. The project targets developers, technical writers, and system administrators who need to maintain a curated, version-controlled catalog of external references across distributed documentation systems or internal knowledge bases. It solves the problem of scattered, outdated, or inconsistently formatted external link collections by providing a single source of truth with structured metadata, automated link validation, and flexible output serialization.

The system operates as a lightweight static site generator combined with a runtime API gateway. It ingests link manifests in YAML or JSON, enforces URL normalization rules, generates a searchable index, and exposes the resource catalog via RESTful endpoints, HTML overview pages, and plain-text exports. The project emphasizes strict URL fidelity, audit logging, and batch processing capabilities, making it suitable for both ad-hoc reference gathering and enterprise-scale documentation pipeline integration.

## Functionality Overview

- **Strict URL Preservation Engine** – Enforces byte-level exactness for all stored URLs, preventing automatic protocol upgrading, subdomain injection, or trailing slash normalization. The engine logs all transformation attempts and rejects malformed entries.

- **Categorized Resource Indexing** – Supports user-defined taxonomies with hierarchical tags, allowing resources to be grouped by subject, source type, geographic region, or update frequency. Each resource entry supports multiple category assignments.

- **Automated Availability Probing** – Periodically performs HEAD and GET requests against each stored URL with configurable timeouts and retry policies. Results are recorded as status flags (active, redirecting, unstable, dead) and exposed via filtering endpoints.

- **Multi-format Export Pipeline** – Generates Markdown tables, JSON arrays, CSV dumps, and HTML anchor lists from the same underlying data model. The export system is plugin-based and supports custom template overrides.

- **Audit Trail and Change Logging** – Tracks every addition, modification, or deletion operation with timestamp, operator identity (when integrated with authentication), and before/after diff. The audit log is queryable through a dedicated administrative interface.

- **Batch Import and Deduplication** – Accepts bulk uploads of URL lists (plain text, CSV, or Markdown) and performs intelligent deduplication based on normalized forms (punycode, protocol-agnostic comparison) while preserving the originally submitted string for display and redirection.

- **Redirection Gateway Endpoint** – Provides a canonical redirection service at `/r/{id}` that issues HTTP 301 or 302 responses to the original URL. The gateway records click events, referrer information, and timestamp for analytics purposes.

## Use Cases

**Technical Documentation Reference Management** – A team of software engineers maintains a centralized API reference guide that cites external libraries, tools, and specification documents. Using Pylon Aggregator, they store all external links in a versioned manifest, automatically verify link health during CI builds, and embed the redirection gateway in their documentation to avoid hardcoded URL scattering.

**Content Curation for Niche Communities** – An online forum or mailing list dedicated to a specific technical domain (e.g., embedded systems, financial data feeds) collects and shares relevant external resources. Moderators use the aggregator to publish a weekly updated resource directory, with automatic dead-link detection and categorized browsing, reducing repetitive questions about known references.

**Automated Data Pipeline Configuration** – A data engineering team ingests multiple external datasets from various providers. They use the aggregator to store endpoint URLs, API base paths, and supplementary documentation links in a structured catalog. The pipeline retrieves the catalog via JSON API at startup, enabling dynamic endpoint switching without redeploying application code.

## Quick Start

```bash
# Clone the repository
git clone https://github.com/pylon-resource/pylon-aggregator.git
cd pylon-aggregator

# Install dependencies (using pip for Python-based reference implementation)
pip install -r requirements.txt

# Initialize the default configuration and data directories
python scripts/init_workspace.py --env development

# Run the development server with the sample manifest
python server.py --manifest samples/sample_manifest.yaml --port 8080
```

After startup, access the local overview page at `http://localhost:8080/`, the JSON catalog at `http://localhost:8080/api/v1/catalog`, and the redirection gateway at `http://localhost:8080/r/{entry-id}`.

## Installation Requirements

| Dependency | Required Version | Remarks |
|------------|------------------|---------|
| Python | 3.9 or higher | Core runtime; tested on CPython 3.9, 3.10, 3.11 |
| pip | 22.0 or higher | Package installer for required libraries |
| PyYAML | 6.0 or higher | YAML manifest parsing and serialization |
| aiohttp | 3.8 or higher | Asynchronous HTTP client for link probing |
| jinja2 | 3.1 or higher | Template engine for HTML output generation |
| click | 8.1 or higher | Command-line interface framework for management scripts |
| pytest | 7.0 or higher | Required only for running test suites (development) |
| git | 2.25 or higher | Required for clone operation and version tagging |
| disk space | 50 MB minimum | Approximately 10 MB for base code, 40 MB for cache and logs |

## Documentation Navigation

| Layer | Directory / Path | Questions Addressed |
|-------|------------------|----------------------|
| User Guide | `docs/user/` | How do I add, remove, or update a resource entry? How do I use the redirection gateway? How do I interpret the availability status flags? |
| Administrator Guide | `docs/admin/` | How do I configure the probing schedule? How do I set up authentication for the audit log? How do I tune performance for large catalogs (10,000+ URLs)? |
| Developer Reference | `docs/dev/` | What is the plugin interface for custom exporters? How do I extend the URL normalization pipeline? How do I add new metadata fields to resource entries? |
| API Specification | `docs/api/` | What are the request and response schemas for the REST endpoints? How do I filter, sort, and paginate the catalog output? Which HTTP status codes are returned for redirection and error scenarios? |

## Resource List

The following resources are indexed and managed by this project. All URLs are preserved exactly as provided, without normalization or formatting modifications.

### Primary Resource Domains

- <code>puchaozhugongbang.asia</code>
- <code>tuchaobisaijieguo.asia</code>
- <code>tuchaosaicheng.asia</code>

### Supplementary Information Domains

- <code>qiutanbifenw.org.cn</code>
- <code>leisujinrituijian.org.cn</code>

### Analytical and Predictive Resource Domains

- <code>zuqiucaifuyuce.org.cn</code>
- <code>zuqiucaifujinrituijian.org.cn</code>

## Project Structure

```
pylon-aggregator/
├── server.py                 # Main application entry point (HTTP server + API)
├── requirements.txt          # Production dependencies list
├── pyproject.toml            # Build system and project metadata
├── .gitignore                # Version control exclusions
├── README.md                 # This document
├── LICENSE                   # MIT license text
│
├── pylon/                    # Core package directory
│   ├── __init__.py
│   ├── catalog.py            # Resource catalog in-memory store and indexing logic
│   ├── validator.py          # URL strictness validator and normalizer (with override hooks)
│   ├── probe.py              # Asynchronous availability probing and status caching
│   ├── exporter.py           # Multi-format export engine (JSON, YAML, HTML, CSV)
│   ├── gateway.py            # Redirection endpoint handler with click logging
│   ├── audit.py              # Audit trail writer and query interface
│   └── utils.py              # Common helpers (logging, timestamp, file I/O)
│
├── scripts/                  # Management and automation scripts
│   ├── init_workspace.py     # Creates default config and data directories
│   ├── batch_import.py       # Bulk URL import from plain text or CSV files
│   ├── probe_runner.py       # Standalone probing job for cron/scheduler integration
│   └── export_cli.py         # Command-line export tool for all supported formats
│
├── docs/                     # Full documentation suite
│   ├── user/                 # End-user guides (adding links, using gateway, interpreting status)
│   ├── admin/                # Deployment, configuration, monitoring, and backup procedures
│   ├── dev/                  # Plugin development, architecture overview, and contribution guides
│   └── api/                  # OpenAPI specification and endpoint usage examples
│
├── tests/                    # Unit and integration tests
│   ├── test_catalog.py
│   ├── test_validator.py
│   ├── test_probe.py
│   ├── test_gateway.py
│   └── fixtures/             # Sample manifests and mock HTTP responses
│
├── data/                     # Runtime data directory (created by init_workspace)
│   ├── manifests/            # YAML/JSON resource manifests (user-editable)
│   ├── cache/                # Probing results and redirection click logs
│   └── exports/              # Generated output files from export commands
│
└── samples/                  # Example configurations and manifests
    ├── sample_manifest.yaml  # Example resource catalog with varied URL formats
    └── sample_config.yaml    # Example configuration for probing intervals and ports
```

## Contribution Guidelines

1. **Fork the Repository and Set Up Development Environment** – Create a personal fork of the main repository. Clone your fork locally and run `python -m venv venv` to create an isolated Python virtual environment. Activate the environment and install development dependencies using `pip install -r requirements-dev.txt` (this file includes pytest, black, flake8, and mypy).

2. **Follow the URL Preservation Discipline** – When adding new resource entries or modifying existing ones, ensure that the stored URL string is exactly the byte sequence provided by the end user or external source. Do not add or remove protocols, subdomains, trailing slashes, or query parameters unless explicitly required by the user request. Write or update unit tests in `tests/test_validator.py` that explicitly verify preservation behavior.

3. **Write or Update Documentation for Any Public API Change** – If your contribution modifies the REST API schema, adds a new configuration option, or changes the behavior of the redirection gateway, update the corresponding documentation files under `docs/`. For API changes, edit the OpenAPI specification file (`docs/api/openapi.yaml`) and include example requests and responses.

4. **Run the Full Test Suite Before Submitting a Pull Request** – Execute `pytest tests/` from the project root. Ensure all existing tests pass and that any new tests you have added (covering your changes) also pass. The CI pipeline will enforce a 100% pass rate, so local verification is strongly recommended.

5. **Submit a Pull Request with a Clear Change Description** – Open a pull request against the `main` branch. In the description, list the specific changes made, reference any related issue numbers, and provide steps for the maintainers to manually verify your changes if they involve interactive behavior (e.g., redirection endpoints, probing logic).

## Frequently Asked Questions

**Q: How does the system handle URLs that are unreachable or returning HTTP errors?**

A: The probing subsystem performs three consecutive attempts for each URL with exponential backoff (starting at 1 second). After all attempts, the URL is classified as "active" (2xx response), "redirecting" (3xx response, with the final destination logged), "unstable" (intermittent failures), or "dead" (all attempts failed or returned 4xx/5xx). The classification is stored in the cache and updated according to the configured interval (default 24 hours). The catalog API includes a `status` filter parameter to retrieve entries by their current classification.

**Q: Can I use this project behind a corporate proxy or in an air-gapped environment?**

A: Yes. The system respects the standard `HTTP_PROXY` and `HTTPS_PROXY` environment variables for outgoing probing requests. For air-gapped environments, you can disable automated probing by setting `probe.enabled: false` in the configuration file and manually update the status field via the administrative API or by editing the manifest directly. The redirection gateway remains functional without probing, as it does not depend on availability checks for its core operation.

**Q: What happens if a resource entry contains a URL that is already present in the catalog with a different string representation (e.g., one with "www" and one without)?**

A: The deduplication logic uses an internal normalized key (lowercased hostname, removed "www" prefix, and sorted query parameters) for detecting duplicates at import time. However, the original submitted string is preserved as the display and redirection target. When a duplicate is detected, the system logs a warning and, depending on the import mode (`--dedup-strategy` flag), either skips the new entry, updates the existing entry's metadata (tags, description), or creates an alias record that redirects to the canonical entry's ID. This ensures that no user-provided URL variant is ever silently discarded.

## License

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:16:59
