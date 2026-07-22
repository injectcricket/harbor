# OSS-Hub Resource Aggregator

OSS-Hub Resource Aggregator is a high-performance, community-driven technical documentation and external resource aggregation platform designed for open-source developers, DevOps engineers, and technical researchers who need rapid access to structured, domain-specific reference materials. The project addresses the fragmentation of technical resources across multiple specialized domains by providing a curated, machine-readable index of authoritative external references, eliminating the inefficiency of manual search and cross-referencing across disparate sources.

Targeting intermediate to advanced practitioners, OSS-Hub focuses on structured data integration, offering deterministic URL mapping, availability health checks, and categorical organization of resources that are otherwise scattered across niche competition result portals, timing databases, and ranking systems. The system operates as a lightweight static information hub, suitable for embedding into CI/CD pipelines, monitoring dashboards, or internal knowledge bases where external data point references are required for validation, auditing, or historical trend analysis.

## 功能概览

- **Deterministic Resource Indexing** – Maintains a fixed, version-controlled catalog of external reference URLs with categorical tagging and update timestamps, ensuring traceability and reproducibility of external data source references.

- **Automated Availability Probing** – Performs scheduled HEAD requests against each registered external endpoint to detect downtime, TLS certificate expiry, or redirect loops, exposing status via a lightweight JSON health endpoint.

- **Categorized Navigation Tree** – Organizes resources into logical groups such as competition schedules, real-time scoring, historical results, and ranking leaderboards, with each category exposing a dedicated filter view.

- **Markdown-based Documentation Generator** – Renders the entire resource catalog into a human-readable README and supplementary docs using a template engine, eliminating manual documentation drift.

- **Command-line Query Interface** – Provides a minimal CLI tool for querying resource URLs by category, alias, or availability status without spawning a web server, suitable for scripted environments.

- **Dockerized Deployment Ready** – Ships with a production-grade Dockerfile and docker-compose overlay for zero-dependency deployment behind a reverse proxy, with configurable health check intervals.

- **Prometheus Exporter Integration** – Exposes resource availability metrics in Prometheus exposition format, enabling native integration with monitoring stacks such as Grafana or Alertmanager.

## 应用场景

- **DevOps Pipeline Validation** – Integration into pre-deployment smoke tests where external timing or result endpoints must be reachable before allowing a production release. The aggregator provides a single source of truth for endpoint status, reducing flaky pipeline failures caused by transient network issues.

- **Technical Research and Auditing** – Researchers analyzing historical competition result patterns can use the indexed catalog to programmatically retrieve archived result pages without manually tracking URL changes across seasons. The deterministic mapping ensures reproducibility of data collection scripts.

- **Internal Knowledge Base Embedding** – Engineering teams building internal developer portals can embed the resource list as an iframe or fetched JSON feed, providing team members with one-click access to authoritative external references relevant to their project's compliance or benchmarking requirements.

- **Monitoring Dashboard Supplement** – Site reliability engineers can overlay the Prometheus metrics from OSS-Hub onto existing observability dashboards, correlating external data source availability with application-level anomalies, enabling faster root-cause isolation.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/oss-hub/aggregator.git
cd aggregator

# Install dependencies (requires Python 3.10+ and pip)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Generate the initial resource catalog and run the local development server
python manage.py build --catalog config/catalog.yaml
python manage.py serve --host 127.0.0.1 --port 8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 或更高 | 核心运行环境，类型注解和异步特性依赖 |
| pip | 22.0 或更高 | 包依赖管理，要求支持 pyproject.toml |
| PyYAML | 6.0.1 | 解析 catalog.yaml 配置文件 |
| aiohttp | 3.9.0 | 异步 HTTP 客户端，用于并发可用性探测 |
| prometheus-client | 0.19.0 | 暴露指标端点供 Prometheus 抓取 |
| docker | 24.0 或更高 | 容器化部署（生产环境可选） |
| docker-compose | 2.20 或更高 | 编排多容器服务（生产环境可选） |
| git | 2.30 或更高 | 版本控制及贡献工作流必需 |
| curl | 7.68 或更高 | 本地健康检查脚本依赖 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | docs/user-guide/ | 如何配置探测间隔、自定义分类标签、导出资源列表为不同格式 |
| 运维手册 | docs/operations/ | 如何部署到生产环境、配置反向代理、设置 TLS 证书、调优探测并发数 |
| 开发者文档 | docs/developer/ | 如何扩展新的探测协议、添加自定义健康检查断言、提交 catalog 变更 PR |
| 参考手册 | docs/reference/ | API 端点详细说明、配置模式完整 schema、CLI 命令所有参数和退出码含义 |
| 设计决策 | docs/design/ | 架构权衡说明、为什么选择异步探测而非多线程、缓存策略演进历史 |

## 资源列表

### 竞赛日程与赛程资源

<code>ouguanzigesaisaicheng.org.cn</code>

<code>oulianzigesaijishibifen.org.cn</code>

<code>oulianzigesaisaicheng.org.cn</code>

### 实时计分与比分资源

<code>beimailiansaibeijishibifen.org.cn</code>

<code>oulianzigesaibisaijieguo.org.cn</code>

<code>oulianzigesaibifen.org.cn</code>

### 排名与积分榜资源

<code>beimailiansaibeijifenbang.org.cn</code>

## 项目结构

