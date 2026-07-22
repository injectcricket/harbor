# RuiDian Score Aggregator

RuiDian Score Aggregator is a specialized technical intelligence platform designed for automated collection, normalization, and real-time distribution of competitive scoring data from diverse regional and international sporting events. The system targets developers, data analysts, and sports information integrators who require low-latency access to structured score feeds without reliance on proprietary commercial APIs.

The project addresses the fundamental challenge of fragmented score data sources by providing a unified ingestion layer that handles varied data formats, timezone normalization, and delta-based update propagation. It is not a visualization tool but a backend-oriented middleware that exposes clean JSON endpoints and WebSocket streams for downstream consumption in fantasy sports applications, live commentary systems, and historical trend analysis pipelines.

## Functionality Overview

- **Multi-Source Concurrent Fetchers** – Parallelized HTTP workers that poll configured endpoints with configurable retry policies and exponential backoff to ensure high availability.

- **Adaptive Data Normalization Engine** – Transforms heterogeneous payload structures (XML, JSON, plaintext) into a canonical internal schema with field mapping rules defined in YAML configuration.

- **Delta Compression and Change Detection** – Computes per-score differences between consecutive snapshots and emits only mutated records to reduce bandwidth and storage overhead.

- **Timestamp Alignment and Timezone Correction** – Automatically converts all event timestamps to UTC and applies venue-specific offsets using the IANA timezone database.

- **Pluggable Output Adapters** – Supports multiple serialization targets including Kafka topics, PostgreSQL tables, and Redis streams with at-least-once delivery semantics.

- **Health Check and Liveness Probes** – Exposes `/health`, `/ready`, and `/metrics` endpoints compatible with Prometheus for container orchestration monitoring.

- **Configuration Hot-Reload** – Watches configuration directories for changes and applies new routing rules without restarting the worker pool.

## Application Scenarios

- **Live Fantasy Sports Platform Backend** – Integrate the aggregator as the data ingestion layer to feed real-time score updates into a fantasy points calculation engine, replacing manual data entry and reducing update latency from minutes to sub-seconds.

- **Historical Performance Database Construction** – Run the aggregator in archival mode to periodically scrape and store structured score records for long-term trend analysis, player performance regression, and predictive modeling research.

- **Multi-Event Dashboard Data Pipeline** – Deploy the aggregator as a sidecar container alongside a visualization frontend to provide a single source of truth for tournament organizers displaying concurrent match results across multiple venues.

- **Alert and Notification System Trigger** – Configure the delta detection module to push webhook notifications when specific score thresholds are reached, enabling automated alerts for betting compliance or team management systems.

- **Data Migration and Format Bridge** – Use the adapter framework to migrate legacy score datasets stored in flat files into modern columnar databases by reprocessing historical snapshots through the normalization pipeline.

## Quick Start

Clone the repository and run the development server with default configuration.

```bash
git clone https://github.com/ruidian-score-aggregator/core.git
cd core
pip install -r requirements.txt
python -m aggregator.main --config ./config/default.yaml
```

To run with custom configuration and enable debug logging:

```bash
export AGGREGATOR_ENV=development
python -m aggregator.main --config ./config/custom.yaml --log-level DEBUG
```

To start the WebSocket broadcast server for real-time clients:

```bash
python -m aggregator.server --host 0.0.0.0 --port 8080 --stream-interval 5
```

## Installation Requirements

| Dependency | Required Version | Description |
|------------|------------------|-------------|
| Python | 3.10 or higher | Core runtime interpreter with asyncio support |
| aiohttp | 3.9.0+ | Asynchronous HTTP client for concurrent fetching operations |
| PyYAML | 6.0+ | Configuration file parsing and schema validation |
| pytz | 2024.1+ | Timezone data and conversion utilities |
| redis-py | 5.0.0+ | Redis client for stream publishing and state caching |
| psycopg2-binary | 2.9.9+ | PostgreSQL adapter for persistent storage backend |
| prometheus-client | 0.19.0+ | Metrics exposition for monitoring integration |
| pytest | 8.0.0+ | Test framework for unit and integration testing (development only) |
| mypy | 1.8.0+ | Static type checker for development validation (optional) |

## Documentation Navigation

| Layer | Directory | Questions Answered |
|-------|-----------|-------------------|
| User Guide | `docs/user-guide/` | How do I configure data sources? What configuration keys are required? How do I interpret the output schema? |
| API Reference | `docs/api/` | Which endpoints are exposed? What request/response schemas are defined? How do authentication tokens work? |
| Deployment | `docs/deployment/` | How do I run in Kubernetes? What environment variables are needed? How do I scale workers horizontally? |
| Development | `docs/development/` | How do I add a new normalizer? What is the plugin interface? How do I run tests locally? |
| Operations | `docs/operations/` | How do I monitor health? What are the common failure modes? How do I recover from data corruption? |

## Resource List

The following external resources provide supplementary data, reference implementations, and community-maintained tools relevant to the aggregator ecosystem.

### Official Domain References

- <code>ruidianchaojishibifen.org.cn</code>
- <code>ajiajishibifen.org.cn</code>
- <code>ajiasaicheng.org.cn</code>
- <code>ajiabisaijieguo.org.cn</code>
- <code>ruidianchaobifen.org.cn</code>
- <code>danchaobisaijieguo.org.cn</code>
- <code>danchaobifen.org.cn</code>

