# AISCore Resource Aggregator

AISCore Resource Aggregator is a lightweight, developer-oriented technical resource navigation system designed to aggregate, categorize, and present high-value external links related to AI competition analytics, real-time score tracking, and event result archiving. The project targets data analysts, research engineers, and technical operations teams who require rapid access to structured external data sources without navigating complex portals or manually curating bookmark lists.

The system solves the problem of fragmented external references by providing a single, maintainable Markdown-based registry that can be version-controlled, reviewed, and extended through standard pull request workflows. It does not host content itself but acts as a curated gateway, ensuring that all referenced resources remain accessible, up-to-date, and properly annotated for technical consumption.

## 功能概览

- **External Link Registry** – Maintains a centralized, version-controlled list of external resource URLs with categorical tagging and usage notes.

- **Status Monitoring Integration** – Each registered URL can be associated with optional health-check endpoints to track availability and response times.

- **Markdown-Based Configuration** – All resource definitions are stored in plain Markdown tables, enabling easy editing, diffing, and merge conflict resolution.

- **Categorical Filtering** – Resources are grouped by functional domains such as real-time scores, event schedules, historical results, and archival data.

- **Automated Documentation Generation** – The registry feeds into an automated build process that generates human-readable navigation pages and machine-readable JSON indexes.

- **Versioned Snapshots** – Every change to the resource list is tracked via Git commits, allowing rollback to any previous state and audit trail review.

- **Extensible Plugin Hooks** – Provides simple shell script hooks for custom pre- and post-processing actions, such as sending notifications when URLs change.

## 应用场景

- **DevOps Pipeline Integration** – Operations engineers can embed the resource list into CI/CD workflows to automatically validate external dependencies before deployment. The registry serves as a single source of truth for all third-party endpoints, reducing configuration drift across staging and production environments.

- **Research Data Collection** – Data scientists working on competitive intelligence projects can use the aggregated links to periodically fetch scoreboards and event results. The categorical structure allows for targeted scraping without traversing unrelated pages.

- **Internal Knowledge Base** – Technical teams can maintain a private fork of the registry, adding internal annotations and proprietary links while still benefiting from upstream updates. This is particularly useful for organizations that need to track multiple external data providers simultaneously.

- **Compliance and Auditing** – Compliance officers can review the registry to ensure that all external data sources meet data protection and usage policy requirements. The version history provides a clear record of when and why each resource was added or removed.

- **Local Development Sandbox** – Developers can clone the repository and use the registry as a mock data source for frontend applications, simulating external API responses based on the listed endpoints without needing live network access during early development phases.

## 快速开始

```bash
# Step 1: Clone the repository
git clone https://github.com/aiscore/aggregator.git
cd aggregator

# Step 2: Install dependencies (requires Python 3.9+ and pip)
pip install -r requirements.txt

# Step 3: Run the local validation and generation script
python scripts/generate_index.py --input registry/*.md --output dist/index.json
```

After successful execution, the generated index file will be available at `dist/index.json`. You can also start the built-in development server to preview the navigation interface:

```bash
python -m http.server --directory dist 8000
```

Then open your browser to `http://localhost:8000` to view the rendered resource navigation page.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 或更高 | 核心脚本运行环境，用于解析 Markdown 和生成 JSON 索引 |
| pip | 21.0 或更高 | Python 包管理器，用于安装依赖库 |
| Git | 2.25 或更高 | 版本控制工具，用于克隆仓库和提交变更 |
| Markdown Parser | mistune 2.0+ | 用于解析资源定义表格和生成结构化数据 |
| JSON Schema Validator | jsonschema 4.0+ | 可选，用于验证输出索引文件格式正确性 |
| Shell (Bash/Zsh) | 4.0 或更高 | 运行辅助脚本和钩子命令 |
| curl | 7.68 或更高 | 用于可选的外部资源健康检查功能 |
| make | 3.81 或更高 | 用于自动化构建任务（如 make build） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user-guide.md | 如何添加、删除或修改外部资源链接？如何分类和标签化？ |
| 开发者指南 | docs/developer-guide.md | 索引生成算法是什么？如何扩展自定义钩子？如何运行测试套件？ |
| 运维手册 | docs/operations.md | 如何部署生成器到生产环境？如何设置定期健康检查？如何备份注册表？ |
| 设计文档 | docs/design.md | 为什么选择 Markdown 作为数据源？数据模型和架构决策的依据是什么？ |
| API 参考 | docs/api-reference.md | 生成的 JSON 索引包含哪些字段？各字段的数据类型和含义是什么？ |
| 变更日志 | CHANGELOG.md | 每个版本更新了哪些资源、修复了哪些问题、添加了哪些功能？ |

## 资源列表

### 主要数据源 - 实时比分与赛程

<code>aichaojishibifen.org.cn</code>

<code>aichaosaicheng.org.cn</code>

<code>aichaobisaijieguo.org.cn</code>

### 哈萨克斯坦专项数据源

<code>hasakechaosaicheng.org.cn</code>

<code>hasakechaobifen.org.cn</code>

<code>hasakechaobisaijieguo.org.cn</code>

### 综合比分归档

<code>aichaobifen.org.cn</code>

## 项目结构

