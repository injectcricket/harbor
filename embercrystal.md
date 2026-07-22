# LeiSu Resource Aggregator

LeiSu Resource Aggregator is a curated technical index and navigation system designed for developers, data analysts, and technical researchers who require structured access to specialized online resources. The project addresses the challenge of managing disparate, domain-specific external links by providing a unified, machine-readable catalog with version-controlled metadata, dependency tracking, and automated health checks for each registered endpoint. Unlike generic bookmark managers or simple link lists, LeiSu treats each external resource as a first-class entity with defined schemas, update frequencies, and content-type fingerprints, enabling downstream automation, monitoring pipelines, and reproducible research workflows.

This project is not a web scraper, a proxy, or a content mirror. It is a strict metadata registry and validation layer. Target users include infrastructure engineers building internal developer portals, researchers curating longitudinal datasets from volatile third-party sources, and DevOps teams who need to externalize configuration-driven endpoint lists with audit trails. The core philosophy is "explicit over implicit": every resource entry must declare its expected response schema, availability SLA, and maintenance contact. LeiSu provides the tooling to enforce these declarations through CI/CD hooks, scheduled probes, and structured logging.

## 功能概览

- **Endpoint Registry with Schema Validation** – Each registered URL is accompanied by a JSON Schema definition that validates the resource's response structure, enabling automated content integrity checks without manual inspection.

- **Automated Health Probing Scheduler** – Periodic HTTP/HTTPS HEAD and GET requests are issued against all endpoints with configurable intervals, retry policies, and failure escalation rules, producing time-series availability metrics.

- **Markdown-to-JSON Compilation Pipeline** – The primary README and associated documentation are parsed into structured JSON metadata, allowing external tools to consume the resource list as a machine-readable API without scraping HTML.

- **Differential Change Detection** – When resource entries are updated (URL changes, schema modifications, or new tags), the system generates a unified diff report that can be integrated into pull request workflows for peer review.

- **Tag-Based Categorization and Filtering** – Every resource can be assigned multiple tags (e.g., "sports-data", "real-time", "asia-region"), and the CLI tool provides subcommands to filter, sort, and export subsets based on tag combinations.

- **Dependency Graph Visualization** – Resources can declare inter-dependencies (e.g., "requires X to be reachable before Y"), and the system generates a directed acyclic graph (DAG) in DOT format for visual inspection using Graphviz.

- **Slack/Webhook Alerting Integration** – Probes that detect prolonged downtime or schema drift can trigger configurable outgoing webhooks, enabling integration with incident management platforms such as PagerDuty or Opsgenie.

## 应用场景

- **Data Pipeline Bootstrapping** – Data engineering teams can use LeiSu as a bootstrapping configuration source for Apache Airflow DAGs. The registry provides the initial list of external data sources, their expected response formats, and update cadences, allowing pipeline code to remain environment-agnostic while the registry drives runtime behavior.

- **Regional Content Aggregation for Asia-Focused Analytics** – Analysts specializing in Asian market trends can rely on LeiSu to maintain a curated list of region-specific data endpoints. The registry includes metadata about geographic origin, language headers, and typical latency patterns, aiding in the selection of optimal sources for time-sensitive dashboards.

- **Internal Developer Portal Backend** – Platform engineering teams can embed LeiSu's compiled JSON output into their internal developer portal as a "trusted endpoints" section. This provides developers with a single source of truth for approved external APIs, reducing the risk of shadow IT and undocumented dependencies.

- **Research Reproducibility Kit** – Academic researchers can distribute their study's data source registry alongside their paper. LeiSu's versioning and schema validation ensure that future reviewers or replicators can verify the availability and structure of the original data sources, even years after publication.

- **Chaos Engineering Target List** – SRE teams can feed LeiSu's health probe results into chaos engineering experiments. By correlating endpoint failures with internal service degradations, teams can identify external dependencies that are single points of failure and design appropriate circuit-breaker strategies.

## 快速开始

The following sequence assumes a POSIX-compliant shell environment with Git, Node.js (v18 or later), and npm installed. All commands should be executed in the terminal.

```bash
# Clone the repository from the official upstream
git clone https://github.com/leisu-agg/leisu-resource-aggregator.git
cd leisu-resource-aggregator

# Install production and development dependencies
npm install

# Run the initial compilation and health check on the default resource set
npm run build
npm run probe:all

# To view the compiled resource manifest as JSON
cat dist/manifest.json | jq '.'
```

