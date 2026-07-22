# OpenResource Hub

OpenResource Hub 是一个面向开发者与技术研究人员的轻量级外链资源聚合与导航系统。该项目定位于对互联网上分散的高价值技术内容、实时数据接口、行业动态站点进行结构化整理与统一呈现，帮助用户快速定位特定领域的信息入口。OpenResource Hub 本身不存储或生产数据，而是通过可维护的链接目录与简洁的查询界面，解决信息分散、检索效率低下的问题。其典型用户包括数据科学爱好者、体育赛事分析开发者、实时信息聚合服务运维人员以及需要频繁查阅特定领域垂直站点的技术决策者。

## 功能概览

- **多维度资源分类**：支持按技术领域、数据类型、更新频率等维度对收录的外链进行标签化分类，便于用户按业务场景筛选。

- **实时可用性检测**：系统后台定期对已收录的外链发起可用性探测，并在前端标注异常状态，降低用户访问无效资源的成本。

- **自定义资源收藏集**：允许用户创建个人收藏集，将常用外链分组保存，支持私有与公开两种可见性模式。

- **全文与标签检索**：提供基于关键词和标签组合的检索能力，支持模糊匹配，可快速定位包含特定术语或属于特定类别的资源。

- **访问统计与热度排序**：记录各外链的点击频次与最近访问趋势，支持按热度、更新时间、稳定性等指标排序展示。

- **外链变更订阅**：提供基于 Atom 或 RSS 协议的订阅端点，当收录资源发生新增、移除或 URL 变更时，订阅方可接收通知。

- **数据导入导出**：支持批量导入 URL 列表（支持 CSV 与纯文本格式）以及将当前收藏或分类结果导出为标准书签文件或 JSON 结构化数据。

## 应用场景

1. **实时数据源聚合**：开发者可通过 OpenResource Hub 集中管理多个体育赛事比分、赔率变动或行情数据的外部接口页面，避免在浏览器中维护大量散落书签，提升数据获取效率。

2. **技术调研与竞品分析**：产品经理或技术调研人员可利用分类标签快速筛选出特定领域的垂直站点，横向对比不同来源的数据呈现方式、更新策略与内容深度，为决策提供参考。

3. **运维监控辅助**：运维工程师可将各类监控面板、日志查询工具、状态页等内部与外部资源统一录入系统，借助可用性检测功能快速发现异常服务，减少故障发现时间。

4. **知识库外链管理**：技术团队可将 OpenResource Hub 作为内部知识库的外链管理中心，统一存放文档、API 参考、社区讨论帖等外部链接，并与内部 Wiki 或文档系统集成。

5. **个人学习路径组织**：学习者可按主题（如机器学习、前端工程、数据库调优）建立资源集合，系统化跟踪优质博客、教程与官方文档的更新动态。

## 快速开始

以下操作指南适用于 Linux 与 macOS 开发环境，Windows 用户建议使用 WSL 或 Git Bash。

```bash
# 克隆项目仓库
git clone https://github.com/openresource-hub/core.git
cd core

# 安装项目依赖（使用 pnpm 或 npm）
pnpm install
# 若未安装 pnpm，可使用 npm install

# 构建并启动开发服务器
pnpm run build
pnpm run start:dev
# 服务默认监听 3000 端口，访问 http://localhost:3000
```

生产环境部署建议使用 `pnpm run build:prod` 并配合 PM2 或 Docker 运行。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
| :--- | :--- | :--- |
| Node.js | 18.x 或 20.x LTS | 运行时环境，推荐使用官方长期支持版 |
| pnpm | 8.x 或 9.x | 包管理工具，用于依赖安装与工作区管理 |
| PostgreSQL | 14.x 或更高版本 | 主数据库，用于存储用户数据、收藏集与分类信息 |
| Redis | 7.x 或更高版本 | 缓存层，用于会话存储与热点资源统计 |
| Nginx | 1.24.x 或更高版本 | 生产环境反向代理与静态资源服务，可选但推荐 |
| Git | 2.30.x 或更高版本 | 用于克隆仓库与版本管理 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
| :--- | :--- | :--- |
| 用户手册 | `/docs/user-guide/` | 如何注册、分类管理、收藏资源、配置订阅？ |
| 管理员指南 | `/docs/admin-guide/` | 如何批量导入链接、配置探测策略、管理用户权限？ |
| 开发者文档 | `/docs/developer/` | 如何扩展分类插件、自定义前端主题、接入外部认证？ |
| API 参考 | `/docs/api/` | 所有 RESTful 接口的请求参数、响应格式与错误码说明 |
| 部署运维 | `/docs/deployment/` | 如何通过 Docker Compose 一键部署、如何迁移数据库、如何配置日志轮转？ |
| 设计决策 | `/docs/design/` | 为何选择当前数据模型、为何采用特定缓存策略、如何保证外链更新的最终一致性？ |

## 资源列表

### 体育赛事数据类

- <code>jinrizuqiubifenyucetuijian.asia</code>

- <code>zuqiudsshoujiban.com.cn</code>

- <code>dszuqiushengpingfu.cn</code>

- <code>zuqiuds.cn</code>

- <code>zuqiudsbifen.cn</code>

- <code>zuqiudsjinrituijian.cn</code>

- <code>zuqiudsbanquanchang.cn</code>

### 说明

上述资源均为用户提供的原始链接，OpenResource Hub 不对其内容、可用性及数据准确性作任何保证。用户应自行评估各站点的合规性与可靠性。系统仅提供导航入口，不参与任何数据转发或代理服务。

## 项目结构

