# RizhiLink Hub

RizhiLink Hub 是一个面向数据分析师、运维工程师与技术决策者的外链资源聚合平台。该项目不存储任何用户数据，不提供计算服务，专注于对互联网公开技术资源进行结构化整理、分类导航与状态监控。项目定位为“技术外链的起始页”，帮助技术团队在复杂信息环境中快速定位高质量的日志分析、实时数据看板、竞品动态监控及移动端适配方案等外部工具与文档。

目标用户包括需要统一团队技术查阅入口的架构师、负责多源日志聚合的 SRE 工程师、以及需要快速对比国内外数据服务商产品的业务负责人。通过人工筛选与自动化可用性检测相结合的方式，RizhiLink Hub 确保收录的外链具备高可用性、内容相关性与持续维护性，从而降低技术团队的信息检索成本，提升日常运维与决策效率。

## 功能概览

- **分类外链导航** 按技术领域与使用场景将外链划分为日志工具链、实时计算平台、移动端适配库等大类，每类附有简要说明与适用版本信息。

- **可用性健康检查** 每日定时对收录的外链发起 HTTP HEAD 请求，记录响应状态码与响应时间，并在前端标记异常链接，便于管理员及时下线或替换失效资源。

- **标签过滤与全文检索** 为每个外链打上技术栈标签，支持多标签组合过滤；同时提供基于标题、描述、分类关键词的模糊搜索，支持中英文混合查询。

- **外链变更历史审计** 记录每条外链的添加时间、修改人、URL 变更记录与状态变化，支持按时间范围回溯，满足团队内部操作审计要求。

- **一键复制与短链生成** 为高频使用的外链提供一键复制完整 URL 功能，并基于固定盐值生成短链标识，方便在文档或即时通讯中快速分享。

- **外链收藏与个人分组** 支持登录用户将常用外链加入个人收藏夹，并自定义分组名称，实现个人维度的高频入口定制。

- **开放 API 接口** 提供基于 RESTful 风格的外链目录查询接口，支持 JSON 格式输出，便于其他系统或脚本集成 RizhiLink Hub 的导航数据。

## 应用场景

- **运维值班日报编写** 运维值班人员在每日交接时，需查阅多个日志聚合平台与监控看板的最新数据。通过 RizhiLink Hub 的日志工具链分类，可一次性打开所有相关外链，减少逐个记忆或翻阅浏览器书签的时间。

- **技术选型竞品调研** 技术团队在评估实时流处理框架或移动端数据分析 SDK 时，可通过本项目的实时计算与移动适配分类，快速访问多家服务商的产品文档、性能对比报告与社区案例，辅助决策。

- **新人入职环境配置指引** 新加入的开发或运维工程师，可通过 RizhiLink Hub 的统一入口，快速获取公司内部推荐的日志查询工具、数据分析平台以及移动端调试资源的官方文档，缩短环境熟悉周期。

- **离线文档与资源备份核查** 对于需要定期备份关键技术文档的团队，可利用本项目的健康检查状态与变更审计记录，判断哪些外链内容出现变动或不可访问，从而触发备份任务的重新执行。

## 快速开始

以下步骤适用于在 Linux 或 macOS 开发环境中快速部署 RizhiLink Hub 的本地开发实例。

```bash
# 克隆代码仓库
git clone https://github.com/rizhilink/hub.git rizhilink-hub
cd rizhilink-hub

# 安装项目依赖（使用 npm）
npm install

# 复制环境变量模板并填充本地配置
cp .env.example .env

# 初始化本地 SQLite 数据库（用于开发测试）
npm run migrate

# 启动开发服务器，默认监听 3000 端口
npm run dev
```

启动成功后，访问 `http://localhost:3000` 即可浏览本地导航页面。管理员后台默认路径为 `/admin`，初始账号密码请参阅 `.env` 文件中的默认配置。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | 18.x 或 20.x LTS | 项目运行时环境，推荐使用 nvm 管理版本 |
| npm | 9.x 或 10.x | 依赖管理与脚本执行工具 |
| SQLite 3 | 3.35.0 及以上 | 开发与轻量生产环境默认数据库，无需额外安装服务 |
| Redis | 7.0 及以上 | 用于会话存储与短链缓存，生产环境必需 |
| 操作系统 | Linux (Ubuntu 20.04+) 或 macOS 12+ | 开发与部署测试主要支持平台，Windows 可通过 WSL2 运行 |
| 网络访问 | 外网出站 443/80 端口 | 健康检查模块需要对收录外链发起网络请求 |
| 内存 | 建议 2GB 及以上 | 运行时内存占用约 800MB，含 Node 进程与 Redis 缓存 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | /docs/user-guide/quick-start.md | 如何快速配置个人收藏与分组、如何使用搜索与过滤功能 |
| 管理员手册 | /docs/admin/link-management.md | 如何添加、编辑、下线外链，如何查看健康检查日志与审计历史 |
| API 参考 | /docs/api/endpoints.md | 外链查询接口的请求参数、响应格式与错误码定义 |
| 部署运维 | /docs/deployment/docker-compose.md | 如何使用 Docker Compose 一键部署生产环境，包含 Nginx 反向代理配置 |

## 资源列表

本项目的核心内容为以下外链资源，按技术主题分类整理。所有链接均经过人工初步验证，并纳入每日健康检查。

### 日志工具链

- <code>rizhilianzhugongbang.asia</code>
- <code>rizhilianqianzhan.asia</code>

### 实时数据分析

