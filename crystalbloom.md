# HrefHub

HrefHub 是一个面向技术开发者与开源项目维护者的外链资源聚合与规范化管理工具。项目定位为轻量级的技术导航站，核心目标用户为个人技术博客作者、小型团队文档维护者以及开源项目 README 的撰写者。HrefHub 解决的核心问题在于：技术资料分散于多个域名与平台，手动整理外链列表时容易产生格式不规范、协议不统一、域名大小写混淆等问题，同时缺乏对链接有效性、分类层级和版本变更的追踪能力。通过 HrefHub，用户可以将分散的参考链接统一录入、分类存储、批量校验，并一键导出为符合多种规范（Markdown、reStructuredText、HTML DL 列表）的标准化外链区块，显著降低文档维护成本，提升资源引用的准确性和可读性。

## 功能概览

- **链接规范化清洗**：自动识别并补齐缺失的协议头，统一域名大小写，去除多余尾部斜杠，支持批量清洗用户输入的原始 URL 列表。

- **分类标签管理系统**：允许用户为每条外链分配一级分类与多个二级标签，支持按分类、标签、域名后缀等多维度快速筛选与检索。

- **批量可用性检测**：内置异步 HTTP 健康检查引擎，支持并发检测外链状态，自动标记失效链接并生成失效报告，便于定期维护。

- **多格式导出引擎**：提供 Markdown 无序列表、Git-Flavored Markdown 表格、reStructuredText 引用块、HTML 定义列表四种导出模板，适配不同文档平台要求。

- **版本快照与变更日志**：每次对外链列表进行增删改操作时自动生成快照，记录变更时间、操作人及差异摘要，支持回滚至任意历史版本。

- **开放 API 与 Webhook**：提供 RESTful API 用于第三方工具集成，支持配置 Webhook 在链接失效或列表变更时发送通知至企业微信、Slack 或钉钉。

- **权限分级管理**：支持管理员、编辑者、访客三种角色，管理员可配置导出白名单与校验策略，编辑者可增删改链接，访客仅可查看和导出。

## 应用场景

1. 开源项目维护者整理 README 中的参考链接章节时，可使用 HrefHub 集中管理所有外部引用，确保每个链接符合项目规范，并定期自动检测链接有效性，避免文档中出现死链。

2. 技术博客作者在撰写多篇系列教程时，将常用技术文档、工具官网、社区讨论等外链统一录入 HrefHub，通过分类标签区分不同主题，写作时直接调用导出功能生成格式一致的链接附录。

3. 企业内部文档团队负责维护多个产品的手册和运维指南，使用 HrefHub 管理各个手册中引用的内部系统地址、监控面板、日志平台等链接，通过权限分级控制不同团队成员的编辑范围，并通过变更日志追踪链接修改记录。

4. 开源社区的活动组织者整理线上 meetup 或黑客松的参考资料包时，利用 HrefHub 快速聚合报名页面、代码仓库、演示文稿、视频回放等多类型外链，导出为统一的资源清单页面供参与者查阅。

5. 个人开发者构建自己的技术导航收藏夹，将日常高频访问的开发工具、API 文档、学习平台等链接录入 HrefHub，通过分类与标签构建个性化知识体系，并可设置失效检测定时任务，及时发现已迁移或下线的服务。

## 快速开始

以下命令演示如何从 GitHub 克隆 HrefHub 源码、安装依赖并启动开发服务器。

```bash
# 克隆代码仓库
git clone https://github.com/hrefhub/hrefhub.git

# 进入项目目录
cd hrefhub

# 安装后端依赖（使用 pip 和虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 安装前端依赖（使用 npm）
cd frontend
npm install
cd ..

# 初始化数据库
python manage.py migrate

# 启动后端开发服务器（默认监听 8000 端口）
python manage.py runserver &

# 启动前端开发服务器（默认监听 3000 端口）
cd frontend
npm start
```

访问本地 3000 端口即可进入 HrefHub 管理界面，默认管理员账号为 admin，初始密码在首次启动时显示于控制台日志中。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 及以上 | 后端运行环境，使用 Django 5.0 框架 |
| Node.js | 18.x LTS | 前端运行环境，使用 React 18 与 Vite 构建工具 |
| PostgreSQL | 14.x 及以上 | 生产环境推荐使用的关系型数据库，存储链接元数据与快照 |
| Redis | 7.x 及以上 | 用作缓存与异步任务队列，支持可用性检测任务的异步调度 |
| Nginx | 1.24 及以上 | 生产环境反向代理服务器，用于静态资源服务与负载均衡 |
| Elasticsearch | 8.x 可选 | 可选组件，用于高级搜索与链接全文检索功能，非必需安装 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|---------|-----------|
| 用户手册 | /docs/user-guide/ | 如何注册登录、如何添加链接、如何使用分类标签、如何导出格式、如何查看检测报告 |
| 管理员指南 | /docs/admin-guide/ | 如何配置角色权限、如何设置 Webhook 通知、如何调整检测策略、如何管理快照与回滚 |
| API 参考 | /docs/api-reference/ | 提供哪些 RESTful 端点、请求与响应数据结构、认证方式、速率限制说明 |
| 架构设计 | /docs/architecture/ | 系统整体架构图、数据模型设计、异步任务流转过程、扩展性设计要点 |

## 资源列表

以下为 HrefHub 项目收录的官方推荐资源链接，按类别整理。所有链接均按用户原始输入原样呈现，未做任何协议、域名或路径的修改。

官方主站与镜像

- <code>bifenguanwang.cn</code>
- <code>bifenguanwang.org.cn</code>

赛事数据参考