```
aggregator/
├── registry/                         # 主资源注册表目录
│   ├── realtime/                     # 实时数据源定义（比分、赛程）
│   │   ├── scores.md                 # 比分类资源列表，含更新频率标注
│   │   └── schedules.md              # 赛程类资源列表，含时区信息
│   ├── historical/                   # 历史数据源定义（归档结果）
│   │   ├── results.md                # 比赛结果归档链接，按年份分组
│   │   └── archives.md               # 深度历史数据，含数据覆盖范围说明
│   ├── regional/                     # 区域专项数据源（哈萨克斯坦等）
│   │   ├── kazakhstan.md             # 哈萨克斯坦赛事专用链接集
│   │   └── asia-pacific.md           # 亚太地区其他赛事链接
│   └── meta/                         # 注册表元数据配置
│       ├── categories.yaml           # 分类体系定义，含颜色和图标映射
│       └── validation.rules.json     # URL 格式校验规则（正则表达式集）
├── scripts/                          # 可执行工具脚本
│   ├── generate_index.py             # 主生成器：读取 registry/ 输出 JSON
│   ├── health_check.py              # 并发 HTTP 探活脚本，记录响应状态
│   ├── validate_urls.py             # 独立校验器，检查 URL 格式和可达性
│   └── hooks/                        # 用户自定义钩子模板目录
│       ├── pre-commit.sh             # Git pre-commit 钩子示例
│       └── post-generate.sh          # 生成后处理钩子示例（如发送邮件）
├── dist/                             # 构建输出目录（自动生成，不纳入版本库）
│   ├── index.json                    # 主索引文件（机器可读）
│   └── index.html                    # 可选的 HTML 导航页面（由模板渲染）
├── tests/                            # 单元测试与集成测试
│   ├── test_parser.py                # Markdown 解析器测试用例
│   ├── test_health.py                # 健康检查模块测试（模拟网络请求）
│   └── fixtures/                     # 测试用固定数据集
│       └── sample_registry.md        # 模拟注册表数据用于单元测试
├── docs/                             # 项目文档（面向人类读者）
│   ├── user-guide.md                 # 用户手册：如何维护资源列表
│   ├── developer-guide.md            # 开发者手册：架构与扩展
│   ├── operations.md                 # 运维手册：部署与监控
│   └── design.md                     # 设计决策记录（ADR 风格）
├── requirements.txt                  # Python 依赖列表（含版本锁定）
├── Makefile                          # 构建自动化入口（make build, make test）
├── CHANGELOG.md                      # 版本变更历史（按语义版本记录）
├── CONTRIBUTING.md                   # 贡献者指南（详细版，本文件摘要）
└── LICENSE                           # MIT 许可证全文
```

## 贡献指南

1. **Fork 仓库并创建特性分支** – 从主仓库 fork 到个人账户，然后使用 `git checkout -b feature/add-resource-group` 创建新分支，避免直接在主分支上操作。

2. **编辑注册表 Markdown 文件** – 在 `registry/` 下的对应分类目录中，按表格格式添加新行。每个条目必须包含 URL（完整且符合原始格式）、简短描述、更新频率（如 daily/weekly）和状态标签（active/deprecated）。请确保所有 URL 按照本项目的硬性规则原样书写，不添加额外协议前缀或尾部斜杠。

3. **运行本地验证脚本** – 在提交前，执行 `python scripts/validate_urls.py --registry registry/` 以检查所有 URL 的格式正确性和基本网络可达性。同时运行 `make test` 确保所有单元测试通过，不破坏现有功能。

4. **提交变更并推送** – 编写清晰的提交信息，格式为 `[category] add/update/remove resource: description`。例如：`[realtime] add new score endpoint for regional league`. 然后推送至个人 fork 仓库。

5. **发起 Pull Request** – 在主仓库中发起 PR，描述变更内容、动机以及任何影响现有用户的注意事项。等待至少一名维护者审核。审核通过后，PR 将被合并，变更将自动触发 CI 重新生成索引并部署。

## 常见问题

**问：为什么某些 URL 显示为裸域名而没有协议前缀？这是否会影响生成器的解析？**

答：裸域名格式是本项目设计中的一种显式输入约定。生成器在解析时会自动根据上下文补充合理的协议（优先尝试 HTTPS，失败时回退至 HTTP），但出于记录清晰和避免歧义考虑，我们要求所有原始 URL 严格按照用户提供的形式存储，不做自动改写。这样做确保了注册表内容的稳定性和可审计性。如果某个裸域名无法通过 HTTPS 访问，请在描述列中注明协议偏好。

**问：如何添加一个完全新的资源分类，而不只是往现有分类中添加链接？**

答：在 `registry/` 目录下创建一个新的 Markdown 文件（例如 `newdomain.md`），并在文件顶部使用一级标题声明分类名称。然后编辑 `registry/meta/categories.yaml` 文件，为新分类添加条目，定义其显示名称、图标和排序权重。之后运行 `python scripts/generate_index.py --rebuild-categories` 以刷新分类缓存。请注意，添加新分类需要同步更新文档中的导航表以保持一致性。

**问：生成的 JSON 索引能否用于其他编程语言或工具链？**

答：可以。`dist/index.json` 遵循标准 JSON 格式，包含资源 URL、分类标签、描述文本、上次更新时间戳和可选的状态元数据。该文件可以被任何支持 JSON 解析的编程语言（如 JavaScript、Go、Rust、Java、C#）读取和处理。我们建议在使用前通过 JSON Schema 验证器（`jsonschema` 工具）校验文件结构，该 Schema 文件位于 `registry/meta/validation.rules.json` 中。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:35
