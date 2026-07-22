# NovaLink 技术资源导航站

NovaLink 是一个面向开发人员、运维工程师与技术研究者的高性能外链聚合与导航系统。项目定位为轻量级、可自托管的统一资源入口，用于解决个人或团队在多个技术平台、数据看板、管理后台之间频繁切换导致的效率损耗。通过将分散的 URL 集中管理并结合自定义分类、权限视图与访问日志，NovaLink 能够显著降低信息查找成本，适用于内部技术中台、开源项目文档聚合、实验环境入口统一等多种场景。

目标用户包括需要维护多个后台系统的研发团队、拥有多套环境（测试、预发布、生产）的运维人员、以及希望建立个人知识导航体系的独立开发者。NovaLink 本身不存储业务数据，只负责 URL 的元信息管理与安全转发，因此可与任何现有认证系统（LDAP、OAuth2、SAML）快速集成，部署后即可作为团队的默认浏览器起始页或新成员入职导航页。

## 功能概览

- **多源链接统一管理**：支持手动添加、批量导入及定期同步外部数据源，自动识别链接状态（可访问、跳转、超时），并提供健康检查仪表盘。

- **分类与标签双维度组织**：允许为每个链接分配一个主分类和多个自定义标签，支持无限层级分类嵌套，满足复杂业务场景下的归类需求。

- **访问频率智能推荐**：基于内置的轻量级计数器和时间衰减算法，自动将高频访问链接置顶，并为低频链接提供“可能有用”的提醒标记。

- **用户级私有与共享视图**：支持多用户账号体系，每位用户可维护个人私有链接集合，同时可订阅公共分类或团队共享列表，权限粒度可细化至查看、编辑、管理三级。

- **快速搜索与模糊匹配**：提供毫秒级响应的本地全文检索，支持链接标题、描述、URL 片段、分类路径、标签组合等多字段联合查询，并支持拼音首字母快速定位。

- **可嵌入的微件模式**：支持将特定分类或标签下的链接列表生成为独立 iframe 嵌入代码，便于集成至内部 Wiki、Confluence、Jira 仪表板等第三方平台。

- **链接变更追踪与审计**：自动记录每个链接的创建人、修改人、修改时间及变更字段，支持导出完整变更日志，满足内部合规审计要求。

- **暗色主题与布局切换**：内置明暗两套配色方案，并提供网格、列表、卡片三种展示布局，用户可基于个人偏好持久化保存。

## 应用场景

- **企业内部开发门户**：将 CI/CD 系统、代码仓库、制品库、监控面板、日志平台等数十个内部系统的入口统一聚合，新入职员工只需打开 NovaLink 即可访问所有研发工具链，无需记忆复杂的内网域名或 IP 端口。

- **开源项目文档镜像站**：开源社区维护者可将项目相关的 GitHub 仓库、Read the Docs 文档、示例部署站点、社区论坛、会议录播等资源整合为一体化的导航页面，方便贡献者与用户一站式获取信息。

- **多环境运维控制台**：运维团队针对开发、测试、预发布、生产等不同环境，分别建立独立的链接分类，每个分类下放置对应环境的数据库管理后台、服务治理面板、告警中心、日志查询入口，配合权限控制确保生产环境仅对资深成员可见。

- **个人学习与收藏夹替代**：技术研究者可将日常阅读的技术博客、在线课程、API 参考手册、交互式教程、工具站等资源按技术栈（例如前端、后端、AI、运维）分类整理，替代浏览器自带的收藏夹，实现跨设备、跨浏览器的统一访问。

- **活动与赛事信息聚合**：针对特定技术活动或竞赛，可将官方公告、报名入口、赛程安排、实时榜单、成绩查询等多个相关页面集中展示，活动结束后一键归档，不影响日常导航结构。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议通过 WSL2 或 Git Bash 执行。默认使用 Python 3.10+ 及内置 SQLite 数据库，无需额外安装数据库服务。

```bash
# 克隆项目仓库
git clone https://github.com/novalink-dev/novalink.git
cd novalink

# 创建并激活 Python 虚拟环境（推荐）
python3 -m venv venv
source venv/bin/activate   # Windows 下使用 venv\Scripts\activate

# 安装核心依赖及推荐扩展
pip install --upgrade pip
pip install -r requirements.txt
pip install gunicorn==21.2.0  # 生产环境部署建议

# 初始化数据库及默认配置
python manage.py migrate
python manage.py loaddata initial_categories.json
python manage.py createsuperuser  # 按提示设置管理员账号

# 启动开发服务器（默认监听 127.0.0.1:8000）
python manage.py runserver
```

