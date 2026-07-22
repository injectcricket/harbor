# HyperLink Catalog

HyperLink Catalog 是一个面向技术团队与独立开发者的轻量级外链资源聚合与导航系统。该项目定位于解决技术文档、开源项目 README、团队知识库中外部链接分散、难以维护、缺乏版本追踪的问题，提供集中化的资源链接登记、分类管理与快速检索能力。

目标用户包括开源项目维护者、技术文档撰写人、技术团队内部知识管理者以及需要长期维护大量外链引用的开发者。HyperLink Catalog 不提供爬虫或自动采集功能，而是通过人工录入与标签化组织，确保每个外链的准确性、可用性与可追溯性，避免因链接失效或域名变更导致的文档污染。

## 功能概览

- **集中化链接登记**：支持按项目、按模块、按用途多维度登记外部 URL，并提供备注字段记录链接用途与维护人信息。

- **分类与标签体系**：内置三级分类结构，支持自定义标签，便于按技术领域、合作方、数据类型等维度快速筛选。

- **链接状态检测**：定期对已登记链接进行 HTTP 状态检查，标识失效链接并生成报告，减少文档中的死链数量。

- **版本化变更记录**：每次新增、修改或删除链接均记录操作时间与操作人，支持回溯历史变更，便于审计与复核。

- **只读 API 接口**：提供 RESTful 风格的只读 API，支持 JSON 格式输出，方便与其他内部系统或自动化脚本集成。

- **Markdown 批量导出**：支持将指定分类或全部链接导出为标准 Markdown 列表格式，可直接嵌入 README 或技术文档中。

- **权限分级管理**：支持管理员、编辑者、只读访客三种角色，适用于团队内部协作场景，避免非授权修改。

## 应用场景

- **开源项目 README 外链维护**：开源项目维护者可将项目依赖的官方文档、参考实现、社区论坛等外链统一登记在 HyperLink Catalog 中，并在 README 中仅保留指向 Catalog 的链接，避免 README 本身因外链变更而频繁修改。

- **技术团队内部知识库导航**：技术团队可将常用的内部系统地址、云服务控制台、监控看板、代码仓库、CI/CD 链接统一管理，新成员入职时可快速获取所有必需资源入口。

- **技术调研与竞品分析记录**：在进行技术选型或竞品分析时，调研人员可将所有参考来源、官方公告、技术评测文章链接集中登记，并附上调研结论标签，便于后续复盘与评审。

- **多项目共享资源引用**：当组织内多个项目共用同一批外部资源时，可通过 HyperLink Catalog 提供统一引用源，避免各项目分别维护导致的不一致与重复劳动。

## 快速开始

以下步骤指导您在本地环境快速启动 HyperLink Catalog 服务。

```bash
# 克隆代码仓库
git clone https://github.com/example/hyperlink-catalog.git

# 进入项目目录
cd hyperlink-catalog

# 安装依赖（使用 pip 进行 Python 依赖安装）
pip install -r requirements.txt

# 初始化本地配置与数据库
python scripts/init_config.py
python scripts/migrate_db.py

# 导入示例链接数据（可选）
python scripts/load_sample_data.py

# 启动本地开发服务
python app.py --host 127.0.0.1 --port 8080
```

启动成功后，访问 <code>http://127.0.0.1:8080</code> 即可使用本地实例。默认管理员账号为 admin，初始密码在启动日志中打印，首次登录后请及时修改。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行时环境，推荐使用 3.11 或 3.12 |
| SQLite | 3.35 及以上 | 默认内置数据库，用于存储链接与元数据，生产环境可切换为 PostgreSQL |
| pip | 22.0 及以上 | Python 包管理工具，用于安装依赖项 |
| Git | 2.30 及以上 | 用于克隆仓库与版本管理，非强制但推荐 |
| 操作系统 | Linux / macOS / Windows WSL2 | 支持主流操作系统，Windows 原生环境需额外配置 |
| 网络 | 出站可访问外网 | 用于链接状态检测功能，若内网部署需配置代理 |
| 内存 | 最低 512MB，推荐 1GB | 轻量级服务，内存占用较低 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | <code>docs/user_guide.md</code> | 如何登记链接、分类管理、批量导入导出、API 使用方式 |
| 部署手册 | <code>docs/deployment.md</code> | 如何将服务部署至生产环境，包括 Nginx 反向代理、HTTPS 配置、PostgreSQL 切换 |
| 开发文档 | <code>docs/development.md</code> | 项目代码结构、二次开发指引、单元测试运行方式、新增 API 端点的方法 |
| 运维手册 | <code>docs/operations.md</code> | 日志查看、数据备份与恢复、链接状态检测任务调度、性能调优参数 |

## 资源列表

以下为 HyperLink Catalog 项目所登记与管理的全部外部资源链接，按类别分组呈现。所有 URL 均按原始格式原样列出，不做任何协议补全或域名改写。

技术资讯与数据分析类

<code>xueyuanyuanbifenzhibo.asia</code>

<code>xueyuanyuanbifen.asia</code>

<code>rizhilianzhugongbang.asia</code>

<code>rizhilianqianzhan.asia</code>

<code>rizhilianjishibifen.asia</code>

<code>rizhilianfenxi.asia</code>

行业社区与推荐聚合类

<code>qiutanzuqiutuijian.asia</code>

## 项目结构