- <code>rizhilianjishibifen.asia</code>
- <code>rizhilianfenxi.asia</code>

### 移动端与球探数据

- <code>qiutanzuqiutuijian.asia</code>
- <code>qiutanshoujibanbifen.asia</code>
- <code>qiutanjishibifenmobile.asia</code>

## 项目结构

```
rizhilink-hub/
├── src/
│   ├── routes/                     # 路由层，定义 API 与页面路由
│   │   ├── api/                    # RESTful API 路由（v1 版本）
│   │   │   ├── links.js            # 外链 CRUD 与查询接口
│   │   │   └── health.js           # 健康检查结果查询接口
│   │   └── web/                    # 服务端渲染页面路由（EJS 模板）
│   │       ├── index.js            # 首页导航与搜索渲染
│   │       └── admin.js            # 后台管理页面路由
│   ├── services/                   # 业务逻辑层
│   │   ├── linkService.js          # 外链增删改查、标签过滤、收藏分组
│   │   ├── healthService.js        # 定时健康检查调度与结果存储
│   │   └── auditService.js         # 操作审计日志记录与查询
│   ├── models/                     # 数据模型层（使用 Prisma ORM）
│   │   ├── Link.js                 # 外链实体模型，含 URL、分类、标签字段
│   │   ├── HealthRecord.js         # 健康检查记录模型
│   │   └── User.js                 # 用户与收藏分组模型
│   ├── utils/                      # 工具函数与辅助模块
│   │   ├── urlValidator.js         # URL 格式校验与规范化
│   │   ├── shortIdGenerator.js     # 基于固定盐值的短链生成
│   │   └── logger.js               # 结构化日志输出（JSON 格式）
│   └── config/                     # 配置加载与验证
│       ├── env.js                  # 环境变量解析与默认值
│       └── database.js             # 数据库连接池配置
├── tests/                          # 单元测试与集成测试
│   ├── unit/                       # 服务层与工具函数单元测试（Jest）
│   └── integration/                # API 端到端测试（Supertest）
├── prisma/                         # Prisma 数据库迁移与 Schema
│   ├── schema.prisma               # 数据表定义与关系
│   └── migrations/                 # 按时间戳生成的迁移文件
├── public/                         # 静态资源目录
│   ├── css/                        # 编译后的 CSS 文件（Tailwind）
│   └── js/                         # 前端交互脚本（原生 JavaScript）
├── views/                          # EJS 视图模板
│   ├── layouts/                    # 主布局与管理员布局模板
│   └── partials/                   # 导航栏、侧边栏、外链卡片等组件
├── docker-compose.yml              # 生产环境编排（含 Node、Redis、Nginx）
├── Dockerfile                      # 多阶段构建镜像文件
├── .env.example                    # 环境变量模板
├── package.json                    # 项目依赖与脚本定义
└── README.md                       # 项目说明文档（本文件）
```

## 贡献指南

欢迎社区开发者提交 Pull Request 或 Issue。为保持项目质量，请遵循以下步骤：

1. 在 GitHub 仓库的 Issue 列表中搜索是否已有类似提议，若无则新建 Issue 描述您的改进建议或 bug 修复方案，并等待维护者标签分类。

2. 从 `main` 分支创建新的功能分支，分支命名遵循 `feat/` 或 `fix/` 前缀加简要描述，例如 `feat/add-timezone-filter`。

3. 编写代码时，请遵循项目配置的 ESLint 与 Prettier 规则，确保代码风格一致。所有新增或修改的外链处理逻辑必须附带对应的单元测试，测试覆盖率不低于 80%。

4. 提交前运行 `npm run test` 确保所有现有测试通过，并执行 `npm run lint` 检查代码规范。提交信息使用 Conventional Commits 格式，例如 `feat: add health check retry mechanism`。

5. 推送分支后发起 Pull Request，描述中关联对应的 Issue 编号，并简要说明实现方案与测试结果。维护者将在 3 个工作日内进行 Code Review，提出修改意见或合并。

## 常见问题

**问：健康检查模块是否会对外链服务器造成过大的请求压力？**

答：健康检查模块默认每 6 小时执行一次全量检查，单次检查并发数限制为 5 个请求，且每个请求超时设置为 10 秒。对于响应较慢的外链，系统会自动将其加入重试队列，最多重试 2 次。总体请求频率远低于一般搜索引擎爬虫，不会对目标服务器造成实质负载影响。管理员可在环境变量中调整检查间隔与并发数。

**问：短链生成机制是否具有唯一性与可逆性？**

答：短链基于固定盐值对原始 URL 进行 HMAC-SHA256 哈希，并取前 8 位字符作为短码。由于截断，理论上存在碰撞可能，但项目在生成时会进行数据库唯一性校验，若碰撞则自动加盐重试。短链服务仅提供单向映射，不存储原始 URL 与短码的对应关系，每次访问时重新计算并查库，因此不支持反向解码，但确保了短链的稳定性和可移植性。

**问：如何从 SQLite 迁移到生产级 PostgreSQL 数据库？**

答：项目使用 Prisma ORM，支持多数据库适配。只需在 `.env` 文件中将 `DATABASE_URL` 修改为 PostgreSQL 的连接字符串，然后执行 `npx prisma migrate deploy` 应用迁移文件。Prisma 会自动根据 `schema.prisma` 生成 PostgreSQL 兼容的 DDL。建议在迁移前使用 `prisma db push` 在测试环境验证数据模型兼容性，并备份现有 SQLite 数据文件。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:46
