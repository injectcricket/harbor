# Project Sentinel Link

Project Sentinel Link is a curated, high-availability technical resource aggregation and external link management system designed for open-source developers, security researchers, and infrastructure operators. The project addresses the growing challenge of link rot, domain obsolescence, and fragmented documentation in the open-source ecosystem by providing a structured, version-controlled, and community-validated repository of external references. Unlike conventional bookmark managers or simple README listings, Sentinel Link implements a formal taxonomy for categorizing external resources, tracks historical domain transitions, and offers a lightweight validation framework to periodically check resource availability. The primary target audience includes maintainers of large-scale documentation portals, DevOps engineers constructing internal knowledge bases, and academic researchers who require persistent citation-grade external references. The project does not host or proxy any third-party content but instead serves as a deterministic index that can be integrated into CI/CD pipelines for automated link health checks.

## 功能概览

- **Hierarchical Resource Taxonomy** – Organize external URLs into a multi-level category system that supports filtering by domain authority, content type, and geographic origin.
- **Historical Domain Migration Tracking** – Record and display the transition history of related domains, enabling users to identify primary versus secondary or alias domains within a related set.
- **Automated Availability Probing** – Integrate with external monitoring hooks to periodically test each listed URL for HTTP status code, TLS certificate validity, and response time, flagging degraded endpoints.
- **Markdown-Centric Data Serialization** – Store all resource metadata and structural information directly in human-readable Markdown, eliminating the need for external databases or proprietary configuration formats.
- **Static Site Generation Compatibility** – Provide a data layer that can be consumed by static site generators such as Hugo, MkDocs, or Jekyll to produce fully navigable resource directories.
- **Versioned Change Logging** – Maintain an internal audit trail of all additions, removals, and modifications to the resource list, with timestamps and contributor attribution.
- **Community Validation Badging** – Allow contributors to vote on the reliability and relevance of each resource, producing a community-driven quality score displayed alongside each entry.
- **Command-Line Interface for Bulk Operations** – Include a set of Python-based scripts that enable batch import, export, and diff operations against the resource manifest.

## 应用场景

- **Documentation Portal Maintenance** – Technical writers and documentation engineers can use Sentinel Link as a backend data source to dynamically generate "Further Reading" or "External References" sections across hundreds of pages, ensuring all external links are uniformly managed and periodically verified without manual per-page edits.
- **Security Research Indexing** – Security analysts investigating threat intelligence or vulnerability databases can employ the project to maintain a curated list of official advisories, CVE feeds, and vendor security portals. The domain migration tracking feature is particularly valuable when monitoring organizations that frequently change their primary domain due to rebranding or regulatory requirements.
- **Academic Citation Management** – Researchers preparing systematic literature reviews or technical reports can leverage the project's deterministic URL formatting and historical tracking to produce citation appendices that remain stable over time. Even if a referenced domain becomes unavailable, the recorded history provides contextual fallback information.
- **Infrastructure Asset Inventory** – Site reliability engineers can extend the project to catalog internal tooling endpoints, API gateways, and dashboard URLs across multiple environments. The automated probing feature integrates with alerting systems to provide early warning of internal service endpoint changes.

## 快速开始

Clone the repository and initialize the local environment using the following commands. The project assumes a POSIX-compliant shell environment with Python 3.10 or later installed.

```bash
git clone https://github.com/sentinel-link/sentinel-link.git
cd sentinel-link
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m sentinel_link.cli validate --manifest manifest.yaml
```

The validation command will parse the manifest file, check the syntax and structural integrity of all resource entries, and output a summary report to the console. To run the complete availability probe across all listed resources, use the following command:

```bash
python -m sentinel_link.cli probe --concurrency 5 --timeout 10 --output report.json
```

## 安装要求

The following dependencies are strictly required for both development and production execution of the core validation and probing engines. All dependencies are available via the Python Package Index and are pinned to specific versions to ensure reproducibility. The table below lists each package, its minimum version, and a brief justification for its inclusion.

| 依赖 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10.0 或更高 | 核心解释器，3.10 及以上版本提供模式匹配和类型提示增强 |
| requests | 2.31.0 | 用于执行 HTTP/HTTPS 探测请求，处理重定向和 TLS 验证 |
| pyyaml | 6.0.1 | 解析 manifest.yaml 配置文件，支持复杂嵌套结构和锚点引用 |
| pydantic | 2.5.0 | 数据模型验证，用于确保资源条目符合预定义的 JSON Schema |
| click | 8.1.7 | 命令行界面框架，提供子命令分组和参数自动补全支持 |
| rich | 13.7.0 | 终端格式化输出，用于生成彩色表格和进度条，改善用户交互体验 |

## 文档导航

The project documentation is organized into four primary layers, each addressing a distinct audience and set of concerns. The following table maps each documentation layer to its corresponding directory path and lists the core questions it answers for the reader.

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | docs/user-guide/ | 如何安装、配置、运行基础命令；如何添加新的资源条目；如何生成自定义报告 |
| 贡献者手册 | docs/contributor/ | 代码风格规范、提交信息格式、PR 审查流程；如何添加新的探测协议支持 |
| 架构设计 | docs/architecture/ | 核心数据模型、模块边界、扩展点设计；外部依赖的抽象层设计决策 |
| API 参考 | docs/api/ | 公共函数签名、类层次结构、异常类型；配置文件的完整 YAML Schema 定义 |

## 资源列表

本项目的资源索引聚焦于一组经过初步验证的域名集合。这些域名共享相似的主题关联，涵盖多种顶级域和二级域变体。所有条目均按照用户提供的原始格式原样收录，未做任何规范化处理。各域名按其注册性质分组展示。

### 主要机构域名