```
core/
├── apps/
│   ├── web/                         # 主 Web 应用（Next.js 前端 + API 路由）
│   │   ├── pages/                   # 页面路由层
│   │   ├── components/              # 可复用 UI 组件
│   │   ├── hooks/                   # 自定义 React Hooks
│   │   └── styles/                  # 全局样式与主题变量
│   ├── worker/                      # 后台任务服务（可用性检测、统计聚合）
│   │   ├── probes/                  # 各类探测策略实现（HTTP、HTTPS、TCP）
│   │   ├── scheduler/               # 基于 cron 的任务调度器
│   │   └── queues/                  # 任务队列定义与消费者
│   └── shared/                      # 跨应用共享代码
│       ├── types/                   # TypeScript 类型定义与接口契约
│       ├── validators/              # 请求参数与数据模型校验器
│       └── constants/               # 系统级常量与配置默认值
├── packages/
│   ├── db/                          # 数据库迁移脚本与 ORM 模型定义
│   │   ├── migrations/              # 增量迁移文件（基于版本号）
│   │   ├── seeders/                 # 初始数据与演示数据填充
│   │   └── models/                  # 表结构对应的对象关系映射模型
│   ├── cache/                       # 缓存抽象层（支持 Redis 与内存适配器）
│   │   ├── adapters/                # 不同后端的适配实现
│   │   └── strategies/              # 缓存穿透、雪崩、击穿防护策略
│   └── utils/                       # 通用工具函数库
│       ├── url-normalizer/          # URL 清洗、归一化与格式校验
│       ├── html-parser/             # 轻量级 HTML 元数据提取（标题、描述）
│       └── logger/                  # 结构化日志封装（基于 pino）
├── configs/
│   ├── env/                         # 环境变量模式定义（dev, test, prod）
│   ├── nginx/                       # 生产环境 Nginx 配置模板
│   └── docker/                      # Docker Compose 与容器化配置
├── tests/
│   ├── unit/                        # 单元测试（覆盖核心工具与模型）
│   ├── integration/                 # 集成测试（数据库、缓存、API 交互）
│   └── e2e/                         # 端到端测试（关键用户路径）
├── scripts/
│   ├── deploy/                      # 自动化部署脚本（滚动更新、回滚）
│   ├── backup/                      # 数据库备份与恢复脚本
│   └── maintenance/                 # 数据清理、索引重建等维护任务
├── .github/
│   ├── workflows/                   # CI/CD 流水线定义（测试、构建、发布）
│   └── ISSUE_TEMPLATE/              # 问题与功能请求提交模板
├── docs/                            # 完整文档目录（见文档导航）
├── LICENSE                          # MIT 许可证文本
├── README.md                        # 项目根目录说明文件（即本文档）
├── package.json                     # 项目主清单与工作区配置
├── pnpm-workspace.yaml              # pnpm 多包工作区定义
└── tsconfig.base.json               # 全局 TypeScript 编译选项与路径映射
```

## 贡献指南

1. **分支管理**：从 `main` 分支创建新的特性分支，分支命名遵循 `feat/`、`fix/`、`docs/`、`refactor/` 前缀规范，例如 `feat/add-import-export`。禁止直接在 main 分支上提交。

2. **代码规范**：所有提交必须通过 ESLint 与 Prettier 检查，提交信息遵循 Conventional Commits 标准（如 `feat: 添加批量导入CSV支持`）。提交前需运行 `pnpm run lint` 与 `pnpm run format`。

3. **测试要求**：新增功能或修复缺陷时，必须补充相应的单元测试或集成测试，确保测试覆盖率不低于现有基线（当前为 82%）。运行 `pnpm run test:coverage` 可查看覆盖率报告。

4. **文档同步**：任何对外接口变更（API、配置项、数据库模式）必须在对应文档目录中更新，并确保文档示例代码可执行。提交时应包含文档变更。

5. **审核流程**：所有变更需通过 Pull Request 提交，至少需要一名项目维护者审批通过后方可合并。CI 流水线必须全部通过（包括测试、构建、代码扫描）。PR 描述中需明确说明变更目的、影响范围及测试验证方式。

## 常见问题

**Q: 系统是否支持添加非公开的内部链接（如内网地址或需要 VPN 访问的站点）？**

A: 支持。系统不对链接协议或域名做任何过滤，但可用性检测模块可能因网络隔离而频繁报告不可达。建议将此类资源标记为“内部”类型，并配置独立的探测策略（如跳过可用性检测或使用自定义超时时间）。私有资源仅在用户已登录且拥有相应权限时可见，具体可见范围可在分类或收藏集中配置。

**Q: 如果收录的外链站点变更了域名或路径，如何批量更新而不丢失关联数据（如收藏次数、标签）？**

A: 系统提供了“重定向迁移”功能。管理员可在管理后台提交旧 URL 到新 URL 的映射关系，系统会自动将所有关联数据（标签、收藏、热度统计）迁移至新 URL，并保留旧 URL 的跳转记录。该操作支持通过 CSV 批量导入映射列表。同时，系统会生成变更日志，供后续审计与回溯。

**Q: 如何将 OpenResource Hub 部署到内网环境且完全不访问公网？**

A: 项目所有依赖包均已锁定版本并可通过私有 npm 镜像或离线包方式安装。数据库与缓存服务均支持内网地址配置。需注意，可用性检测模块默认使用 Node.js 原生 `http`/`https` 模块，不会主动访问外部公网服务，除非用户录入的外链本身指向公网。若需完全离线运行，可关闭探测模块或将探测目标限定为内网 IP 段。

## 许可证

MIT License

Copyright (c) 2026 OpenResource Hub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:27
