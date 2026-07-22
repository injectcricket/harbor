# Fajiabi Score Hub

Fajiabi Score Hub is a lightweight, developer-oriented technical resource aggregation and external link management system designed for sports data enthusiasts, odds analysts, and real-time score tracking developers. This project serves as a curated navigation gateway that organizes and presents structured external score sources, match result references, and competitive analysis endpoints without replicating or hosting any underlying data.

The primary audience includes independent data engineers, hobbyist bettors building personal analytics dashboards, and open-source contributors seeking a maintainable reference architecture for external link aggregation. By providing a clean, configurable interface over multiple external score providers, Fajiabi Score Hub reduces the friction of manually discovering and validating score endpoints, enabling users to focus on data processing, visualization, and automated alerting logic.

## 功能概览

- **Unified External Link Registry** – Centralized storage and versioning of all configured score source URLs with metadata tags for origin, update frequency, and content type.

- **Configurable Category Grouping** – Dynamic grouping of links by league, region, or data type (e.g., live scores, historical results, odds comparisons) via a single YAML configuration file.

- **Health Check Endpoint** – Built-in cron-style HTTP ping monitor that periodically validates each external URL's availability and response time, logging failures to a rotating file.

- **RESTful Query Interface** – Exposes a read-only JSON API over the aggregated link set, supporting filtering by category, keyword search, and pagination for large collections.

- **Static Site Generation Mode** – Optionally outputs a fully static HTML dashboard that can be deployed to any CDN or web server, with zero runtime dependencies.

- **Docker-Ready Deployment** – Includes a production-grade Dockerfile and compose example for one-command startup in containerized environments.

- **Prometheus Metrics Export** – Exposes standard Prometheus metrics for request counts, external call latencies, and health check results, suitable for integration with monitoring stacks.

- **Audit Logging** – Tracks all link access attempts and health check events with timestamps, source IP (optional), and user-agent, supporting compliance and debugging needs.

## 应用场景

- **Personal Sports Dashboard Development** – A data engineer building a custom real-time score dashboard can use Fajiabi Score Hub as the configuration backend. Instead of hardcoding multiple score URLs, the engineer defines all endpoints once in the hub, then consumes the JSON API from a frontend React or Vue application. This decouples data source changes from code redeployment.

- **Automated Odds Comparison System** – An algorithmic trader developing a odds aggregation pipeline integrates Fajiabi Score Hub to manage the rotating set of odds providers. The hub's health check feature automatically removes dead or slow sources, ensuring the pipeline only queries responsive endpoints. The trader can update the link set via configuration without restarting the pipeline.

- **Research Data Collection for Academic Analysis** – A sports analytics researcher uses the static site generation mode to create a snapshot of all relevant score sources at a given time. This snapshot serves as a reproducible reference for methodology papers, allowing peer reviewers to verify the data sources used in retrospective studies.

- **Local Development Sandbox for External API Integration** – A junior developer learning to consume third-party sports APIs leverages Fajiabi Score Hub as a mock gateway. By pointing the hub to staging or sandbox URLs, the developer can experiment with error handling, retry logic, and response parsing without affecting production systems or incurring rate limits.

## 快速开始

Below are the standard steps to clone, install dependencies, and run the Fajiabi Score Hub in development mode. Ensure you have Git and Go (version 1.21 or later) installed on your system.

```bash
# Clone the repository from the official source
git clone https://github.com/fajiabi/score-hub.git
cd score-hub

# Install Go module dependencies
go mod download
go mod verify

# Build the binary from source
go build -o bin/fajiabi-hub ./cmd/server

# Run the server with default configuration (listening on port 8080)
./bin/fajiabi-hub --config ./configs/default.yaml --port 8080
```

After executing the above commands, the server will start and serve the REST API at `http://localhost:8080/api/v1/links`. You can test the installation by running `curl http://localhost:8080/health` which should return a `200 OK` status with a JSON health report.

## 安装要求

The following table lists all mandatory and optional dependencies required for building, running, and developing Fajiabi Score Hub. Please review each item carefully before proceeding with installation.

