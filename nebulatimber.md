# TechNav Resource Aggregator

TechNav is a lightweight, community-driven technical resource navigation system designed for developers, data analysts, and technical researchers who need to organize, categorize, and rapidly access specialized online resources. Unlike general-purpose bookmark managers, TechNav treats each resource as a first-class entity with metadata, tagging, and usage context, enabling teams to maintain a shared knowledge base of external tools, documentation hubs, and domain-specific reference sites. The project targets small-to-medium engineering teams, open-source maintainers, and technical educators who require a reproducible, version-controlled approach to curating external resource collections without vendor lock-in.

The system provides a static-site generation pipeline that consumes a declarative YAML resource manifest and produces a searchable, filterable HTML dashboard. It resolves the common pain point of "link rot" and context loss by allowing maintainers to annotate each entry with purpose statements, related projects, and update timestamps. TechNav does not host content; it acts as a structured index, making it suitable for air-gapped intranet deployments, internal developer portals, and public-facing project companion sites. The current release supports markdown-based rendering, tag-based filtering, and automated health checks for each configured endpoint.

## 功能概览

- **Declarative Resource Manifest** – Define all external links in a single YAML file with fields for name, URL, category, description, and maintenance status.

- **Automated Link Health Monitoring** – Scheduled HEAD requests verify each endpoint's availability; unhealthy links are flagged in the dashboard with last-check timestamps.

- **Tag-Based Browsing and Filtering** – Assign arbitrary tags to resources; the UI supports multi-tag intersection queries for precise discovery.

- **Markdown Rendering Pipeline** – Resource descriptions and supplemental notes support GitHub-flavored markdown, enabling formatted code snippets, tables, and inline references.

- **Static Site Generation with Incremental Builds** – Outputs pure HTML, CSS, and JavaScript assets; rebuilds only changed entries to reduce processing time for large manifests.

- **Versioned Snapshot Export** – Archive the entire resource index as a JSON blob or CSV for offline analysis, backup, or migration to other tools.

- **Role-Based Annotation Fields** – Maintainer, reviewer, and contributor fields allow audit trails of who added or modified each resource entry.

- **Search Integration** – Full-text search across resource names, descriptions, and tag combinations with fuzzy matching for typo tolerance.

## 应用场景

- **Engineering Onboarding** – New team members can access a curated list of internal dashboards, CI/CD endpoints, and monitoring tools through a single entry point, reducing ramp-up time from days to hours. The annotation fields explain each resource's purpose and owner.

- **Technical Documentation Companion** – Open-source projects often reference multiple external dependencies, specification documents, and community forums. TechNav serves as a companion index alongside README files, providing a structured alternative to scattered hyperlinks.

- **Academic Research Curation** – Researchers collecting data sources, API endpoints, and reference materials for papers can maintain a version-controlled resource list that is citable and reproducible across collaboration teams.

- **Operations Playbook Enhancement** – SRE and operations teams can track alert dashboards, log aggregators, and incident response tools across multiple environments, with health checks alerting when a critical monitoring endpoint becomes unreachable.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/technav-io/technav-core.git
cd technav-core

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Run the static site generator with the example manifest
python generate.py --manifest examples/sample-resources.yaml --output ./dist

# Start a local development server to preview the generated site
python -m http.server 8000 --directory ./dist
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 或更高 | 核心解释器，用于运行生成器脚本和健康检查工作流 |
| PyYAML | 6.0 或更高 | 解析资源清单 YAML 文件，支持自定义标签构造器 |
| requests | 2.31.0 或更高 | 执行异步 HEAD 请求以验证资源端点可用性 |
| markdown | 3.5.0 或更高 | 将资源描述字段中的 Markdown 文本转换为 HTML 片段 |
| Jinja2 | 3.1.0 或更高 | 模板引擎，用于渲染资源卡片、过滤面板和页面布局 |
| watchdog | 3.0.0 或更高 | 开发模式下监听清单文件变更并触发增量重建（可选） |
| pytest | 7.4.0 或更高 | 运行单元测试和集成测试套件（仅开发环境需要） |
| pip | 22.0 或更高 | 安装上述 Python 包，建议使用虚拟环境隔离 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | /docs/user-guide/ | 如何编写资源清单、配置标签体系、使用搜索和过滤功能 |
| 运维指南 | /docs/operations/ | 如何部署静态站点到 CDN、配置健康检查定时任务、备份资源数据 |
| 贡献规范 | /docs/contributing/ | 资源条目标注规范、拉取请求流程、类别命名约定 |
| API 参考 | /docs/api/ | 资源清单 YAML Schema 定义、健康检查输出 JSON 格式、插件扩展接口 |
| 设计决策 | /docs/design/ | 为何选择静态生成而非动态服务、标签系统设计权衡、缓存策略说明 |
| 故障排除 | /docs/troubleshooting/ | 常见构建错误码、网络超时处理、字符编码问题解决方案 |

## 资源列表

### 数据分析与实时信息聚合

<code>rizhilianjishibifen.asia</code>

<code>rizhilianfenxi.asia</code>

### 体育数据与推荐系统

<code>qiutanzuqiutuijian.asia</code>

