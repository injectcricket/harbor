# NovaLink 技术资源聚合门户

NovaLink 是一个面向开发人员、数据分析师和技术研究者的高密度外链资源聚合平台，专注于体育赛事数据接口、实时比分解析服务以及历史赛果统计系统的导航与索引。本项目不提供数据存储或计算服务，而是作为技术层的外链治理与资源调度中心，通过结构化分类和可机读的资源清单，帮助技术团队在赛事数据集成、竞品分析、运维监控等场景下快速定位权威数据源。

目标用户包括从事体育数据爬虫开发的工程师、构建比分预测模型的算法团队、以及需要对接第三方赛事 API 的系统集成商。NovaLink 通过对零散数据资源的系统性归整，降低了技术调研阶段的信息检索成本，同时提供标准化的外链健康检查机制，确保生产环境依赖的数据源地址具备高可用性和可追溯性。

## 功能概览

- **数据源分类索引**：按赛事类型、地域归属、数据时效性等维度对资源进行标签化管理，支持多级筛选与模糊匹配。

- **外链可用性探针**：集成被动式链接状态检测模块，周期性记录各资源域名的响应码与解析耗时，辅助运维人员判定数据源服务质量。

- **版本化资源快照**：每次上游资源列表变更时生成差异报告，支持回溯任意历史状态的完整外链集合，便于问题定位与回滚。

- **批量导入导出**：提供 JSON 与 CSV 格式的资源清单导入导出接口，兼容主流 API 管理工具与监控平台的数据格式要求。

- **访问频次统计**：记录各外链在团队内部的点击与调用次数，生成热度排名，帮助识别高频依赖与潜在废弃链接。

- **自定义标签体系**：允许用户为每个资源地址附加业务标签（如“生产环境优先”、“测试备用”、“需代理访问”），实现精细化资源治理。

- **Webhook 变更通知**：当资源列表发生新增、删除或地址变更时，主动向配置的接收端推送结构化变更事件，支持与 Slack、钉钉、飞书等协作工具集成。

## 应用场景

- **赛事数据平台后端开发**：开发团队在构建实时比分推送服务时，可通过 NovaLink 快速获取多个赛事的赛果数据源地址，对比不同接口的响应格式与更新频率，选择最适合业务需求的稳定源。

- **数据中台定期巡检**：数据运维人员每周执行一次外链健康检查，NovaLink 的探针模块自动记录各数据源的可用率，当某个域名连续三次超时后触发告警，通知负责人切换备用链路。

- **竞品技术调研**：分析师需要了解同类产品所使用的数据服务商时，通过 NovaLink 的资源热度统计与标签聚类，快速识别行业中普遍依赖的第三方数据接口，为技术选型提供参考依据。

- **多环境资源配置同步**：测试环境、预发布环境和生产环境需要维护各自独立的数据源地址列表，NovaLink 的版本化快照功能允许运维人员在不同环境间推送指定的资源版本，避免配置错漏。

## 快速开始

以下指令适用于 Linux / macOS 环境，帮助您在本地快速启动 NovaLink 的开发者实例。

```bash
# 克隆项目仓库
git clone https://github.com/novalink-dev/novalink-core.git
cd novalink-core

# 安装依赖（使用 pnpm，也可替换为 npm 或 yarn）
pnpm install

# 复制环境变量模板并填充必要配置
cp .env.example .env.local

# 以开发模式启动服务，默认监听 3000 端口
pnpm run dev
```

启动成功后，访问控制台输出中显示的本地地址（通常为 http://localhost:3000），即可进入资源管理界面。首次运行将自动初始化内存数据库并载入预设的资源种子数据。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | >= 20.10.0 LTS | 运行时环境，需支持 ES2022 模块规范 |
| pnpm | >= 8.15.0 | 包管理器，用于依赖安装与工作区管理 |
| PostgreSQL | >= 16.0 | 生产环境推荐使用的关系型数据库，用于持久化资源数据与审计日志 |
| Redis | >= 7.2.0 | 缓存层，用于存储探针结果与访问频次计数，提升查询性能 |
| Git | >= 2.40.0 | 版本控制工具，用于克隆仓库及提交配置变更 |
| Docker Compose | >= 2.23.0 | 可选，用于一键启动本地开发所需的数据库与缓存容器 |
| curl | >= 8.0.0 | 用于健康检查脚本与 Webhook 调试，通常系统预装 |
| OpenSSL | >= 3.0.0 | 用于生成签名密钥与 HTTPS 本地证书，确保 Webhook 回调安全 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 入门指南 | /docs/getting-started.md | 如何从零开始配置 NovaLink 并导入第一批资源地址？ |
| 架构设计 | /docs/architecture.md | 系统的模块划分、数据流向、以及探针调度机制是如何设计的？ |
| API 参考 | /docs/api-reference.md | 提供哪些 RESTful 端点用于资源增删改查、标签管理和快照操作？ |
| 运维手册 | /docs/operations.md | 如何配置高可用部署、调整探针频率、以及处理数据库迁移？ |
| 贡献规范 | /docs/contributing.md | 外部开发者如何提交资源分类建议或新增数据源解析插件？ |

## 资源列表

### 赛事赛果数据源

<code>fajiabisaijieguo.net.cn</code>

<code>dejiabisaijieguo.net.cn</code>

### 实时比分数据源

