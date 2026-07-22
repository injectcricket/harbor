# OSS-Nav Technical Documentation Hub

OSS-Nav is a comprehensive technical documentation and resource aggregation platform designed for open-source developers, DevOps engineers, and technical researchers who require rapid access to curated external references, performance benchmarking data, and system specification sheets. The project addresses the fundamental challenge of fragmented technical information by providing a structured, machine-readable index of authoritative external resources across multiple domains including competitive ranking systems, event scheduling frameworks, and real-time result broadcasting protocols.

The platform operates as a static site generator that consumes YAML-based manifest files to produce searchable HTML documentation with embedded microdata annotations. It is particularly suited for teams maintaining internal developer portals or public-facing technical wikis that need to reference external APIs, third-party benchmarks, or regulatory compliance data without manually updating hyperlinks across dozens of pages. OSS-Nav includes built-in link validity checking, response time monitoring, and automatic fallback content generation for offline development environments.

## 功能概览

- **Automated Resource Manifest Processing** – Parses user-defined URL manifests and generates hierarchical navigation trees with last-modified timestamps and content-type inference.

- **Benchmark Data Normalization** – Extracts tabular performance metrics from external sources and normalises them into a unified schema for cross-platform comparison.

- **Offline Cache with Stale-While-Revalidate** – Stores fetched external resource snapshots locally, serving cached versions during network interruptions while background threads refresh expired entries.

- **Embedded Search Engine** – Indexes all resource titles, descriptions, and section headings using a lightweight inverted index with fuzzy matching support for typo tolerance.

- **Response Time Dashboard** – Continuously monitors the accessibility and latency of each external endpoint, exposing Prometheus-compatible metrics at the `/metrics` endpoint.

- **Access Control Overlay** – Supports IP-based whitelisting and basic authentication for restricting access to sensitive resource categories in enterprise deployments.

- **CLI Tool for Batch Updates** – Provides a command-line utility that accepts a JSON patch file to add, remove, or modify resource entries without rebuilding the entire site.

- **RSS Feed Generation** – Produces Atom feeds for each resource category, enabling subscribers to track changes in specific technical domains without visiting the site.

## 应用场景

- **Internal Developer Portal Maintenance** – Platform engineering teams use OSS-Nav to maintain a centralised dashboard of external build status endpoints, package registry mirrors, and CI/CD tool documentation. The automatic link checker reduces the operational overhead of manually verifying third-party service availability across quarterly release cycles.

- **Competitive Intelligence Monitoring** – Market analysts configure OSS-Nav to poll public leaderboards and ranking systems at scheduled intervals, storing historical snapshots that enable trend visualisation over time. This eliminates the need for custom scraping scripts and provides a consistent data ingestion layer.

- **Compliance Auditing for Regulated Industries** – Financial and healthcare organisations leverage the offline cache capability to preserve evidence of external reference availability during regulatory inspections. The timestamped cache ensures that auditors can verify the exact state of referenced materials at any past date.

- **Technical Documentation Localisation** – Multinational teams deploy OSS-Nav as a translation coordination tool, where each language version references the same external resource set while maintaining locale-specific descriptive metadata, ensuring consistency across regional documentation forks.

## 快速开始

