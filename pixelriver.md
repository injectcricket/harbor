# Bifeng Resource Gateway

Bifeng Resource Gateway is a lightweight, high-performance technical resource aggregation and navigation system designed for developers, researchers, and IT operations teams who need to rapidly access domain-specific information sources. The project serves as a structured gateway to curated external resources, providing a unified entry point for retrieving real-time data from multiple specialized web endpoints without the overhead of full-page scraping or browser automation.

The system targets users who require programmatic access to region-specific network resources, competitive result feeds, and ranking datasets. By offering a consistent interface over heterogeneous external sources, Bifeng Resource Gateway reduces integration complexity and improves data acquisition reliability. The project is suitable for integration into monitoring dashboards, data pipelines, alerting systems, and analytical workflows that depend on periodically refreshed external content.

## 功能概览

- **Unified Resource Proxy** – Transparently forwards requests to configured upstream endpoints with support for custom headers and timeout policies.

- **Configurable Polling Scheduler** – Built-in cron-style scheduler for periodic resource checks and content freshness validation.

- **Response Normalization Pipeline** – Applies configurable transformations (JSON-to-JSON, HTML-to-text, regex extraction) to standardize outputs from diverse sources.

- **Health and Latency Monitoring** – Continuously tracks upstream availability, response time, and error rates, exposing metrics via a status endpoint.

- **Access Control Layer** – Supports API key authentication and IP-based allowlisting for production deployments.

- **Cache-Aside Strategy** – Configurable local cache with TTL to reduce load on upstream resources and improve response times.

- **Structured Logging and Auditing** – Outputs JSON-formatted logs with request IDs, source tags, and timing information for operational visibility.

- **Hot-Reload Configuration** – Allows dynamic updates to resource endpoints and polling intervals without service restart.

## 应用场景

- **Regional Network Status Dashboards** – Operations teams can aggregate availability data from multiple local domain endpoints into a single monitoring panel, receiving alerts when any resource becomes unreachable.

- **Competitive Results Aggregation** – Sports analytics platforms can periodically fetch result and ranking data from specialized sources to feed into predictive models or real-time leaderboards.

- **Content Curation for Research** – Academic researchers studying regional internet infrastructure can use the gateway to collect historical response data and latency metrics for longitudinal analysis.

- **DevOps Integration Pipelines** – CI/CD workflows can query the gateway to retrieve external configuration parameters or environment-specific metadata during deployment stages.

- **Internal Developer Portals** – Engineering teams can embed the gateway as a backend service to expose curated external data to internal frontend applications without exposing raw upstream URLs.

## 快速开始

Clone the repository, install dependencies, and start the gateway service with default configuration.

```bash
git clone https://github.com/bifeng-resource/gateway.git
cd gateway
pip install -r requirements.txt
cp config.example.yaml config.yaml
python gateway.py --config config.yaml
```

The service will listen on port 8080 by default. Send a test request to verify operation:

```bash
curl http://localhost:8080/health
```

To query a specific resource endpoint, use the proxy path:

```bash
curl http://localhost:8080/proxy/bifenguanwang.net.cn
```

## 安装要求

The following dependencies are required for installation and runtime operation. All listed packages are available via PyPI and system package managers.

| Dependency | Required Version | Description |
|------------|------------------|-------------|
| Python | 3.10 or higher | Core runtime interpreter |
| aiohttp | 3.9.0 or higher | Asynchronous HTTP client and server framework |
| PyYAML | 6.0 or higher | Configuration file parsing and serialization |
| schedule | 1.2.0 or higher | In-process cron-style job scheduling |
| uvloop | 0.19.0 or higher | High-performance event loop replacement for asyncio |
| cachetools | 5.3.0 or higher | TTL-based in-memory caching utilities |
| python-json-logger | 2.0.7 or higher | Structured JSON log output formatter |
| pytest | 7.4.0 or higher | Test framework (development only) |
| black | 23.11.0 or higher | Code formatter (development only) |

## 文档导航

The project documentation is organized into the following layers, each addressing a distinct audience and set of concerns.

| Layer | Directory | Questions Answered |
|-------|-----------|-------------------|
| User Guide | docs/user/ | How do I configure upstream endpoints? What caching policies are available? How do I interpret health metrics? |
| API Reference | docs/api/ | What are all exposed endpoints, their request formats, and response schemas? Which status codes are returned? |
| Deployment Guide | docs/deploy/ | How do I deploy behind a reverse proxy? What are the recommended container configurations? How to set up TLS termination? |
| Development Guide | docs/dev/ | How do I add a new response transformer? How to extend the scheduler with custom triggers? What are the coding conventions? |
| Operations Handbook | docs/ops/ | How to monitor production instances? What log levels are available? How to perform disaster recovery and database rollback? |

## 资源列表

The following external resources are curated and exposed through the gateway. Each entry is presented exactly as provided by the upstream registration authority.

### Primary Domain Resources

- <code>bifenguanwang.net.cn</code>
- <code>bifenguanfang.net.cn</code>
- <code>bifenguanwang.cn</code>
- <code>bifenguanwang.org.cn</code>

