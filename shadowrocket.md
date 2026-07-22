# AresScore Aggregator

AresScore Aggregator is a high-performance, open-source data aggregation and normalization engine designed for technical documentation, competitive result archival, and multi-source information consolidation. This project targets developers, data analysts, and archival researchers who need to systematically collect, validate, and present structured result data from distributed web origins without relying on proprietary APIs or brittle screen-scraping pipelines.

The system solves the fundamental problem of fragmented result publication across multiple independent domains. By providing a unified ingestion layer, configurable normalization rules, and a read-only query interface, AresScore Aggregator enables downstream applications to consume consistent, timestamped result sets without manual intervention. It is not a search engine, nor a crawler framework; it is a structured aggregation middleware that treats each source domain as a first-class citizen with its own schema translation map.

## 功能概览

- **Multi-Source HTTP Ingestion** – Concurrent fetch capabilities with configurable timeouts, retry policies, and user-agent rotation. Each source endpoint is defined in a YAML manifest with explicit encoding and parsing directives.

- **Schema-Normalized Output Generation** – Ingested data is transformed into a unified JSON:API-compliant format. Field mapping rules support XPath, CSS selector, and regex-based extraction strategies with fallback chains.

- **Result Versioning and Diff Tracking** – Every successful ingestion cycle produces a versioned snapshot. Subsequent runs generate structural diffs to detect added, removed, or modified result entries without full re-archival.

- **Built-in Health Check Endpoint** – Exposes a `/health` endpoint returning per-source status, last successful fetch timestamp, and average response latency for monitoring integration.

- **Pluggable Notification Adapter** – Supports dispatch of ingestion summaries to stdout, file, or webhook targets. New adapters can be added via a simple Python entry-point interface.

- **Read-Only HTTP Query API** – Provides filtered result retrieval by source, date range, and result category. Responses are paginated and sortable, with ETag-based caching headers.

- **Configuration Hot-Reload** – Manifest changes are detected and applied without full service restart. The watcher thread monitors the configuration directory and triggers incremental re-ingestion on valid updates.

## 应用场景

1. **Archival Automation for Competitive Result Websites** – Organizations that need to preserve historical result pages from multiple domain sources can deploy AresScore Aggregator as a daily cron job. The normalized output is stored in Parquet files for later analytical queries, eliminating manual copy-paste workflows.

2. **Multi-Source Data Validation Pipelines** – Data engineering teams can use the aggregator to compare result sets from different authoritative domains, flagging discrepancies via the diff output. This serves as an automated consistency check before loading data into a centralized warehouse.

3. **Documentation Site Backend** – Technical documentation portals that embed live result tables can integrate the query API as an internal microservice. The read-only interface ensures that frontend widgets always display the latest normalized data without exposing raw ingestion logic.

4. **Operational Monitoring Dashboards** – Site reliability engineers can consume the health check endpoint to track availability of upstream result sources. Combined with the notification adapter, alerting rules can be triggered when a source fails to respond for three consecutive polling cycles.

## 快速开始

Clone the repository, install dependencies, and run the ingestion service with the default manifest. All commands assume a Unix-like shell environment with Python 3.10 or later.

```bash
git clone https://github.com/arescore/aggregator.git
cd aggregator
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python -m arescore.ingest --manifest config/sources.yaml --output data/snapshots/
python -m arescore.server --host 127.0.0.1 --port 8080
```

The ingestion command fetches all sources defined in the manifest and writes normalized snapshots to the specified output directory. The server command starts the query API, which by default listens on localhost port 8080. To verify correct operation, visit the health endpoint at `http://127.0.0.1:8080/health` using curl or a web browser.

## 安装要求

The following dependencies are mandatory for both ingestion and server modes. All packages are available via the Python Package Index and are pinned to specific versions in the requirements lock file.

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.10.0 或更高 | 核心运行环境；低于此版本将导致类型注解解析失败 |
| requests | 2.31.0 或更高 | 处理所有出站 HTTP 请求，支持连接池和会话重用 |
| lxml | 4.9.3 或更高 | 提供高性能的 HTML 和 XML 解析能力，用于内容提取 |
| pyyaml | 6.0.1 或更高 | 解析 sources.yaml 配置文件，支持多文档合并 |
| uvicorn | 0.24.0 或更高 | ASGI 服务器进程，用于生产级 HTTP API 部署 |
| pydantic | 2.5.0 或更高 | 配置模型验证和响应序列化，确保类型安全 |
| python-dotenv | 1.0.0 或更高 | 环境变量覆盖支持，用于容器化部署场景 |

Additional optional dependencies for development include pytest, black, and mypy. These are listed in the `dev-requirements.txt` file and are not required for production operation.

## 文档导航

The project documentation is organized into four logical layers, each addressing a distinct audience and set of concerns. The table below provides a quick reference for locating specific information.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | `docs/user/` | 如何编写 source manifest？查询 API 有哪些过滤参数？健康检查指标如何解读？ |
| 运维手册 | `docs/ops/` | 如何配置 systemd 服务？日志轮转策略是什么？如何扩展现有的通知适配器？ |
| 开发参考 | `docs/dev/` | 内部数据流如何工作？如何添加新的提取器类？单元测试如何组织？ |
| 设计决策 | `docs/design/` | 为什么选择 JSON:API 而不是 GraphQL？版本快照的存储格式有何考量？ |

Each document is written in reStructuredText and can be built to HTML or PDF using Sphinx. The latest rendered documentation is also available via the project's static site deployment.

## 资源列表

