# HrefHub

HrefHub 是一个面向开发人员、技术研究员与信息架构师的高质量外链与网络资源导航系统。项目定位于解决互联网信息过载环境下，技术文档碎片化、社区资源分散、官方入口难以追溯等核心问题，通过人工策展与自动化校验相结合的方式，构建一个可维护、可审计、可快速检索的 URL 资源聚合仓库。目标用户包括技术团队负责人、开源贡献者、运维工程师以及需要频繁查阅外部依赖文档的研发人员。

## 功能概览

- **分类策展体系**：按技术领域、服务商类型、内容形式对资源进行多级标签划分，支持快速过滤与批量导出。
- **可用性自动校验**：每日定时任务对收录 URL 执行 HTTP 状态检查，标记异常链接并生成报告，确保资源库活跃度。
- **版本化变更记录**：每次增删改操作均提交至 Git 历史，支持回溯任意时间点的完整资源快照。
- **批量导入与去重**：支持通过 CSV 或 JSON 批量追加链接，内置相似度检测算法避免重复条目。
- **Markdown 渲染预览**：资源详情页支持直接渲染外部 README 或接口文档的摘要信息，减少跳转损耗。
- **权限分级管理**：提供管理员、编辑者、只读观察员三种角色，适用于团队协作维护。
- **API 查询接口**：开放 RESTful 风格的查询端点，支持按标签、域名、状态码等参数编程式获取资源列表。
- **自定义标签模板**：允许用户根据自身项目需求创建私有标签分组，不影响公共策展体系。

## 应用场景

- **技术选型调研**：团队在引入新中间件或云服务时，可通过 HrefHub 快速检索官方文档、社区论坛、性能评测报告等入口，节省逐个搜索引擎查找的时间。
- **离线文档镜像规划**：运维人员可导出特定分类下的所有 URL，用于构建内部离线文档中心的初始爬取种子列表，保障内网环境下的访问效率。
- **开源项目依赖溯源**：开源维护者可将项目 README 中散落的外部引用链接统一迁移至 HrefHub 集中管理，通过版本化记录清晰追踪每次外部资源变更的原因。
- **技术培训材料准备**：讲师或技术作者在编写教程时，利用分类策展功能快速聚合参考链接，确保引用的权威性与时效性，避免交付后链接失效。
- **安全合规审查**：安全团队可定期导出全量资源列表，配合自动校验报告审查是否存在恶意域名或已下架的服务入口，满足企业合规要求。

## 快速开始

以下步骤适用于 Linux 及 macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 克隆项目仓库
git clone https://github.com/hrefhub/hrefhub.git
cd hrefhub

# 安装依赖（使用 Python 3.10+ 及 pipenv）
pip install pipenv
pipenv install --dev

# 初始化本地数据库并导入种子资源
pipenv run python manage.py migrate
pipenv run python manage.py loaddata seed_urls.json

# 启动开发服务器
pipenv run python manage.py runserver --host=0.0.0.0 --port=8080
```

访问 http://localhost:8080 即可查看本地实例。若需执行首次可用性校验，请运行：

```bash
pipenv run python manage.py check_all_urls --workers=4
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 至 3.12 | 核心解释器，低于 3.10 将无法使用 match 语句及类型提示新特性 |
| PostgreSQL | 14.x 或更高 | 主数据库，用于存储资源条目、校验历史及用户权限数据，需启用 pg_trgm 扩展支持去重 |
| Redis | 7.x 或更高 | 缓存与任务队列后端，用于异步校验任务及 API 速率限制计数 |
| Node.js | 18.x LTS | 仅用于前端资产构建（Tailwind CSS 及 Alpine.js），后端运行不依赖 |
| Git | 2.30+ | 版本控制，用于记录策展变更及回滚操作 |
| pipenv | 2023.x | Python 虚拟环境与依赖管理工具，确保依赖隔离 |
| curl / wget | 任意稳定版 | 仅用于生产环境健康检查脚本，开发环境非必需 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | /docs/user-guide/ | 如何浏览、搜索、收藏资源；如何理解标签体系与状态标记；如何导出个人列表 |
| 策展操作手册 | /docs/curator-guide/ | 如何新增、编辑、废止 URL；如何合并重复条目；如何填写变更注释 |
| 开发者指南 | /docs/developer-guide/ | API 鉴权方式；数据库 ER 图解析；自定义校验钩子的编写方法；前端组件复用规范 |
| 运维部署手册 | /docs/operations/ | 生产环境 Docker Compose 编排说明；日志采集策略；备份与恢复流程；性能调优参数 |

## 资源列表

### 体育数据与赛事资讯类

<code>zuqiudsshoujiban.net.cn</code>

<code>ajiasaicheng.asia</code>

<code>baxizuqiujiajiliansai.asia</code>

<code>bijiasaicheng.asia</code>

<code>hanklianjifenbang.asia</code>

<code>jishibifenqiutan.asia</code>

<code>puchaojifenbang.asia</code>

## 项目结构