| 依赖 | 必需 | 说明 |
|------|------|------|
| Go 1.21+ | 是 | Core compiler and runtime; required for building the binary and running the server. |
| Git 2.30+ | 是 | Needed to clone the repository and manage version control during development. |
| Make 4.0+ | 否 | Optional but recommended for using the provided Makefile with common tasks (build, test, clean). |
| Docker 24.0+ | 否 | Required only if you intend to build and run the containerized version. |
| Docker Compose 2.20+ | 否 | Needed for multi-container setups (e.g., with Prometheus sidecar). |
| curl / wget | 否 | Useful for manual API testing and health checks from the command line. |
| jq 1.6+ | 否 | Recommended for parsing JSON responses in shell scripts and debugging. |
| Prometheus 2.45+ | 否 | Only if you wish to scrape and visualize the exported metrics. |
| Node.js 18+ | 否 | Required only for frontend development if you modify the static site templates. |
| Yarn 1.22+ | 否 | Package manager for frontend assets; used in conjunction with Node.js. |

## 文档导航

The table below provides a high-level roadmap to the project documentation, organized by knowledge domain. Each row describes a documentation layer, the corresponding directory or file, and the primary questions it answers for different user personas.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| User Guide | `docs/user-guide/` | How do I configure new links? How do I group them by category? What does the JSON API response look like? |
| Administrator Manual | `docs/admin/` | How do I set up health check intervals? How do I enable Prometheus metrics? How do I rotate logs? |
| Developer Reference | `docs/dev/` | What is the internal architecture? How do I add a new filter or middleware? How do I run unit and integration tests? |
| Deployment Guide | `docs/deployment/` | How do I deploy to Kubernetes? How do I use the official Docker image? What environment variables are supported? |
| API Specification | `docs/api/openapi.yaml` | What are all available endpoints? What request/response schemas are defined? Which error codes can I expect? |
| Configuration Schema | `configs/schema.yaml` | What are all configuration keys? Which values are valid? What are the default fallback values? |

## 资源列表

This section contains the complete and unmodified list of external resource URLs as provided by the user. Each URL is presented exactly in its original form without any additions, modifications, or protocol corrections. These links are intended for reference and integration purposes within the Fajiabi Score Hub ecosystem.

### 比分与赛事数据源

<code>fajiabifen.net.cn</code>

<code>bingdaochaobifen.net.cn</code>

<code>xijiabifen.cn</code>

<code>fajiajifenbang.cn</code>

<code>bingdaochaobisaijieguo.net.cn</code>

<code>yingchaobifen.cn</code>

## 项目结构

The source tree follows a standard Go project layout with clear separation of concerns. Below is the ASCII directory tree showing the primary directories and their roles. Each line includes a short comment describing the purpose of that directory or file.

```
fajiabi-score-hub/
├── cmd/                                 # Entry points for the application
│   └── server/                          # Main server binary entry
│       └── main.go                      # Initializes config, logger, and starts HTTP server
├── internal/                            # Private application code (not imported externally)
│   ├── config/                          # Configuration parsing and validation logic
│   │   ├── loader.go                    # Reads YAML and environment variables
│   │   └── schema.go                    # Defines configuration structs and defaults
│   ├── health/                          # Health check subsystem
│   │   ├── checker.go                   # Performs HTTP pings on registered URLs
│   │   └── scheduler.go                 # Manages cron-based periodic checking
│   ├── api/                             # RESTful API handlers and routing
│   │   ├── router.go                    # Registers all endpoints and middlewares
│   │   ├── links.go                     # GET /api/v1/links handler with filtering
│   │   └── status.go                    # GET /health and /metrics endpoints
│   ├── storage/                         # In-memory and file-based link registry
│   │   ├── registry.go                  # Main link store with CRUD operations
│   │   └── loader.go                    # Loads links from YAML or JSON files
│   └── logger/                          # Structured logging with rotation support
│       ├── logger.go                    # Wrapper over slog with custom formatting
│       └── rotation.go                  # File rotation for persistent logs
├── pkg/                                 # Public reusable libraries (can be imported by others)
│   └── utils/                           # Utility functions (string, time, HTTP helpers)
│       ├── retry.go                     # Exponential backoff retry logic
│       └── validator.go                 # URL and category validation helpers
├── configs/                             # Configuration files for different environments
│   ├── default.yaml                     # Base configuration with example links
│   ├── staging.yaml                     # Overrides for staging environment
│   └── production.yaml                  # Production-specific settings (metrics enabled)
├── docs/                                # All documentation (user, admin, dev, deployment)
│   ├── user-guide/                      # End-user tutorials and FAQ
│   ├── admin/                           # Operational runbooks and troubleshooting
│   ├── dev/                             # Code contribution guidelines and architecture
│   └── deployment/                      # Kubernetes, Docker, and cloud deployment notes
├── scripts/                             # Helper scripts for development and CI/CD
│   ├── test.sh                          # Runs all unit and integration tests
│   ├── lint.sh                          # Executes golangci-lint with project rules
│   └── build-static.sh                  # Generates static HTML from the link registry
├── web/                                 # Frontend assets for static site generation
│   ├── templates/                       # Go html/template files for dashboard rendering
│   └── static/                          # CSS, JavaScript, and image assets
├── deployments/                         # Deployment manifests for container orchestration
│   ├── docker/                          # Dockerfile and .dockerignore
│   └── kubernetes/                      # K8s manifests (deployment, service, ingress)
├── go.mod                               # Go module definitions and dependencies
├── go.sum                               # Checksums for dependency integrity
├── Makefile                             # Common development tasks (build, test, run, clean)
└── README.md                            # This file – the main project overview
```