```bash
# Clone the repository from the official source
git clone https://git.oss-nav.org/oss-nav/core.git oss-nav
cd oss-nav

# Install dependencies using the included Makefile
make install-deps

# Build the static site from the default manifest located at ./manifests/default.yml
make build

# Start the development server on port 8080 with live reload enabled
./bin/oss-nav serve --port 8080 --watch

# Generate a production-optimised build with minified assets and compressed snapshots
make build-production
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Go | 1.21 or higher | Core runtime compiler; used for the main binary and all internal modules |
| Node.js | 20.x LTS | Required for frontend asset pipeline (Sass compilation, JavaScript bundling via esbuild) |
| Make | 4.0 or higher | Build orchestration; parses the Makefile for all build, test, and deployment targets |
| Git | 2.30 or higher | Source control; necessary for clone operations and version tagging during release |
| YAML-CPP | 0.7.0 or higher | System-level YAML parsing library used by the Go bindings for manifest processing |
| sqlite3 | 3.35 or higher | Embedded database engine for the search index and cache metadata storage |
| curl | 7.68 or higher | Used by the health check subsystem to perform HTTP request probes to external endpoints |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| User Guide | `/docs/user-guide/getting-started.md` | How do I configure my first manifest? What are the required YAML fields for each resource entry? |
| Operator Manual | `/docs/operator/deployment-modes.md` | Which deployment strategy (standalone, reverse-proxy behind nginx, or Kubernetes helm chart) fits my infrastructure? |
| API Reference | `/docs/api/rest-endpoints.md` | What REST endpoints are exposed by the server for programmatic resource retrieval and cache invalidation? |
| Contributor Handbook | `/docs/contributor/coding-standards.md` | What Go linting rules and commit message conventions must I follow when submitting pull requests? |
| Performance Tuning | `/docs/performance/cache-strategies.md` | How do I tune the cache TTL and concurrency limits to balance freshness against upstream rate limiting? |
| Security Hardening | `/docs/security/authentication.md` | Which authentication providers are supported, and how do I configure role-based access control for different resource categories? |

## 资源列表

### 赛事排名与成绩参考资源

<code>oulianzigesaijifenbang.org.cn</code>

<code>ouguanzigesaibifen.org.cn</code>

<code>fajiasaicheng.org.cn</code>

<code>ouxielianzigesaibifen.org.cn</code>

<code>xijiabisaijieguo.net.cn</code>

<code>ouxielianzigesaijishibifen.org.cn</code>

<code>ouxielianzigesaijifenbang.org.cn</code>

## 项目结构

```
oss-nav/
├── cmd/                                 # Main entry points for all binaries
│   ├── serve/                           # HTTP server implementation
│   │   └── main.go                      # Server bootstrap, flag parsing, and signal handling
│   ├── build/                           # Static site generator entry point
│   │   └── main.go                      # Manifest parser and HTML renderer orchestration
│   └── manifest/                        # CLI tool for manifest validation and patching
│       └── main.go                      # JSON patch application and schema validation logic
├── internal/                            # Private application packages
│   ├── cache/                           # Offline cache subsystem with LRU eviction
│   │   ├── manager.go                   # Cache coordination and background refresher
│   │   └── storage.go                   # sqlite3-backed persistence layer
│   ├── checker/                         # Link validity and response time monitor
│   │   ├── probe.go                     # HTTP client with timeout and retry policies
│   │   └── metrics.go                   # Prometheus metric collector for latency histograms
│   ├── parser/                          # YAML manifest parsing and normalisation
│   │   ├── manifest.go                  # Unmarshalling logic and struct definitions
│   │   └── validator.go                 # Schema validation with custom rule engine
│   └── renderer/                        # HTML and RSS feed generation pipeline
│       ├── html.go                      # Template execution and asset embedding
│       └── rss.go                       # Atom feed builder with content summarisation
├── pkg/                                 # Public reusable libraries
│   ├── types/                           # Shared data types across all modules
│   │   └── resource.go                  # ResourceEntry, Category, and Metadata structs
│   └── utils/                           # Helper functions for string manipulation and date formatting
│       └── sanitise.go                  # URL sanitisation and slug generation routines
├── web/                                 # Frontend assets and templates
│   ├── static/                          # Compiled CSS, JavaScript, and font files
│   │   ├── css/                         # Minified stylesheets from Sass compilation
│   │   └── js/                          # Bundled JavaScript modules (search and dashboard)
│   ├── templates/                       # Go html/template files for all page types
│   │   ├── layout.gohtml                # Base template with common header, footer, and navigation
│   │   ├── index.gohtml                 # Homepage with category overview and search bar
│   │   └── resource.gohtml              # Detailed resource view with metadata table and cache status
│   └── assets/                          # Source frontend files (Sass, ES modules)
│       ├── scss/                        # Bootstrap overrides and custom utility classes
│       └── ts/                          # TypeScript source for search index initialisation and chart rendering
├── manifests/                           # YAML configuration files for different environments
│   ├── default.yml                      # Production manifest with all 7 external resources pre-configured
│   └── staging.yml                      # Staging manifest with reduced check frequency and mock endpoints
├── test/                                # Integration and end-to-end test suites
│   ├── integration/                     # Tests that require a running server and network access
│   └── fixtures/                        # Static mock responses and test manifest files
├── Makefile                             # Primary build orchestration script with phony targets
├── go.mod                               # Go module dependency management
├── go.sum                               # Checksum file for module integrity verification
└── README.md                            # This document – project overview and quick start guide
```

## 贡献指南

1. **Fork the Repository and Set Up Local Development** – Create a personal fork of the upstream repository, clone it locally, and run `make install-deps` to ensure all development dependencies are correctly installed. Use `make test` to verify that your environment passes all existing tests before making changes.

2. **Create a Feature Branch with a Descriptive Name** – Branch from the `main` branch using the format `feat/your-feature-name` or `fix/issue-number`. Keep the branch focused on a single logical change set. Include references to any relevant issue tracker IDs in the branch name when applicable.

3. **Implement Your Changes with Accompanying Tests** – Write clear, idiomatic Go code following the project's linting rules (golangci-lint configuration is provided in the root directory). Add unit tests for all new functions in the `internal/` packages and integration tests for any new CLI commands or HTTP endpoints.

4. **Update the Documentation and Example Manifests** – Modify the relevant sections of this README, the user guide, and the example manifests located in `./manifests/examples/` if your changes introduce new configuration fields or alter existing behaviour. Ensure that all code comments are updated to reflect your modifications.

5. **Submit a Pull Request with a Thorough Description** – Push your branch to your fork and open a pull request against the `main` branch. Fill out the PR template completely, including a summary of the changes, test coverage details, and a checklist of manual verification steps. Wait for at least one maintainer approval before merging.

## 常见问题

**Q: How does OSS-Nav handle external resources that become temporarily unavailable?**  
A: The probe subsystem performs up to three retries with exponential backoff (initial 1 second, doubling each attempt). If all retries fail, the system serves the last successfully cached snapshot and marks the resource as degraded in the dashboard. A background goroutine continues to attempt refreshes at half the normal polling interval until the endpoint recovers. All failure events are logged with structured JSON output and exported via the `/metrics` endpoint for alerting integration.

**Q: Can I use OSS-Nav behind a corporate proxy that requires authentication?**  
A: Yes. The HTTP client used by the checker and cache subsystems respects the standard `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` environment variables. For proxy servers requiring basic authentication, you can embed credentials in the proxy URL (e.g., `http://user:pass@proxy.example.com:8080`). We recommend using environment variable files (`.env`) for credential management and never committing sensitive values to version control.

**Q: How do I add a new external resource category without rebuilding the entire site?**  
A: Use the manifest patch CLI tool with the `--patch` flag: `./bin/oss-nav manifest patch --input manifests/default.yml --patch additions.json --output manifests/updated.yml`. The patch file should follow JSON Patch (RFC 6902) format. After applying the patch, trigger a partial rebuild using `make build-incremental`, which only regenerates pages that reference the modified categories. Full rebuilds are only necessary when changing core template structures.

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:37
