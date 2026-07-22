# TrenchLinks

TrenchLinks 是一个面向技术内容创作者、开源项目维护者以及开发者社区运营者的外链资源聚合与导航系统。本项目定位为轻量级的技术资源目录管理工具，通过结构化方式收录并分类展示互联网上的优质技术文章、赛事数据、分析预测及行业动态链接，帮助用户快速定位所需信息，降低信息筛选成本。TrenchLinks 不生产内容，而是提供一套严谨的链接组织与检索框架，适用于个人书签管理、团队知识库建设以及公开技术导航站部署。

## 功能概览

- **多级分类目录体系**：支持按技术领域、数据类别、更新频率等维度建立层级化分类，每条链接可归属多个分类标签，便于多路径检索。

- **链接状态健康检查**：内置周期性 HTTP 状态监测机制，自动标记失效或重定向的链接，确保导航库内资源的可用性。

- **全文检索与过滤**：基于标题、描述、分类标签及来源域名进行关键词检索，支持按添加时间、点击量、状态码排序。

- **RSS 订阅源生成**：为每个分类目录生成独立 RSS 输出，方便用户通过阅读器订阅特定领域的链接更新。

- **Markdown 批量导入导出**：支持将链接列表以 Markdown 表格或列表形式批量导入，也可将全库导出为标准 Markdown 文档，便于版本管理与静态站点生成。

- **访问统计与热度排行**：记录每条链接的点击次数与最近访问时间，生成周榜与月榜，辅助识别高价值资源。

- **用户自定义收藏夹**：注册用户可创建个人收藏夹，将链接分组保存，并支持公开分享收藏夹为只读页面。

- **API 查询接口**：提供 RESTful API 用于外部程序查询链接数据，支持 JSON 与 CSV 响应格式，方便与其他系统集成。

## 应用场景

- **技术团队内部知识库维护**：开发团队可使用 TrenchLinks 整理项目相关的技术文档、API 参考、设计规范及运维手册链接，通过分类和检索功能快速查阅，减少重复问答。

- **个人开发者资源聚合**：独立开发者可利用本系统管理每日阅读的技术博客、开源工具、数据看板及行业资讯，通过健康检查功能自动过滤失效书签。

- **赛事数据与预测信息导航**：针对足球赛事比分、预测分析类网站，TrenchLinks 提供按赛事类型、预测模型、数据来源等标签分类的能力，帮助数据分析师快速定位特定维度的外部数据源。

- **开源项目外链文档站**：开源项目可将 TrenchLinks 部署为项目官网的扩展页面，用于展示相关生态工具、社区博客、视频教程及第三方评测链接，丰富项目生态展示。

- **内容聚合站点的链接后台**：内容平台或门户网站可使用 TrenchLinks 作为后台链接管理模块，通过 API 将分类链接输出至前端页面，实现动态导航栏或友情链接区域。

## 快速开始

以下步骤适用于 Linux 及 macOS 环境，Windows 用户建议使用 WSL 或 Git Bash。

```bash
# 克隆项目仓库
git clone https://github.com/trenchlinks/trenchlinks.git
cd trenchlinks

# 安装依赖（使用 pip 与虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化 SQLite 数据库并创建默认分类
python manage.py initdb
python manage.py seed_categories

# 以开发模式运行本地服务
python manage.py runserver --host 0.0.0.0 --port 8080
```

访问 http://localhost:8080 即可进入 TrenchLinks 仪表板。生产环境部署请参考文档导航中的部署指南。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行环境，推荐 3.11 |
| SQLite | 3.35 及以上 | 默认内置数据库，用于存储链接与分类元数据 |
| Redis | 6.0 及以上 | 可选，用于缓存访问统计与会话管理，非必需但建议 |
| Node.js | 18.x 及以上 | 仅用于前端资源构建，若使用预编译静态文件则可跳过 |
| Git | 2.25 及以上 | 用于版本克隆及后续更新拉取 |
| curl / wget | 任意稳定版本 | 用于健康检查脚本的外部请求，系统自带即可 |
| virtualenv | 20.0 及以上 | Python 虚拟环境管理，可通过 pip 安装 |
| gunicorn | 20.1 及以上 | 生产环境 WSGI 服务器，非开发模式必需 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user-guide/ | 如何添加链接、创建分类、使用检索及管理个人收藏夹 |
| 部署指南 | docs/deployment/ | 如何配置 Nginx 反向代理、设置 systemd 服务、启用 HTTPS |
| API 参考 | docs/api/ | 所有 RESTful 端点的请求参数、响应格式及错误码说明 |
| 开发者指南 | docs/developer/ | 项目代码结构、插件扩展机制、前端编译流程及测试规范 |

## 资源列表

以下为 TrenchLinks 项目官方维护及推荐的外部资源，按类别分组收录。所有链接均按照用户原始数据原样列出，未做任何格式修改。

赛事结果数据类

<code>tuchaobisaijieguo.asia</code>