访问控制台输出中显示的本地地址（通常为 http://127.0.0.1:8000）即可开始使用。生产环境部署时，请参考 `docs/deployment.md` 使用 Gunicorn + Nginx 组合。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 ~ 3.12 | 核心运行环境，不支持 3.9 以下及 3.13 以上测试版 |
| Django | 4.2 LTS | Web 框架，长期支持版本提供稳定安全更新 |
| SQLite | 3.31+ | 默认内置数据库，适用于小型部署（链接总数 < 5 万） |
| PostgreSQL | 12+ | 生产环境推荐替代 SQLite，支持高并发与高级查询 |
| Redis | 6.2+ | 可选，用于缓存高频访问链接及会话存储，提升响应速度 |
| Node.js | 18+ | 仅用于前端静态资源构建（CSS/JS 压缩），不涉及运行时 |
| Nginx | 1.20+ | 生产环境反向代理与静态文件服务，非强制但强烈建议 |
| Memcached | 1.6+ | 替代 Redis 的缓存方案，二选一即可 |
| LDAP | 无版本限制 | 可选，用于对接企业统一身份认证，需额外安装 python-ldap |
| Docker | 20.10+ | 容器化部署方式支持，提供官方镜像及 compose 编排文件 |

## 文档导航

| 层面 | 目录 / 文档 | 回答的问题 |
|------|------------|-----------|
| 入门指南 | `docs/quickstart.md` | 如何 5 分钟内完成基础配置并导入第一批链接？首次登录后应该做什么？ |
| 运维手册 | `docs/operations/` | 如何配置 HTTPS 证书？如何备份与恢复数据库？如何调整健康检查的超时阈值？ |
| 开发参考 | `docs/development/` | 如何扩展一个新的链接来源解析器？如何修改前端主题变量？API 接口如何鉴权？ |
| 部署方案 | `docs/deployment/` | 支持哪些部署方式（Docker、Kubernetes、systemd）？各方案的资源要求及优劣比较 |
| 安全加固 | `docs/security/` | 如何限制仅内网访问？如何配置 IP 白名单？如何开启双因素认证？ |
| API 手册 | `docs/api/v2/` | 所有 RESTful 端点的请求/响应格式、状态码含义、分页参数及限流策略 |
| 插件开发 | `docs/plugins/` | 如何编写一个自定义插件以实现链接自动打标或访问统计上报？插件生命周期钩子说明 |
| 常见故障 | `docs/troubleshooting.md` | 启动报错、链接检测失败、搜索无结果、页面空白等典型问题的排查步骤 |

## 资源列表

### 体育数据及赛事信息类

<code>zuqiudsshengpingfu.net.cn</code>

<code>zuqiudsshoujiban.net.cn</code>

<code>ajiasaicheng.asia</code>

<code>baxizuqiujiajiliansai.asia</code>

<code>bijiasaicheng.asia</code>

<code>hanklianjifenbang.asia</code>

<code>jishibifenqiutan.asia</code>

### 使用说明

上述资源链接均为外部独立站点，与 NovaLink 核心项目无直接隶属关系。NovaLink 仅提供技术导航能力，不对外部站点的内容可用性、数据准确性及服务持续性作任何保证。用户添加外部链接时，应遵守所在地法律法规及目标站点的 robots 协议。建议定期通过 NovaLink 内置的健康检查功能扫描上述链接的可访问状态，并依据检查结果及时更新或移除失效条目。

## 项目结构

```
novalink/
├── manage.py                  # Django 命令行入口，用于开发调试及运维脚本
├── requirements.txt           # 核心 Python 依赖清单（含版本锁定）
├── runtime.txt                # 声明推荐 Python 版本，供 PaaS 平台识别
├── .env.example               # 环境变量模板，包含 SECRET_KEY、DB_URL、REDIS_URL 等
├── docker-compose.yml         # 本地开发及小规模生产使用的容器编排文件
├── Dockerfile                 # 多阶段构建文件，基于 Alpine 镜像优化尺寸
│
├── novalink/                  # 项目主配置目录
│   ├── settings/              # 分环境配置（base, dev, prod, test）
│   │   ├── base.py            # 所有环境共享的基础配置
│   │   └── production.py      # 生产环境特有配置（禁用 DEBUG、静态文件 CDN 等）
│   ├── urls.py                # 根路由分发，挂载 admin、api、health 及静态资源
│   └── wsgi.py                # WSGI 应用入口，适配 Gunicorn / uWSGI
│
├── apps/                      # 所有独立功能模块（Django apps）
│   ├── links/                 # 链接核心管理：模型、分类、标签、健康检查
│   │   ├── models.py          # Link, Category, Tag, HealthRecord 等
│   │   ├── services.py        # 链接状态检测、访问计数、推荐逻辑
│   │   └── admin.py           # 后台管理界面定制化
│   ├── users/                 # 用户扩展模型、权限分组、LDAP 适配器
│   ├── search/                # 基于 SQLite FTS5 / PostgreSQL 全文检索的实现
│   └── widgets/               # 可嵌入微件的渲染引擎及缓存策略
│
├── static/                    # 前端静态资源（CSS、JavaScript、图标字体）
│   ├── css/                   # 主样式文件及暗色主题变量（SCSS 源文件在 src 下）
│   ├── js/                    # 原生 JS 模块（搜索防抖、布局切换、键盘快捷键）
│   └── images/                # 默认 Logo、默认分类图标、空状态占位图
│
├── templates/                 # Django 模板文件（含 base、首页、分类视图、管理页）
│   ├── base.html              # 全局骨架，包含导航栏、侧边栏、页脚
│   └── links/                 # 链接列表、详情、编辑、导入导出等子模板
│
├── fixtures/                  # 初始数据固件（默认分类、示例链接、权限组）
├── scripts/                   # 运维辅助脚本（数据库备份、链接批量导入、迁移检查）
├── tests/                     # 单元测试与集成测试（覆盖模型、服务、API）
└── docs/                      # 完整文档源文件（Markdown 格式，用于生成静态站点）
```