For local development, the `npm run dev` command starts a file watcher that re-compiles the registry upon any change to the `registry/` directory. To run a single health check against a specific resource by its ID, use `npm run probe -- --id <resource-id>`.

## 安装要求

The following table lists all mandatory dependencies, their minimum required versions, and the purpose they serve within the system. Older versions may work but are not actively tested. Production deployments should match these specifications.

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Node.js | v18.0.0+ | JavaScript runtime for the compilation and probing engine. The asynchronous event loop is heavily utilized for concurrent health checks. |
| npm | v9.0.0+ | Package manager used to install all third-party libraries listed in package.json. Also used to run lifecycle scripts. |
| Git | v2.30.0+ | Required for cloning the repository and for automatic version tagging during CI/CD releases. |
| jq | v1.6+ | Optional but strongly recommended for command-line JSON processing and human-readable output formatting in the probe results. |
| curl | v7.68.0+ | Used as a fallback HTTP client when Node.js native fetch is unavailable or when debugging TLS/certificate issues. |
| Graphviz | v2.40.1+ | Only required for generating dependency graphs. If absent, the `graph` subcommand will display a warning but continue functioning. |
| ShellCheck | v0.7.0+ | Development-time dependency for linting all shell scripts included in the `scripts/` directory. CI pipelines will fail if ShellCheck is not installed. |

## 文档导航

The documentation is organized into four conceptual layers, each addressing a distinct audience and set of questions. The table below maps each layer to its primary directory and the typical inquiries it answers.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户入门层 | `docs/getting-started/` | How do I install the tool? How do I add my first resource? How do I run a basic health check? What do the probe status codes mean? |
| 操作管理层 | `docs/operations/` | How do I configure the scheduler? What are the recommended probe intervals? How do I integrate with Slack? How do I backup the registry? |
| 开发者扩展层 | `docs/development/` | What is the plugin interface for custom validators? How do I write a new probe strategy? What are the coding conventions? How do I run the test suite? |
| 架构参考层 | `docs/architecture/` | What is the overall system design? How are schemas versioned? How does the differential detection work internally? What are the data flow diagrams? |

For a comprehensive index of all available CLI commands, refer to `docs/cli-reference.md`. For the formal specification of the resource registry schema, see `docs/schema-spec.md`.

## 资源列表

This section enumerates every external resource that is registered and actively monitored by the LeiSu Aggregator. Each entry is listed exactly as provided by the upstream curator, without any normalization of protocols, subdomains, or trailing slashes. The resources are grouped by thematic category for improved navigation, but the raw URL strings remain unaltered.

### 即时比分与实时数据类

- <code>leisushishibifen.asia</code>
- <code>leisuwanchangbifen.asia</code>
- <code>leisuzuqiubifenwang.asia</code>

### 足球数据分析类

- <code>leisuzuqiufenxi.asia</code>

### 足球推荐与预测类

- <code>leisuzuqiutuijian.asia</code>
- <code>leisuzuqiuyuce.asia</code>

### 综合推荐类

- <code>leisujinrituijian.asia</code>

All resources listed above are probed at an hourly interval by default. If any resource is unreachable for three consecutive probes, an alert is generated. The full probe history is stored in `logs/probes/` with daily rotation. For each resource, the registry also maintains a `meta.json` file that contains the last known content-type, content-length, and a SHA-256 checksum of the response body, enabling drift detection even when the endpoint remains HTTP 200 OK.

## 项目结构

The repository follows a modular monorepo style, with clear separation between source code, registry data, documentation, and operational artifacts. The ASCII tree below illustrates the top-level directories and key files, each annotated with its primary responsibility.