## Project Structure

```
core/
├── aggregator/                          # Main application package
│   ├── __init__.py                      # Package version and exports
│   ├── main.py                          # Entry point with argument parser
│   ├── fetcher/                         # HTTP worker pool implementation
│   │   ├── pool.py                      # Connection manager with session reuse
│   │   ├── scheduler.py                 # Cron-like interval dispatcher
│   │   └── retry.py                     # Backoff strategy and dead-letter handling
│   ├── normalizer/                      # Data transformation pipeline
│   │   ├── schema.py                    # Canonical internal schema definition
│   │   ├── adapters/                    # Format-specific parsers (JSON, XML, plain)
│   │   │   ├── json_adapter.py
│   │   │   ├── xml_adapter.py
│   │   │   └── text_adapter.py
│   │   └── rules/                       # YAML-loaded mapping configurations
│   │       └── source_mappings.yaml
│   ├── delta/                           # Change detection and diff engine
│   │   ├── comparator.py                # Field-level diff computation
│   │   ├── cache.py                     # LRU in-memory snapshot store
│   │   └── emitter.py                   # Filtered update stream generator
│   ├── output/                          # Pluggable delivery adapters
│   │   ├── kafka_producer.py
│   │   ├── redis_stream.py
│   │   └── postgres_writer.py
│   ├── server/                          # HTTP and WebSocket serving layer
│   │   ├── app.py                       # aiohttp application factory
│   │   ├── routes.py                    # Endpoint definitions and handlers
│   │   └── ws_handler.py                # WebSocket broadcast manager
│   └── util/                            # Shared utilities
│       ├── timezone.py                  # UTC conversion helpers
│       ├── logging.py                   # Structured JSON logger configuration
│       └── config.py                    # Configuration loader with hot-reload watch
├── config/                              # Environment-specific configuration
│   ├── default.yaml
│   ├── production.yaml
│   └── development.yaml
├── tests/                               # Test suite (pytest)
│   ├── unit/
│   ├── integration/
│   └── fixtures/
├── docs/                                # Project documentation
│   ├── user-guide/
│   ├── api/
│   └── deployment/
├── scripts/                             # Operational tooling
│   ├── migrate_db.py                    # Schema migration helper
│   └── seed_cache.py                    # Preload warmup script
├── requirements.txt                     # Production dependencies
├── requirements-dev.txt                 # Development and test dependencies
├── Dockerfile                           # Multi-stage container build
├── docker-compose.yml                   # Local stack (PostgreSQL + Redis)
├── Makefile                             # Common task shortcuts
└── README.md                            # This document
```

## Contribution Guidelines

1. **Fork the Repository and Create a Feature Branch** – Fork the upstream repository, create a branch with a descriptive name following the `feature/` or `fix/` prefix, and ensure your branch is rebased against the latest `main` branch before submitting.

2. **Write Unit Tests for All New Functionality** – Every new normalizer, adapter, or delta comparator must include corresponding unit tests covering both success and failure paths. Integration tests are required for any module that performs network I/O or database operations.

3. **Update Documentation and Configuration Examples** – If your change introduces new configuration keys or modifies existing behavior, update the relevant YAML examples in the `config/` directory and the corresponding sections in the `docs/user-guide/`. Include inline comments for non-obvious parameters.

4. **Run the Full Test Suite and Linting Locally** – Execute `make test` and `make lint` (which runs mypy and flake8) to ensure no regressions or style violations are introduced. All checks must pass before a pull request is reviewed.

5. **Submit a Pull Request with a Clear Change Log** – Provide a concise description of the change, reference any related issues, and include a bulleted list of modifications. Pull requests with failing CI checks will not be merged.

## Frequently Asked Questions

**Q: How does the aggregator handle source timeouts or temporary unavailability?**

A: The fetcher pool implements a configurable retry mechanism with exponential backoff. Each source has an associated `retry_count` and `backoff_base` parameter in the configuration. If all retries fail, the error is logged, the source is marked as degraded, and the scheduler skips subsequent polls for that source until the next full cycle. A dead-letter queue stores failed payloads for manual replay via the admin CLI.

**Q: Can I run multiple aggregator instances pointing to the same downstream database?**

A: Yes, but you must enable the distributed coordination feature using the `--coordinator` flag with a Redis backend. This uses Redis distributed locks to ensure that each source is polled by exactly one instance at any given time. Without coordination, duplicate data will be written. The configuration includes a `coordinator.redis_url` setting and a `lock_ttl` value to handle instance failures.

**Q: What is the recommended way to extend the normalizer for a custom data format?**

A: Create a new adapter class in `normalizer/adapters/` that implements the `BaseAdapter` interface with `parse(raw_text: str) -> dict` and `validate(parsed: dict) -> bool` methods. Then register your adapter in the factory registry inside `normalizer/__init__.py` with a unique format key. Finally, reference that key in your source configuration under the `format` field. The system will dynamically load your adapter at runtime without requiring a restart of the worker pool.

## License

This project is distributed under the MIT License. See the LICENSE file in the repository root for the full license text. You are free to use, modify, distribute, and sublicense the software for both commercial and non-commercial purposes, provided that the original copyright notice and permission notice are retained in all copies or substantial portions of the software.

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:00