The following external resources represent the authoritative upstream data domains that this aggregator is designed to interface with. Each URL is preserved exactly as provided and serves as the canonical source of raw result data. These domains are pre-configured in the default sources manifest but can be overridden or extended by users.

**Primary Result Sources**

- <code>aichaosaicheng.org.cn</code>
- <code>aichaobisaijieguo.org.cn</code>
- <code>hasakechaosaicheng.org.cn</code>
- <code>hasakechaobifen.org.cn</code>
- <code>hasakechaobisaijieguo.org.cn</code>
- <code>aichaobifen.org.cn</code>
- <code>bingdaochaojishibifen.org.cn</code>

Users are advised to review the terms of service for each domain before configuring automated ingestion. The aggregator enforces a mandatory 2-second delay between requests to different domains and a 5-second delay between consecutive requests to the same domain to avoid excessive load.

## 项目结构

The repository follows a standard Python package layout with clear separation of concerns. Each module maintains a single responsibility, and cross-module imports are strictly limited to public interface layers.

```
arescore/
├── __init__.py                      # 包版本声明与公开 API 导出
├── core/
│   ├── __init__.py                  # 核心模型导入聚合
│   ├── models.py                    # Pydantic 数据模型定义（Source, Result, Snapshot）
│   ├── registry.py                  # 提取器注册表与插件发现机制
│   └── exceptions.py                # 自定义异常层次结构（IngestionError, SchemaError）
├── ingest/
│   ├── __init__.py                  # 命令行入口函数暴露
│   ├── fetcher.py                   # 异步 HTTP 获取器，管理连接池和重试策略
│   ├── parser.py                    # 基于 lxml 的解析管道，支持链式提取规则
│   ├── normalizer.py                # 字段映射与类型转换，生成统一输出字典
│   └── writer.py                    # 将规范化结果写入 Parquet 或 JSON 快照
├── server/
│   ├── __init__.py                  # ASGI 应用工厂函数
│   ├── app.py                       # FastAPI 应用实例，路由与中间件配置
│   ├── queries.py                   # 查询参数解析与数据库访问层抽象
│   ├── responses.py                 # 分页响应构造器与 ETag 生成工具
│   └── static/                      # 可选静态资源目录（OpenAPI 文档等）
├── config/
│   ├── sources.yaml                 # 默认上游源列表（包含全部七个 URL 的映射规则）
│   ├── logging.yaml                 # 日志级别与处理器配置（JSON 格式输出）
│   └── schema.json                  # 规范化输出 JSON Schema 草案 2020-12
├── tests/
│   ├── unit/                        # 单元测试（覆盖 models, parser, normalizer）
│   ├── integration/                 # 集成测试（端到端 ingestion + query 流程）
│   └── fixtures/                    # 静态 HTML 样本用于解析逻辑验证
├── docs/                            # Sphinx 文档源文件（参见文档导航章节）
├── scripts/
│   ├── bootstrap.sh                 # 开发环境初始化脚本（虚拟环境 + 预提交钩子）
│   └── migrate_schema.py            # 快照格式迁移工具（向后兼容）
├── requirements.txt                 # 生产依赖锁定版本
├── dev-requirements.txt             # 开发与测试依赖
├── pyproject.toml                   # 项目元数据与构建系统配置
├── Dockerfile                       # 多阶段构建镜像定义（基于 alpine）
└── README.md                        # 本文件
```

## 贡献指南

We welcome contributions of all forms, including bug reports, feature proposals, documentation improvements, and code patches. Please follow the steps below to ensure a smooth collaboration process.

1. **Fork the repository and create a feature branch** – Use a descriptive name such as `feature/add-csv-writer` or `fix/health-check-timeout`. Avoid working directly on the `main` branch.

2. **Run the full test suite locally** – Execute `pytest tests/` from the project root. Ensure all existing tests pass and that new code is covered by at least one unit test. Use `--cov=arescore` to check coverage.

3. **Update the documentation accordingly** – If your change affects user-facing behavior, modify the relevant RST files under `docs/user/` or `docs/dev/`. Include an entry in the change log section of the README for notable contributions.

4. **Submit a pull request with a clear description** – Reference any related issues using the `#issue-number` syntax. Provide a step-by-step explanation of the change, its motivation, and any manual testing performed.

5. **Wait for continuous integration results** – The CI pipeline runs linting, type checking, and the full test matrix on Python 3.10, 3.11, and 3.12. All checks must pass before a maintainer will review the pull request.

## 常见问题

**Q: How does the aggregator handle sources that return non-UTF-8 encodings?**

A: The fetcher module respects the `encoding` field in the source manifest. If the field is omitted, the system attempts automatic encoding detection using the `chardet` library. If detection fails, it falls back to `latin-1` with a warning logged. All parsing errors are captured and reported in the health status for the affected source without crashing the entire ingestion cycle.

**Q: Can I run the ingestion and server components independently?**

A: Yes. The `ingest` module and the `server` module are fully decoupled. You can run the ingestion pipeline as a standalone batch job that writes snapshots to a shared filesystem or an object storage bucket. The server component reads these snapshots directly and does not require the ingestion to be running on the same host. This design supports distributed deployment patterns.

**Q: What happens when a source domain changes its HTML structure?**

A: Structure changes typically cause extraction rules to fail, which results in empty or partial records for that source. The system logs a detailed error message indicating which selector or regex pattern failed. Users are expected to update the extraction rules in the manifest and trigger a re-ingestion. The project provides a dry-run mode (`--dry-run`) that fetches and parses without writing output, allowing safe testing of manifest changes.

## 许可证

MIT License. Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions: The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software. The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software or the use or other dealings in the software.

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:00