```
leisu-resource-aggregator/
├── registry/                        # Source-of-truth for resource entries
│   ├── schemas/                     # JSON Schema definitions per resource ID
│   │   ├── live-score.schema.json   # Validates <code>leisushishibifen.asia</code> responses
│   │   └── football-odds.schema.json # Shared schema for odds-related endpoints
│   ├── entries/                     # One YAML file per registered URL
│   │   ├── live-score.yaml          # Contains tags, owner, probe interval
│   │   ├── analysis.yaml            # For <code>leisuzuqiufenxi.asia</code>
│   │   └── prediction.yaml          # For <code>leisuzuqiuyuce.asia</code>
│   └── manifest.yaml                # Global registry index with all IDs
├── src/                             # TypeScript source code
│   ├── probes/                      # Probe strategy implementations
│   │   ├── http-probe.ts            # Core HEAD+GET logic with redirects
│   │   └── schema-validator.ts      # Validates response against schema
│   ├── scheduler/                   # Cron-based orchestration engine
│   ├── reporters/                   # Output formatters (JSON, console, Slack)
│   └── cli/                         # Commander.js command definitions
├── tests/                           # Jest unit and integration tests
│   ├── unit/                        # Isolated function tests
│   └── integration/                 # Full-stack tests with mock HTTP server
├── docs/                            # All user and developer documentation
│   ├── getting-started/             # Quick start, installation, FAQ
│   ├── operations/                  # Deployment, monitoring, backup
│   └── architecture/                # Design decisions, data flow diagrams
├── scripts/                         # Shell utilities for maintenance
│   ├── rotate-logs.sh               # Log rotation with gzip compression
│   └── generate-deps-dot.sh         # Emits Graphviz DOT from dependency graph
├── dist/                            # Compiled JavaScript output (gitignored)
├── logs/                            # Probe logs and health check history
│   └── probes/                      # Daily log files named by YYYY-MM-DD
├── .github/                         # CI/CD workflows and issue templates
│   └── workflows/                   # GitHub Actions for testing and publishing
├── package.json                     # npm dependencies and script aliases
├── tsconfig.json                    # TypeScript compiler options
└── README.md                        # This document (auto-compiled to JSON)
```

## 贡献指南

Contributions to LeiSu Resource Aggregator are welcomed, provided they align with the project's focus on metadata quality and automation reliability. All contributions must go through the standard pull request workflow, with mandatory reviews from at least one core maintainer for any changes affecting the `registry/` directory.

1.  **Fork and Clone** – Create a personal fork of the main repository and clone it locally. Set the upstream remote to the original repository to sync changes before submitting your pull request.

2.  **Create a Feature Branch** – Use a descriptive branch name that includes a prefix such as `feat/`, `fix/`, or `docs/`, followed by a short identifier of the change. For example, `feat/add-new-resource-asia`.

3.  **Implement Changes with Tests** – For any new probe strategy or schema validator, write corresponding unit tests in the `tests/unit/` directory. For adding a new resource, update the appropriate YAML file in `registry/entries/` and ensure the schema passes validation.

4.  **Run the Full Test Suite** – Execute `npm test` and `npm run lint` locally. The CI pipeline will reproduce these checks. Any failures must be resolved before the pull request can be merged.

5.  **Submit a Pull Request** – Open a pull request against the `main` branch with a clear title and description that references any relevant issues. The description must include a change log entry in the format specified in `CONTRIBUTING.md`.

6.  **Address Review Feedback** – Maintainers may request additional tests, documentation updates, or clarifications. Respond to all comments in a timely manner. Once all checks pass and at least one maintainer approves, the PR will be squashed and merged.

## 常见问题

**Q: How does LeiSu handle resources that use HTTP instead of HTTPS, or that redirect to a different domain?**

The probe engine follows redirects up to a configurable maximum (default 5) and records the final resolved URL in the probe result. However, the registry entry itself is never automatically updated to the redirected URL; this requires a manual review and an explicit commit. For HTTP-only resources, the probe logs a warning but does not fail the check, as some legacy endpoints do not support TLS. The schema validation step is still executed on the final response body.

**Q: What happens if a resource's schema changes frequently, causing repeated validation failures?**

The registry supports a schema versioning scheme where each resource can specify a `schema-version` field in its YAML entry. When a schema change is detected via the differential check, the system generates a diff report but continues to use the old schema for three consecutive probe cycles. This grace period allows the resource owner to update the schema file without immediate pipeline disruption. After the grace period, validation failures become hard errors and trigger alerts. The recommended practice is to coordinate schema updates with the upstream resource provider and test the new schema in a staging branch.

**Q: Can LeiSu be used behind a corporate proxy or in an air-gapped environment?**

Yes, the HTTP probe module respects the `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` environment variables as per the Node.js `global-agent` package. For air-gapped environments where no external network access is allowed, LeiSu can be configured in "offline" mode where all probe tasks are skipped and only the local manifest compilation is performed. The offline mode is activated by setting the environment variable `LEISU_OFFLINE=true`. In this mode, the system still performs schema validation against locally cached responses, which must be pre-populated via an out-of-band process.

## 许可证

This project is licensed under the terms of the MIT License. The full license text is available in the `LICENSE` file in the root of the repository. Under this license, you are free to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, subject to the condition that the above copyright notice and this permission notice are included in all copies or substantial portions of the software. The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

> 外链数量: 7 | 生成时间: 2026-07-22 18:16:59