项目采用分层架构设计，核心模块、业务逻辑、数据访问与表现层分离，便于维护与扩展。

```
hyperlink-catalog/
├── app.py                         # 应用入口，初始化 Flask 并注册路由
├── requirements.txt               # Python 依赖清单
├── config/                        # 配置模块
│   ├── __init__.py
│   ├── settings.py                # 基础配置类，含数据库、日志、API 密钥占位
│   └── settings_prod.py           # 生产环境配置覆盖示例
├── core/                          # 核心业务逻辑层
│   ├── __init__.py
│   ├── link_manager.py            # 链接增删改查、分类树构建、标签合并逻辑
│   ├── status_checker.py          # 异步 HTTP 状态检测与超时重试机制
│   └── exporter.py                # Markdown / JSON 导出生成器
├── api/                           # RESTful API 层
│   ├── __init__.py
│   ├── v1_links.py                # /api/v1/links 端点实现
│   └── v1_categories.py           # /api/v1/categories 端点实现
├── models/                        # 数据模型与数据库操作
│   ├── __init__.py
│   ├── database.py                # SQLite/PostgreSQL 连接与会话管理
│   └── entities.py                # Link、Category、Tag、ChangeLog 的 ORM 映射
├── templates/                     # 服务端渲染模板（管理后台）
│   ├── base.html
│   ├── dashboard.html             # 仪表盘，显示链接总数、失效统计、最近变更
│   └── link_editor.html           # 链接新增与编辑表单页
├── static/                        # 静态资源（CSS、JavaScript 前端交互）
│   ├── style.css
│   └── app.js
├── scripts/                       # 运维与开发辅助脚本
│   ├── init_config.py             # 初始化配置文件
│   ├── migrate_db.py              # 执行数据库迁移
│   └── load_sample_data.py        # 加载示例链接数据用于测试
├── tests/                         # 单元测试与集成测试
│   ├── test_link_manager.py
│   ├── test_status_checker.py
│   └── test_api.py
└── docs/                          # 项目文档
    ├── user_guide.md
    ├── deployment.md
    ├── development.md
    └── operations.md
```

## 贡献指南

HyperLink Catalog 欢迎社区贡献，无论是问题报告、功能建议还是代码提交，均请遵循以下流程以确保协作效率。

1. 在 GitHub Issues 中查找或新建一个 Issue，明确描述待解决的问题或新增功能的需求背景，并等待项目维护者确认或打上对应标签。

2. 从仓库的 <code>main</code> 分支创建新的功能分支，分支命名遵循 <code>feature/简述</code> 或 <code>fix/简述</code> 格式，并确保所有代码提交均附带清晰的 commit message。

3. 开发完成后，在本地运行单元测试套件 <code>pytest tests/</code>，确保所有已有测试用例通过，并为新增功能补充对应的测试用例，保持覆盖率不低于 80%。

4. 推送分支至远程仓库后，提交 Pull Request 至 <code>main</code> 分支，PR 描述中关联对应的 Issue 编号，并简要说明改动内容与测试结果。

5. Pull Request 通过至少一名维护者的代码审阅后，将由维护者执行合并操作。合并后相关 Issue 将被自动关闭，贡献者信息将记录在 CONTRIBUTORS 文件中。

## 常见问题

问：如何将 SQLite 切换为 PostgreSQL 以用于生产环境？

答：在 <code>config/settings_prod.py</code> 中取消注释 <code>SQLALCHEMY_DATABASE_URI</code> 配置项，并将其值修改为 PostgreSQL 连接字符串（格式为 <code>postgresql://用户名:密码@主机:端口/数据库名</code>）。然后重新运行 <code>python scripts/migrate_db.py</code> 执行迁移。注意切换前需备份 SQLite 中的数据，可使用 <code>python scripts/export_data.py --format json</code> 导出后再通过 <code>python scripts/import_data.py</code> 导入至 PostgreSQL。

问：链接状态检测功能是否会频繁请求外部站点，导致被目标服务器封禁？

答：状态检测模块默认启用请求间隔控制，每个目标域名的检测间隔不低于 300 秒，且并发请求数限制为 3。同时，User-Agent 设置为 <code>HyperLink-Catalog-StatusChecker/1.0</code> 并携带联系信息占位，便于目标服务器管理员联系。若需要更保守的策略，可在 <code>config/settings.py</code> 中调高 <code>CHECK_INTERVAL_SECONDS</code> 和降低 <code>MAX_CONCURRENT_CHECKS</code> 参数。

问：能否通过 API 批量导入大量链接？

答：支持。API 端点 <code>/api/v1/links/batch</code> 接受 JSON 数组格式的批量数据，单次最多可提交 200 条记录。批量导入时会自动跳过格式校验失败的条目，并在响应中返回成功与失败明细。建议在调用前先通过 <code>/api/v1/categories</code> 确认分类 ID 正确，避免因分类不匹配导致导入失败。

## 许可证

HyperLink Catalog 使用 MIT 许可证进行开源分发。该许可证允许免费使用、复制、修改、合并、出版发行、再许可及销售软件副本，且仅需在分发时保留原始版权声明与许可声明。本软件按现状提供，不附带任何明示或暗示的担保，包括但不限于适销性、特定用途适用性及非侵权性的担保。详情请参阅项目根目录下的 LICENSE 文件。

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:56