## 贡献指南

1. **阅读行为准则与贡献约定**：请先查阅 `CODE_OF_CONDUCT.md` 及 `CONTRIBUTING.md` 文件，了解社区沟通规范、签署 CLA（贡献者许可协议）的流程，以及 commit message 的格式要求（遵循 Conventional Commits 标准）。

2. **选择或创建 Issue 并获取指认**：在 GitHub Issues 中查找带有 `help wanted` 或 `good first issue` 标签的任务。若计划新增较大功能特性，建议先创建一个讨论型 Issue，描述设计思路与预期改动范围，等待维护者反馈后再动手，避免无效 PR。

3. **Fork 仓库并创建功能分支**：将主仓库 Fork 至个人账号下，然后克隆本地。请基于 `develop` 分支创建新的特性分支，分支命名建议采用 `feature/描述` 或 `fix/描述` 格式。提交前务必运行 `pre-commit` 钩子以执行代码格式化（black、isort、flake8）。

4. **补充或更新测试用例**：所有新功能或 Bug 修复必须包含对应的单元测试（位于 `tests/` 目录下）。确保 `python manage.py test` 全部通过且覆盖率不低于 85%。对于涉及前端改动的贡献，需附上手动的浏览器兼容性测试说明。

5. **提交 Pull Request 并参与评审**：将分支推送至个人 Fork 仓库后，向主仓库的 `develop` 分支发起 PR。PR 描述中需清晰关联相关 Issue，列出改动点、测试结果以及 UI 变化截图（如适用）。评审过程中请及时响应评审意见，评审通过后由维护者合并或由贡献者自行 squash 合并。

## 常见问题

**Q: 添加的链接数量超过一万条后，页面加载和搜索响应明显变慢，应如何优化？**

A: 首先确认是否已启用 Redis 或 Memcached 缓存服务，并在 `settings/production.py` 中正确配置缓存后端。其次，确保数据库表已建立索引（项目迁移文件默认包含针对 `url`、`title`、`category_id` 及 `last_checked` 字段的复合索引）。如果使用 PostgreSQL，建议定期执行 `VACUUM ANALYZE` 更新统计信息。对于搜索场景，可切换至 PostgreSQL 的 pg_trgm 模块或启用 SQLite 的 FTS5 虚拟表以获得更好的模糊匹配性能。最后，检查 `LINK_PER_PAGE` 配置值，建议保持在 50 ~ 100 之间，避免单次查询返回过多数据。

**Q: 如何实现链接变更后自动通知订阅用户？**

A: 项目未内置邮件或消息队列通知系统，但提供了扩展钩子 `post_link_update` 和 `post_link_create`。您可以在 `apps/links/signals.py` 中编写自定义信号接收器，通过调用第三方邮件服务（如 SendGrid）、企业微信机器人、钉钉自定义机器人或 Slack Webhook 发送变更通知。若需要更完备的通知管理，建议集成 Celery 或 Django Q 实现异步任务，避免阻塞主请求响应。

**Q: 是否可以同时运行两个 NovaLink 实例共享同一套数据库？**

A: 支持。但需要确保所有实例使用相同的 `SECRET_KEY` 和缓存后端（Redis 共享）。同时，必须将 `SESSION_ENGINE` 设置为 `django.contrib.sessions.backends.cache` 或 `django.contrib.sessions.backends.db`，并统一配置 `SESSION_COOKIE_DOMAIN`。多实例部署时，链接健康检查任务（通过 `manage.py check_links` 定时执行）应仅在一个实例上启用，避免重复检测造成资源浪费和误报。建议使用分布式锁（如 Redis 锁）来保证定时任务的单例执行。

## 许可证

MIT License

Copyright (c) 2026 NovaLink Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:30