- <code>xijiasaicheng.org.cn</code>
- <code>ruidianchaobisaijieguo.org.cn</code>
- <code>ruidianchaojifenbang.org.cn</code>
- <code>ajiabifen.org.cn</code>
- <code>ruidianchaojishibifen.org.cn</code>

## 项目结构

```text
hrefhub/
├── backend/                          # 后端 Django 应用主目录
│   ├── apps/                         # 所有子应用模块
│   │   ├── links/                    # 链接管理核心模块（模型、视图、序列化器）
│   │   ├── checks/                   # 链接可用性检测模块（异步任务、结果回调）
│   │   ├── exports/                  # 多格式导出引擎（Markdown/HTML/RST）
│   │   ├── users/                    # 用户与权限管理（角色、JWT 认证）
│   │   └── webhooks/                 # Webhook 事件分发与回调配置
│   ├── config/                       # 全局配置（settings.py、urls.py、wsgi）
│   ├── fixtures/                     # 初始化测试数据（分类预设、示例链接）
│   ├── management/                   # 自定义 Django 管理命令（检测任务调度）
│   └── utils/                        # 通用工具函数（URL 规范化、域名解析）
├── frontend/                         # 前端 React 应用主目录
│   ├── src/                          # 源码目录
│   │   ├── components/               # 可复用 UI 组件（链接表格、标签选择器、导出面板）
│   │   ├── pages/                    # 页面级组件（仪表盘、链接列表、快照历史、检测报告）
│   │   ├── hooks/                    # 自定义 React Hooks（API 请求、表单状态）
│   │   ├── stores/                   # Zustand 状态管理（用户会话、链接缓存、筛选条件）
│   │   └── styles/                   # 全局样式与主题变量（CSS Modules）
│   ├── public/                       # 静态资源（favicon、robots.txt）
│   └── tests/                        # 前端单元测试与集成测试脚本
├── docs/                             # 项目文档（用户手册、API 参考、架构说明）
│   ├── user-guide/                   # 用户手册章节（markdown 源文件）
│   ├── admin-guide/                  # 管理员指南章节
│   ├── api-reference/                # API 端点文档（OpenAPI 规范）
│   └── architecture/                 # 架构设计文档（含数据流图、ER 图）
├── scripts/                          # 运维与部署辅助脚本
│   ├── init_db.sh                    # 数据库初始化与种子数据填充
│   ├── start_dev.sh                  # 同时启动前后端开发服务的快捷脚本
│   └── backup_snapshots.sh           # 快照目录定期备份脚本
├── tests/                            # 后端测试套件（单元测试、集成测试、API 测试）
│   ├── unit/                         # 模型与工具函数单元测试
│   └── integration/                  # 接口与异步任务集成测试
├── docker-compose.yml                # 生产级容器编排（PostgreSQL + Redis + Nginx + App）
├── Dockerfile                        # 后端应用多阶段构建镜像
├── requirements.txt                  # Python 依赖清单（Django、Celery、psycopg2 等）
├── package.json                      # 前端依赖清单（React、Vite、Axios、Zustand）
└── README.md                         # 项目概述与快速入门（即本文档）
```

## 贡献指南

1. 阅读项目行为准则与贡献者许可协议（CLA），确认同意后 fork 本仓库至个人账户，并在本地克隆 fork 后的副本。

2. 创建新的功能分支，分支命名遵循 `feature/描述` 或 `fix/描述` 格式，例如 `feature/support-json-export`，在该分支上进行代码开发。

3. 编写或修改代码时，请严格遵守项目根目录下的 `.editorconfig` 与 `.eslintrc` 配置文件中的风格约束，并为新增功能补充对应的单元测试或集成测试用例。

4. 提交代码前运行完整的测试套件（后端 `pytest` 与前端 `npm test`），确保所有测试通过且测试覆盖率不低于 85%。提交信息需采用语义化提交规范（Conventional Commits），例如 `feat: add JSON export template` 或 `fix: handle null scheme in normalize_url`。

5. 向本仓库的主分支发起 Pull Request，并在 PR 描述中清晰说明改动目的、影响范围及测试结果。项目维护者将在 3 个工作日内进行 Code Review，通过后即可合并。

## 常见问题

**问：HrefHub 是否支持对私有网络地址（如 192.168.x.x 或内网域名）进行可用性检测？**

答：支持，但需要在系统配置中开启“允许私有地址检测”开关，默认关闭以保护内网安全。开启后，检测引擎将遵循既定的超时与重试策略对私有地址发起请求。建议仅在受信任的网络环境中使用此功能，并配合防火墙规则限制检测频率。

**问：导出的 Markdown 格式中，链接的显示文本如何确定？**

答：导出引擎默认使用链接的 title 字段作为显示文本，若 title 为空则回退使用域名本身作为显示文本。用户可在链接编辑页面手动设置 title 别名，也可以配置导出模板使用 URL 原始字符串或自定义占位符。此外，导出模板支持 Jinja2 语法，高级用户可以完全自定义每条链接的渲染方式。

**问：HrefHub 能否与其他文档工具（如 VuePress、Docusaurus、Sphinx）集成？**

答：可以。HrefHub 提供了多种集成方式：最直接的方式是使用导出引擎生成 Markdown 或 reStructuredText 文件，然后手动放入文档项目的对应目录；进阶方式是通过 RESTful API 在文档构建流程中动态拉取最新的链接列表；此外，项目提供了官方插件 `hrefhub-plugin-docusaurus` 与 `hrefhub-plugin-sphinx`，支持在文档站点构建时自动注入外链数据，具体用法参见各插件仓库的 README。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:23