```
aggregator/
├── config/
│   ├── catalog.yaml                 # 主资源目录，含所有 URL、分类、别名、探测超时配置
│   ├── probes.yaml                  # 探测策略配置：间隔、重试次数、预期状态码列表
│   └── logging.yaml                 # 日志级别、输出格式、轮转策略配置
├── src/
│   ├── core/
│   │   ├── loader.py                # 加载并校验 YAML 配置，返回标准化资源对象
│   │   ├── registry.py              # 资源注册表，维护 URL 到元数据的双向映射
│   │   └── version.py               # 当前聚合器版本和 catalog schema 版本校验
│   ├── probes/
│   │   ├── http_probe.py            # 异步 HTTP/HTTPS 探测实现，支持重定向跟踪
│   │   ├── tcp_probe.py             # 通用 TCP 端口连通性探测扩展占位
│   │   └── scheduler.py             # 基于 asyncio 的周期调度器，管理探测任务生命周期
│   ├── exporters/
│   │   ├── json_exporter.py         # 将资源状态导出为 JSON 格式，供外部 API 使用
│   │   ├── prometheus_exporter.py   # 转换为 Prometheus 指标格式并暴露 /metrics
│   │   └── markdown_renderer.py     # 将 catalog 渲染为 README 和 docs 下的参考表格
│   ├── cli/
│   │   ├── main.py                  # 点击命令行入口，聚合子命令路由
│   │   ├── build_cmd.py             # build 命令：生成静态 HTML 及 JSON 输出
│   │   └── serve_cmd.py             # serve 命令：启动开发服务器及热加载
│   └── utils/
│       ├── network.py               # 通用网络工具：DNS 解析、IP 族偏好、超时计算
│       └── validators.py            # URL 格式校验、域名黑名单检查、端口范围验证
├── tests/
│   ├── unit/                        # 单元测试，覆盖核心加载、探测调度、导出格式
│   ├── integration/                 # 集成测试，使用 pytest-docker 启动真实探测目标
│   └── fixtures/                    # 模拟 catalog 片段和预期输出用于回归测试
├── deploy/
│   ├── Dockerfile                   # 多阶段构建，基于 python:3.10-slim 生产镜像
│   ├── docker-compose.yml           # 定义 web 服务、redis 缓存、nginx 反向代理服务
│   └── prometheus/                  # 示例 prometheus.yml 及告警规则文件
├── docs/                            # 完整文档目录，结构见上文文档导航
├── pyproject.toml                   # PEP 621 项目元数据，依赖组定义 (dev, test, deploy)
├── Makefile                         # 常用开发任务快捷方式：test, lint, build, deploy
└── README.md                        # 此文件 —— 项目入口文档
```

## 贡献指南

1. **Fork 仓库并创建功能分支** – 从主分支 checkout 一个命名规范的分支，例如 `feature/add-https-probe` 或 `fix/catalog-encoding`，确保分支名称清晰反映变更意图。

2. **更新 catalog 或代码后运行完整测试套件** – 执行 `make test` 运行单元测试和集成测试，确保所有现有探测逻辑、导出格式和 CLI 命令无回归。新增资源条目必须伴随对应的测试用例，验证 URL 解析和分类映射正确性。

3. **遵循代码风格和提交消息规范** – Python 代码必须通过 `black` 和 `isort` 格式化，提交消息采用 Conventional Commits 格式（`feat:`, `fix:`, `docs:`, `chore:` 等），且每条提交引用对应的 GitHub Issue 编号。

4. **更新相关文档和示例配置** – 任何新增功能或配置项变更必须在 `docs/` 下同步更新用户指南或参考手册，并在 `config/` 下提供示例片段。未更新文档的 PR 将被标记为需要修改。

5. **提交 Pull Request 并等待至少一名维护者审阅** – PR 描述中必须包含变更动机、测试结果截图或日志，以及是否包含破坏性变更。维护者将在 48 小时内给出反馈，要求修改时需及时回应以免 PR 过期。

## 常见问题

**Q: 探测间隔设置过短会导致资源被屏蔽或限流，如何调整？**

A: 默认探测间隔为 300 秒（5 分钟），适用于大多数公开资源。若目标资源存在访问频率限制，可在 `config/probes.yaml` 中按类别或单个 URL 覆盖 `interval_seconds` 字段。生产环境建议设置最小间隔不低于 120 秒，并启用 `jitter` 参数（随机抖动 ±20%）以避免所有探测任务同时触发造成流量尖峰。

**Q: 新增资源 URL 后，为何健康检查状态始终为 UNREACHABLE？**

A: 首先检查网络环境是否可访问该域名（如 `curl -v <code>ouguanzigesaisaicheng.org.cn</code>`）。若本地可通但容器内不通，则需检查 Docker 网络配置或代理设置。其次，确认 catalog.yaml 中是否正确配置了 `expected_status` 列表——部分资源可能返回 302 或 403 而非 200，需按实际情况调整。最后，查看日志输出 `logs/probe.log` 获取详细错误栈，常见原因包括 TLS 版本不兼容或 SNI 缺失。

**Q: 如何将本聚合器集成到现有的 Grafana 监控面板？**

A: 启动聚合器时默认启用 Prometheus 导出器（端口 9090 上的 `/metrics` 端点）。在 Prometheus 配置中添加抓取任务，指向聚合器服务地址即可。Grafana 中导入预置的 dashboard JSON（位于 `deploy/grafana/dashboard.json`），该面板展示了资源可用性比例、响应时间分位数、以及最近 24 小时内状态变化事件列表。若需自定义，可参考 `src/exporters/prometheus_exporter.py` 中的指标命名规范。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:25