<code>bifenguanfang.org.cn</code>
<code>bifenguanfang.cn</code>
<code>bifenguanfang.net.cn</code>

### 关联网络域名

<code>bifenguanwang.com.cn</code>
<code>bifenguanwang.net.cn</code>
<code>bifenguanwang.cn</code>
<code>bifenguanwang.org.cn</code>

## 项目结构

The source tree is organized to separate core logic, configuration, user-facing tooling, and documentation. Each subdirectory contains an __init__.py file to denote a Python package and is accompanied by a README.md explaining its internal organization. The diagram below illustrates the top-level layout with annotations for the primary responsibility of each major component.

```
sentinel-link/
├── src/                                        # 核心源代码目录
│   └── sentinel_link/                          # 主包命名空间
│       ├── core/                              # 数据模型与验证逻辑
│       │   ├── models.py                      # Pydantic 资源条目定义
│       │   ├── validator.py                   # YAML 语法与 schema 校验
│       │   └── registry.py                    # 资源条目的内存注册表
│       ├── probe/                            # 可用性探测引擎
│       │   ├── http_probe.py                 # HTTP/HTTPS 探测实现
│       │   ├── tls_checker.py                # TLS 证书有效期检查
│       │   └── runner.py                     # 并发探测调度器
│       ├── cli/                              # 命令行接口模块
│       │   ├── main.py                       # Click 入口点
│       │   ├── validate_cmd.py               # validate 子命令实现
│       │   └── probe_cmd.py                  # probe 子命令实现
│       └── utils/                            # 通用辅助函数
│           ├── logger.py                     # 日志配置与格式化
│           └── file_io.py                    # 异步文件读写封装
├── config/                                    # 全局配置文件
│   ├── manifest.yaml                         # 主资源清单（用户可编辑）
│   └── probe_defaults.yaml                   # 探测超时与重试策略
├── tests/                                     # 单元测试与集成测试
│   ├── unit/                                 # 各模块独立单元测试
│   └── integration/                          # 端到端探测流程测试
├── docs/                                      # 完整项目文档
│   ├── user-guide/                           # 用户操作手册
│   ├── contributor/                          # 贡献者指引
│   └── architecture/                         # 系统设计文档
├── scripts/                                   # 开发辅助脚本
│   ├── pre-commit-hook.sh                    # Git 提交前代码检查
│   └── generate-report.py                    # 生成 HTML 静态资源报告
├── requirements.txt                           # 生产环境依赖列表
├── requirements-dev.txt                       # 开发与测试额外依赖
└── pyproject.toml                            # 项目元数据与构建配置
```

## 贡献指南

Contributions are welcomed in the form of new resource entries, improved probing logic, enhanced documentation, and bug fixes. All contributions must adhere to the following procedural steps to ensure consistency and maintainability.

1.  **Fork the Repository and Create a Feature Branch** – Fork the main repository to your personal GitHub account, then create a new branch with a descriptive name following the pattern `feature/<short-description>` or `fix/<issue-number>` . Ensure your branch is based on the latest `main` branch commit.
2.  **Update the Manifest File** – If adding or removing resources, edit the `config/manifest.yaml` file. Each entry must include the full URL (as provided by the user), a category tag, and a brief annotation describing the domain's purpose. Run `python -m sentinel_link.cli validate --manifest config/manifest.yaml` to verify structural correctness.
3.  **Write or Update Tests** – For any code changes to the probing engine or CLI, add corresponding unit tests under `tests/unit/` and ensure all existing tests pass. Use `pytest` as the test runner and aim for a minimum of 85% code coverage on new code.
4.  **Update Documentation** – If your contribution affects user-facing behavior, update the relevant sections in `docs/user-guide/` and `docs/api/`. Include inline code examples where appropriate to illustrate new configuration options or commands.
5.  **Submit a Pull Request** – Push your branch to your fork and open a pull request against the main repository's `main` branch. Fill out the pull request template completely, including a checklist of verification steps performed. The maintainers will review your submission within five business days.

## 常见问题

**Q: 该项目是否提供对所列域名的内容缓存或代理服务？**

A: 不提供。Project Sentinel Link 严格定位为外部资源的索引与元数据管理工具。它不会抓取、缓存、代理或重写任何第三方域名的内容。所有探测操作仅涉及 HTTP HEAD 或 GET 请求，且仅记录状态码、响应时间和 TLS 证书信息，不存储响应体内容。用户应当遵守各目标域名所在国家或地区的法律法规，并尊重目标站点的 robots.txt 规则。

**Q: 如何批量导入大量外部 URL 并自动分类？**

A: 项目提供了一个实验性的导入脚本，位于 `scripts/bulk-import.py`。该脚本接受 CSV 或 JSON 格式的输入文件，每行需包含 `url` 字段和可选的 `category` 字段。如果未指定类别，脚本会尝试基于顶级域名和路径前缀进行启发式分类，并将结果输出为 YAML 片段，用户随后可手动合并至主 `manifest.yaml` 文件。对于超过 1000 个条目的批量操作，建议分批导入并每批运行验证命令，以避免单次加载占用过多内存。

**Q: 当探测结果显示某个域名不可达时，系统会采取什么行动？**

A: 探测命令默认仅生成报告并输出到终端或指定文件，不会自动修改主清单或执行任何自动化故障转移操作。系统设计遵循 "观测优先" 原则，由运维人员或 CI 流程根据报告内容人工决策。如果连续三次探测（每次间隔 24 小时）均显示同一域名不可达，系统会向预先配置的 Webhook 发送警告事件，但具体处置策略（如移除条目、更换备选域名或通知原提供方）需由用户根据自身 SLA 要求自行实现。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:50