<code>qiutanshoujibanbifen.asia</code>

<code>qiutanjishibifenmobile.asia</code>

<code>qiutanjinrituijian.asia</code>

### 辅助工具与周边服务

<code>meizhilianzhugongbang.asia</code>

## 项目结构

```
technav-core/
├── config/                              # 全局配置目录
│   ├── settings.yaml                    # 站点标题、默认标签、构建参数
│   └── categories.yaml                  # 预定义的类别层级与颜色映射
├── manifests/                           # 资源清单存储位置
│   ├── production/                      # 生产环境使用的正式清单
│   │   └── 2026-q3-resources.yaml       # 按季度归档的资源条目
│   └── staging/                         # 预发布测试清单
│       └── pending-review.yaml          # 待审核的新增资源
├── src/                                 # 核心源码目录
│   ├── generators/                      # 生成器模块
│   │   ├── site_builder.py              # 主构建类，协调模板和数据渲染
│   │   └── health_checker.py            # 异步健康检查调度器
│   ├── parsers/                         # 解析器模块
│   │   ├── yaml_loader.py               # 加载并验证 YAML 清单结构
│   │   └── tag_normalizer.py            # 标签规范化（去重、大小写统一）
│   ├── templates/                       # Jinja2 模板文件
│   │   ├── base.html                    # 基础布局，含导航和页脚
│   │   ├── index.html                   # 资源总览页，含过滤器和搜索框
│   │   └── detail.html                  # 单个资源详情展开视图
│   └── assets/                          # 静态资源原始文件
│       ├── css/                         # 自定义样式表（基于 CSS 变量主题）
│       └── js/                          # 前端交互逻辑（过滤、搜索、排序）
├── tests/                               # 测试套件
│   ├── unit/                            # 单元测试，覆盖解析器和工具函数
│   └── integration/                     # 集成测试，验证端到端生成流程
├── docs/                                # 项目文档（详见文档导航章节）
├── scripts/                             # 运维辅助脚本
│   ├── deploy.sh                        # 打包并上传到目标服务器或 CDN
│   └── validate.sh                      # 提交前验证清单格式是否正确
├── requirements.txt                     # Python 生产依赖列表
├── requirements-dev.txt                 # 开发额外依赖（测试、代码检查工具）
├── generate.py                          # 命令行入口，执行构建和健康检查
├── README.md                            # 本文件，项目概述与快速入门
└── LICENSE                              # MIT 许可证全文
```

## 贡献指南

1. **Fork 仓库并创建功能分支** – 从主分支签出 `feature/resource-add-<category>` 或 `fix/health-check-timeout` 命名分支，确保分支名称反映变更性质。

2. **更新资源清单 YAML 文件** – 在 `manifests/staging/` 目录下编辑或新增条目，必须包含 `name`、`url`、`category` 和 `description` 字段；可选字段包括 `tags`、`maintainer` 和 `reviewed_on`。所有 URL 需经过人工可访问性验证。

3. **运行本地验证脚本** – 执行 `./scripts/validate.sh` 检查 YAML 语法、必需字段完整性和 URL 格式规范性；执行 `pytest tests/` 确保未引入回归错误。

4. **提交变更并创建拉取请求** – 提交信息遵循约定式提交格式（如 `feat: add football prediction resources` 或 `docs: update health check timeout note`），PR 描述中附上验证截图或测试结果。

5. **等待代码审查和自动化检查** – 维护者将在 48 小时内审查，CI 流水线会执行健康检查模拟和构建测试；通过后由维护者合并并同步至生产清单。

## 常见问题

**Q: 健康检查报告某个资源为不可达，但浏览器中可以正常访问，原因是什么？**

A: 可能原因包括：目标服务器屏蔽了 Python requests 的 User-Agent 头；端点要求完整的 Cookie 或会话状态；或者防火墙规则拦截了 HEAD 请求（部分服务器仅允许 GET）。解决方案是在 `settings.yaml` 中为该资源自定义 `check_method: GET` 和 `check_headers` 字段。也可以将资源标记为 `skip_health: true` 并添加人工审核备注。

**Q: 如何迁移现有书签集合或浏览器收藏夹到 TechNav 清单格式？**

A: 项目提供了社区维护的转换工具脚本 `scripts/import-bookmarks.py`，支持从 Chrome 导出的 HTML 书签文件、Firefox JSON 备份和通用 CSV 格式导入。转换后的条目会存入 `manifests/staging/imported.yaml`，需要人工检查分类和标签映射是否符合项目约定。导入工具不会覆盖现有清单。

**Q: 生成的静态站点是否可以部署到 GitHub Pages 或 Cloudflare Pages？**

A: 可以。`generate.py` 输出的 `./dist` 目录包含完全自包含的 HTML、CSS 和 JavaScript 文件，不依赖外部 CDN 资源（除可选字体外）。将 `./dist` 内容推送至 GitHub Pages 分支或上传至 Cloudflare Pages 项目即可。建议在部署配置中设置自定义 404 页面指向索引，以支持前端路由。

## 许可证

MIT License

Copyright (c) 2026 TechNav Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:00