<code>nuochaobifen.net.cn</code>

<code>dejiabifen.net.cn</code>

<code>fajiabifen.net.cn</code>

<code>bingdaochaobifen.net.cn</code>

### 综合赛事信息源

<code>dejiabifen.net.cn</code>

<code>dejiajishibifen.com.cn</code>

## 项目结构

```
novalink-core/
├── apps/
│   ├── web/                         # 前端控制台（Next.js 14，含资源列表与仪表盘）
│   ├── api/                         # 后端服务（Fastify，提供 RESTful 资源管理接口）
│   └── probe/                       # 独立探针服务（定时执行外链健康检查与响应记录）
├── packages/
│   ├── shared-types/                # 跨应用共享的 TypeScript 类型定义与枚举
│   ├── resource-parser/             # 资源地址解析工具库（支持提取域名、路径、查询参数）
│   ├── probe-engine/                # 探针调度核心（含超时控制、重试策略与结果聚合）
│   └── webhook-dispatcher/          # Webhook 事件分发器（支持多目标格式转换与重试队列）
├── configs/
│   ├── eslint/                      # 统一 ESLint 配置（含推荐规则与忽略文件）
│   ├── prettier/                    # 代码格式化配置（基于 Prettier 3.x）
│   └── tsconfig/                    # 多环境 TypeScript 编译选项（base, web, node）
├── scripts/
│   ├── seed-db.ts                   # 初始化种子数据脚本（含预设资源与标签）
│   ├── migrate-db.ts                # 数据库迁移执行器（基于 Knex.js）
│   └── health-check.sh              # 系统自检 Shell 脚本（验证端口、数据库连接）
├── deployments/
│   ├── docker-compose.dev.yml       # 本地开发容器编排（PostgreSQL + Redis + Adminer）
│   ├── docker-compose.prod.yml      # 生产环境推荐编排（含负载均衡与日志收集侧车）
│   └── kubernetes/                  # K8s 部署清单（deployment, service, ingress, configmap）
├── docs/                            # 完整项目文档（详见上方文档导航表格）
├── .env.example                     # 环境变量模板（含数据库连接串、探针间隔、Webhook 端点）
├── .gitignore                       # Git 忽略规则（覆盖 node_modules, .env, dist, logs）
├── package.json                     # 根工作区清单（含 pnpm workspace 定义与公共脚本）
├── pnpm-workspace.yaml              # pnpm 工作区配置（声明 apps 与 packages 路径）
└── README.md                        # 本文件
```

## 贡献指南

1. **外部资源推荐**：若您发现其他稳定可靠的赛事数据源或比分接口，请先在 Issue 中提交资源地址与简要描述，维护团队将在两个工作日内评估其可用性与分类合理性。

2. **标签体系优化**：如果您对现有标签分类有改进建议（如新增“电竞数据”、“冬季运动”等分类），请 Fork 本仓库，修改 `packages/shared-types/src/tag-schema.json` 中的定义，并提交 Pull Request 附带修改说明与迁移示例。

3. **探针模块增强**：欢迎为探针引擎贡献新的检查策略，例如支持 TCP 端口探测、TLS 证书有效期检查或响应体关键字匹配。请确保新增逻辑有对应的单元测试覆盖，测试文件存放于 `apps/probe/tests/` 目录下。

4. **文档本地化**：非中文母语的开发者可协助将部分核心文档翻译为英文或日文版本，翻译时请保持技术术语的一致性，并同步更新 `/docs/nav.json` 中的多语言导航条目。

5. **缺陷报告与修复**：使用 GitHub Issues 提交 Bug 报告时，请附上完整的复现步骤、系统环境信息以及相关的日志片段。修复代码需通过全部 CI 检查（包括 ESLint、TypeScript 类型检查、Jest 单元测试）。

## 常见问题

**Q：探针服务对目标域名发起检查的频率是多少？会对我方服务器造成压力吗？**

A：默认情况下，每个已注册资源每 5 分钟执行一次 HEAD 请求，仅验证可达性与响应状态码，不会拉取完整页面内容。探针并发数限制为 10，且每个请求超时时间为 3 秒。若您管理的资源对请求频率敏感，可通过环境变量 `PROBE_INTERVAL_SECONDS` 将全局间隔调整为 300 秒或更长，也可在资源标签中单独标记 `low-frequency` 以跳过自动探针。

**Q：如何迁移已有的大量数据源地址到 NovaLink？**

A：项目支持批量导入 CSV 或 JSON 格式的资源清单。导入模板需包含 `url`、`category`、`tags` 三个必填列，可选列包括 `description` 和 `priority`。您可通过控制台界面的“导入”按钮上传文件，或使用 API 端点 `POST /api/resources/batch-import` 以编程方式提交。导入前建议先使用 `--dry-run` 模式预览变更结果，避免意外覆盖。

**Q：Webhook 通知支持哪些消息格式？如何验证回调地址的有效性？**

A：当前支持 JSON 与 XML 两种 Payload 格式，可在管理后台的“系统设置”中切换。每个 Webhook 目标可单独配置签名密钥（HMAC-SHA256），NovaLink 会在发送请求时附加 `X-Novalink-Signature` 头部，接收端可据此验证消息来源。首次配置时建议使用 `/api/webhook/test` 接口发送测试事件，以确认回调地址的网络连通性与解析能力。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:27