<code>tuchaosaicheng.asia</code>

比分查询与走势类

<code>qiutanbifenw.org.cn</code>

<code>qiutanbifenw.com.cn</code>

预测与推荐分析类

<code>leisujinrituijian.org.cn</code>

<code>zuqiucaifuyuce.org.cn</code>

<code>zuqiucaifujinrituijian.org.cn</code>

## 项目结构

```
trenchlinks/
├── app/
│   ├── api/                         # RESTful API 路由与视图函数
│   │   ├── v1/                      # API 版本 v1 端点
│   │   │   ├── links.py             # 链接资源的 CRUD 操作
│   │   │   ├── categories.py        # 分类管理接口
│   │   │   └── stats.py             # 访问统计与排行接口
│   │   └── __init__.py
│   ├── core/                        # 核心业务逻辑层
│   │   ├── checker.py               # 链接健康状态检查器（异步任务）
│   │   ├── parser.py                # Markdown 导入导出解析器
│   │   └── rss_generator.py         # RSS 订阅源生成器
│   ├── models/                      # 数据库模型定义
│   │   ├── link.py                  # 链接实体模型（含状态、点击量）
│   │   ├── category.py              # 分类树模型（支持无限级嵌套）
│   │   └── user.py                  # 用户与收藏夹模型
│   ├── templates/                   # Jinja2 前端模板
│   │   ├── dashboard/               # 管理后台页面模板
│   │   └── public/                  # 公开导航页面模板
│   ├── static/                      # 预编译 CSS/JS 静态资源
│   │   ├── css/
│   │   └── js/
│   └── __init__.py
├── config/                          # 配置文件目录
│   ├── development.py               # 开发环境配置（SQLite + 调试）
│   ├── production.py                # 生产环境配置（PostgreSQL + Redis）
│   └── testing.py                   # 单元测试配置
├── docs/                            # 完整文档（见文档导航章节）
├── scripts/                         # 运维与辅助脚本
│   ├── health_check_cron.py         # 定时健康检查任务脚本
│   ├── backup_db.py                 # 数据库备份脚本
│   └── seed_demo_links.py           # 演示数据填充脚本
├── tests/                           # 单元测试与集成测试
│   ├── unit/                        # 模型与工具函数单测
│   └── integration/                 # API 与数据库集成测试
├── requirements.txt                 # Python 依赖清单
├── manage.py                        # 项目管理命令行入口
├── .env.example                     # 环境变量示例文件
└── README.md                        # 本文件
```

## 贡献指南

1. 复刻项目仓库至个人账户，并在本地创建功能分支，分支命名格式为 `feature/功能简述` 或 `fix/问题编号`，避免直接在主分支修改。

2. 在 `tests/` 目录下为新功能或修复编写对应的单元测试用例，确保测试覆盖率达到 80% 以上，并执行 `python manage.py test` 验证全部用例通过。

3. 按照 PEP 8 规范编写 Python 代码，提交前运行 `flake8 app/` 与 `black app/` 进行格式检查和自动格式化，保持代码风格一致。

4. 若涉及前端模板或静态资源修改，需在提交前执行 `npm run build` 重新编译资源文件，并将编译后的文件一并提交至 `app/static/` 目录。

5. 发起 Pull Request 至主仓库的 `develop` 分支，PR 描述中需清晰说明改动目的、涉及模块及测试结果，等待项目维护者审核与合并。

## 常见问题

Q: 健康检查功能如何配置检查频率和超时时间？

A: 健康检查的调度间隔和超时阈值可通过环境变量调整。在 `.env` 文件中设置 `CHECK_INTERVAL_HOURS=6` 表示每 6 小时执行一次，设置 `CHECK_TIMEOUT_SECONDS=10` 表示每次请求超时时间为 10 秒。修改后需重启服务或执行 `python manage.py restart_worker` 使配置生效。默认情况下，系统会检查所有状态为 "active" 的链接，失效链接将自动标记为 "inactive"。

Q: 如何从旧版 SQLite 迁移至 PostgreSQL 以支持多用户高并发？

A: 项目提供了迁移脚本 `scripts/migrate_sqlite_to_pg.py`。首先在 `config/production.py` 中配置 PostgreSQL 连接字符串，然后执行该脚本，工具会自动读取 SQLite 数据并映射至 PostgreSQL 的表结构，迁移完成后需将 `DATABASE_URL` 环境变量指向 PostgreSQL 地址并重启服务。建议在低峰期执行，并提前备份 SQLite 文件。

Q: Markdown 批量导入时支持哪些链接格式？

A: 导入器支持三种格式：无序列表 `- [标题](URL)`、有序列表 `1. [标题](URL)` 以及表格形式 `| 标题 | URL | 分类 |`。导入时系统会尝试自动识别格式，若无法解析则会跳过并记录日志。每行的分类字段可使用斜杠表示层级，例如 "数据/足球/欧洲杯"，系统会自动创建缺失的分类节点。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:18