```
hrefhub/
├── .github/                         # GitHub 工作流配置
│   ├── workflows/
│   │   ├── ci.yml                   # 主 CI：运行测试、lint、构建
│   │   └── nightly_check.yml        # 每日凌晨执行全量 URL 校验
│   └── CODEOWNERS                   # 模块代码所有者分配
├── src/
│   ├── backend/                     # Python Django 后端核心
│   │   ├── apps/
│   │   │   ├── resources/           # 资源条目管理（模型、视图、序列化器）
│   │   │   ├── checks/              # 校验引擎（含异步任务定义及重试策略）
│   │   │   ├── users/               # 用户认证、角色权限及操作审计日志
│   │   │   └── tags/                # 标签分类系统（树形结构支持）
│   │   ├── core/                    # 项目全局配置、中间件、异常处理器
│   │   ├── manage.py
│   │   └── requirements/            # 分环境依赖文件（base, dev, prod）
│   ├── frontend/                    # 基于 Alpine.js + Tailwind 的轻量管理面板
│   │   ├── public/                  # 静态资源入口
│   │   ├── src/
│   │   │   ├── components/          # 可复用的 Alpine 组件（搜索框、标签选择器）
│   │   │   ├── layouts/             # 页面布局模板
│   │   │   └── styles/              # Tailwind 定制主题及暗色模式变量
│   │   └── package.json
│   └── scripts/                     # 运维与工具脚本
│       ├── seed_loader.py           # 种子数据导入工具
│       ├── export_snapshot.py       # 全量资源导出为 JSON/CSV
│       └── health_check.sh          # 服务存活探测（用于 Kubernetes 探针）
├── tests/                           # 单元测试与集成测试（pytest 框架）
│   ├── unit/
│   └── integration/
├── docs/                            # 完整文档源文件（Sphinx + Markdown 混合）
├── docker-compose.yml               # 开发/生产统一编排（PostgreSQL, Redis, 应用服务）
├── Dockerfile                       # 多阶段构建镜像（精简生产镜像 < 200MB）
├── .env.example                     # 环境变量模板（含数据库连接、密钥、校验超时）
├── LICENSE                          # MIT 许可证
└── README.md                        # 当前文件
```

## 贡献指南

1.  **问题追踪与讨论**：请在 GitHub Issues 中搜索现有话题，若未找到相关讨论，请新建 Issue 并详细描述您发现的问题或期望的功能。对于较大的功能提案，建议先通过 Discussion 板块征求社区反馈。
2.  **本地开发环境准备**：根据上述「快速开始」及「安装要求」章节完成本地环境搭建。运行 `pipenv run pre-commit install` 安装 Git 提交钩子，自动执行代码格式检查（Black + isort）及基础安全扫描。
3.  **分支与提交规范**：从 `main` 分支新建功能分支，命名格式为 `feature/描述` 或 `fix/描述`。提交信息请遵循 Conventional Commits 规范（如 `feat: 添加批量去重接口` 或 `fix: 修复校验超时导致任务堆积的问题`），确保变更日志可自动生成。
4.  **测试与校验**：在提交 Pull Request 前，请确保所有单元测试通过（`pipenv run pytest`），并针对新增功能补充相应的测试用例。若修改了资源校验逻辑，请附带模拟数据验证重试及降级行为。
5.  **Pull Request 流程**：提交 PR 时请关联对应的 Issue 编号，详细描述实现思路、测试覆盖情况以及潜在兼容性影响。至少需要一位维护者批准后，方可合并。合并后 CI 将自动构建并推送测试镜像至内部仓库。

## 常见问题

**Q: 自动校验发现 URL 状态异常后，系统会如何处理？**

A: 系统不会自动删除或禁用异常 URL，以免因临时网络抖动或目标站点维护造成误伤。每次校验结果会记录在 `CheckRecord` 表中，包含状态码、响应时间、错误摘要。策展人员可在管理后台查看连续失败次数，若连续失败达到阈值（默认 7 次），条目会被标记为「待审查」并推送通知。最终是否移除或更新 URL，由人工决策并记录变更原因。

**Q: 如何迁移已有的书签集合或浏览器收藏夹？**

A: 项目提供了 `import_bookmarks` 管理命令，支持解析 Netscape 格式的 HTML 书签导出文件（Chrome、Firefox 均支持导出该格式）。此外，也支持通过 CSV 导入，表头需包含 `url`、`title`、`tags`（逗号分隔）三列。导入前系统会自动执行去重，并生成冲突报告供用户确认。

**Q: API 的速率限制策略是怎样的？匿名用户与认证用户有何区别？**

A: 匿名用户（未提供有效 API Token）的请求限制为每分钟 30 次，主要用于浏览和搜索。认证用户（通过 JWT 或 Session 鉴权）的默认限制为每分钟 300 次，适用于自动化脚本或批量导出场景。若需要更高的配额，可在管理后台为特定用户或服务账号提升限制等级。超出限制时，API 将返回 HTTP 429 状态码，并在响应头中包含 `X-RateLimit-Reset` 时间戳。

## 许可证

MIT License

Copyright (c) 2025 HrefHub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-07-22 18:16:59