## 贡献指南

We welcome contributions from the community, whether you are fixing a bug, improving documentation, or proposing a new feature. Please follow these concrete steps to ensure a smooth collaboration process.

1. **Fork the Repository and Set Up Local Environment** – Navigate to the official GitHub repository and click the "Fork" button. Then clone your fork locally, set the upstream remote to the original repository, and run `make setup` to install pre-commit hooks and development tools.

2. **Pick an Issue or Propose a Change** – Browse the existing GitHub Issues labeled "good first issue" or "help wanted". If you have a new idea, open a discussion thread first to align with maintainers. For significant changes, create a design document in the `docs/dev/` directory.

3. **Write Tests and Update Documentation** – Every code change must include corresponding unit tests under the `internal/` package with at least 80% coverage. Update the relevant documentation files, including this README if needed. Use `make test` to run the full test suite locally.

4. **Submit a Pull Request with Clear Description** – Push your changes to a feature branch with a descriptive name (e.g., `feature/add-timeout-config`). Open a pull request against the `main` branch, fill in the PR template completely, and reference any related issues. Ensure all CI checks (lint, test, build) pass.

5. **Participate in Code Review** – Respond to reviewer comments promptly, make requested adjustments, and keep the PR up-to-date with the latest `main` branch. Once at least two maintainers approve and all checks are green, your PR will be merged.

## 常见问题

**Q1: How do I add a new external score URL to the hub without restarting the server?**

A1: Fajiabi Score Hub supports hot-reload of the link registry. You can edit the YAML configuration file specified in the `--config` flag (default: `configs/default.yaml`). The server watches this file for changes and reloads the registry automatically within 10 seconds. No restart is required. If you prefer manual reload, send a `POST /api/v1/reload` request with an admin token (if configured).

**Q2: What should I do if a configured external URL becomes permanently unavailable?**

A2: The built-in health checker will mark the URL as "unhealthy" after three consecutive failed pings. The API response includes an `available` field for each link. You can either remove the URL from the configuration file (which triggers hot-reload) or set a `retry_count` parameter to allow more attempts before marking it as dead. For monitoring, Prometheus exports a `fajiabi_link_up{url="..."}` metric that you can alert on.

**Q3: Can I run Fajiabi Score Hub behind a corporate proxy or firewall?**

A3: Yes. The server respects the standard `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` environment variables. Set these before starting the binary or include them in your Docker Compose environment section. The health checker and all outgoing HTTP requests will route through the specified proxy. Additionally, you can configure a custom `timeout` per link in the YAML configuration to accommodate slower corporate networks.

## 许可证

This project is licensed under the terms of the MIT License. See the `LICENSE` file in the repository root for the full text. In summary, you are free to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, subject to the condition that the original copyright notice and permission notice appear in all copies or substantial portions of the software. The software is provided "as is", without warranty of any kind.

> 外链数量: 6 | 生成时间: 2026-07-22 18:18:53