### Regional Sports Data Sources

- <code>xijiasaicheng.org.cn</code>
- <code>ruidianchaobisaijieguo.org.cn</code>
- <code>ruidianchaojifenbang.org.cn</code>

## 项目结构

The repository follows a modular layout to separate core logic, configuration, utilities, and testing artifacts.

```
gateway/
├── gateway.py                  # Main entry point: argument parsing, app factory
├── config.yaml                 # User-editable configuration: endpoints, schedules, cache
├── config.example.yaml         # Template with all available parameters documented
├── requirements.txt            # Production and development dependency pins
├── Dockerfile                  # Multi-stage container build definition
├── docker-compose.yml          # Local orchestration with redis and prometheus
├── src/
│   ├── core/                   # Core abstractions: resource, transformer, cache
│   │   ├── resource.py         # Resource class with URL, method, headers, timeout
│   │   ├── transformer.py      # Base transformer and built-in html-to-text, jsonpath
│   │   └── cache.py            # TTL cache wrapper with hit/miss counters
│   ├── scheduler/              # Scheduling and polling orchestration
│   │   ├── manager.py          # Manages job registration and lifecycle
│   │   └── registry.py         # In-memory job store with persistence hooks
│   ├── proxy/                  # HTTP proxy and routing logic
│   │   ├── handler.py          # Request handler with error recovery and retries
│   │   └── middleware.py       # Auth, logging, CORS, and rate limiting
│   ├── monitor/                # Health checks, metrics aggregation
│   │   ├── health.py           # Liveness and readiness probes
│   │   └── metrics.py          # Prometheus-compatible counter and gauge exports
│   └── utils/                  # Shared utilities
│       ├── logger.py           # Structured logger with context injection
│       └── validators.py       # URL validation, schema checks, type coercion
├── tests/
│   ├── unit/                   # Isolated unit tests for each module
│   │   ├── test_resource.py
│   │   ├── test_cache.py
│   │   └── test_transformer.py
│   └── integration/            # End-to-end tests with mock upstream servers
│       └── test_proxy_flow.py
├── docs/
│   ├── user/                   # User-facing guides and configuration walkthrough
│   ├── api/                    # Auto-generated OpenAPI specification and manual additions
│   ├── deploy/                 # Deployment scenarios: k8s, systemd, cloud-specific
│   └── dev/                    # Development setup, contribution workflow
└── scripts/
    ├── setup_dev.sh            # Installs pre-commit hooks and dev dependencies
    └── run_tests.sh            # Runs all tests with coverage report
```

## 贡献指南

Contributions to Bifeng Resource Gateway are welcomed from the community. All submissions must follow the established coding standards and pass the automated verification pipeline.

1. Fork the repository and create a feature branch from `main` with a descriptive name such as `feature/add-jsonpath-transformer` or `fix/timeout-retry-logic`. Ensure your branch is rebased on the latest upstream `main` before opening a pull request.

2. Write or update unit tests for any new functionality or bug fixes. All tests must pass locally with `pytest tests/` and achieve at least 85% code coverage for modified modules. Include integration tests if your change affects the proxy or scheduler behavior.

3. Update the relevant documentation under the `docs/` directory. For new configuration parameters, add entries to `config.example.yaml` and describe them in the user guide. For API changes, update the OpenAPI specification and the corresponding reference page.

4. Run the code formatter `black` and linter `flake8` over your changes. Ensure no warnings or formatting violations are introduced. Commit messages should follow the conventional commits format with a clear subject and a detailed body explaining the rationale.

5. Submit a pull request with a concise description of the change, referencing any related issues. The maintainers will review the submission within five business days and may request additional modifications or clarifications before merging.

## 常见问题

**Q: How does the gateway handle upstream timeouts or connection failures?**

A: The gateway applies a configurable per-resource timeout (default 10 seconds) and a retry policy with exponential backoff (max 3 retries). If all attempts fail, the gateway returns a 502 Bad Gateway response with a structured error payload containing the upstream URL and the failure reason. The health monitoring subsystem will mark the resource as degraded and emit a corresponding log event.

**Q: Can the gateway operate without an external cache backend like Redis?**

A: Yes. By default, the gateway uses an in-memory TTL cache implemented via the `cachetools` library. This is suitable for single-instance deployments and low-to-medium traffic volumes. For high-availability or multi-instance deployments, Redis can be configured as the cache backend by setting `cache.backend: redis` in the configuration file and providing the appropriate connection parameters.

**Q: How often are the upstream resources polled, and can the schedule be customized per resource?**

A: The polling schedule is defined per resource in the configuration file using standard cron expressions (e.g., `*/5 * * * *` for every 5 minutes). Each resource entry can specify its own schedule independently, allowing fine-grained control. If no schedule is provided, the resource defaults to the global `default_poll_interval` set at the root of the configuration.

## 许可证

This project is licensed under the terms of the MIT License. See the LICENSE file in the repository root for full details.

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:34
