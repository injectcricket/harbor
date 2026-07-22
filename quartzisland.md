# OSS-Navi

开源技术资源导航与信息聚合系统，面向开发者、技术决策者及科研人员，用于高效检索、分类管理和版本追踪互联网中的公开技术文档、赛事数据与实时信息流。本项目解决信息分散、来源不可靠、检索效率低下的问题，通过结构化索引和自动化校验机制，将零散的网络资源转化为可信任的知识库。

## 功能概览

- **多源数据聚合**：系统内置爬虫调度框架，支持对指定数据源进行周期性抓取，自动去重并归并至统一索引。
- **实时状态监控**：对每个注册的数据端点实施可用性检测与响应时间记录，异常时触发告警通知。
- **结构化检索接口**：提供基于关键词、时间范围、数据类别等多维度的查询语法，返回 JSON 或 Markdown 格式结果。
- **版本快照对比**：每次抓取生成数据指纹，支持任意两次快照间的差异比对，便于追踪内容变动。
- **外链校验机制**：自动检测已收录外部链接的有效性，标记失效链接并生成报告，辅助人工清理或更新。
- **自定义分类标签**：允许用户为任意资源添加层级标签，形成个人或团队维度的分类体系。
- **只读模式与缓存策略**：生产环境默认启用只读缓存，降低下游依赖压力，保证核心检索功能的稳定性。

## 应用场景

- **技术资讯聚合**：团队可将本系统部署为内部技术雷达，自动收集指定赛事数据源、技术社区公告板的信息，统一呈现在团队仪表盘上，避免成员频繁切换多个外部站点。
- **数据校验与清洗**：数据分析师可利用快照对比功能，快速定位数据集中的变更字段，辅助清洗流程，识别异常波动或数据缺失。
- **离线文档中心**：在网络受限的内网环境中，本系统可作为离线资源索引层，预先缓存关键外部链接的元信息，供内网用户进行快速主题检索。
- **开源项目依赖追踪**：开源维护者可将本系统用于追踪上游依赖的发布状态或相关比赛结果公告，将非结构化的网页信息转化为结构化的变更提醒。
- **教育科研素材整理**：高校实验室使用本系统归档历年实验数据、竞赛成绩或公开排行榜，形成可追溯的历史数据集，用于教学案例或科研分析。

## 快速开始

以下步骤适用于 Linux/macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 1. 克隆项目仓库
git clone https://github.com/oss-navi/oss-navi-core.git
cd oss-navi-core

# 2. 安装依赖（使用 pip 和虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 初始化配置文件并启动服务
cp config/example.yaml config/production.yaml
python manage.py migrate
python manage.py runserver --host 0.0.0.0 --port 8080
```

访问 http://localhost:8080/status 可查看系统健康状态与已注册数据源列表。首次启动将自动执行一次全量抓取，耗时取决于网络环境与数据源数量。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 或更高（3.11 以上推荐） | 核心运行环境，需包含 sqlite3 和 ssl 模块 |
| pip | 22.0+ | 用于安装 Python 依赖包 |
| SQLite | 3.35+ | 内置数据库，用于存储索引、快照和任务队列 |
| Redis | 6.2+（可选） | 启用缓存和分布式锁时必需，单机模式可忽略 |
| curl / wget | 任意稳定版本 | 健康检查脚本依赖，用于外部连通性测试 |
| cron / systemd | 任意（Linux） | 建议配置定时任务触发增量抓取，非强制 |
| Git | 2.25+ | 仅开发模式需从源码拉取，生产环境可使用发行包 |
| 网络出口 | 允许 TCP 出站 80/443 | 所有数据源均为公网 HTTP/HTTPS 资源，需开放访问 |
| 磁盘空间 | 至少 500MB 可用 | 用于存放索引文件、日志及快照备份 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user-guide/ | 如何配置数据源、执行检索、导出报告、管理标签 |
| 运维手册 | docs/ops/ | 如何部署高可用集群、设置备份策略、调整抓取频率 |
| 开发指南 | docs/contributing/ | 代码规范、PR 流程、新增数据源解析器的方法 |
| API 参考 | docs/api/ | RESTful 接口的请求/响应格式、鉴权方式、分页参数 |
| 设计文档 | docs/design/ | 系统架构图、数据模型 ER 图、快照比对算法原理 |
| 变更日志 | CHANGELOG.md | 每个版本的特性新增、修复项和破坏性变更列表 |
| 安全策略 | SECURITY.md | 漏洞报告渠道、支持的版本范围、安全更新周期 |

## 资源列表

以下为系统默认预置的外部参考数据源，用于初始索引填充和测试验证。所有链接均按原始来源归类，未做任何协议或格式修改。

### 竞技数据类

- <code>ouxielianzigesaibifen.org.cn</code>
- <code>xijiabisaijieguo.net.cn</code>
- <code>ouxielianzigesaijishibifen.org.cn</code>
- <code>ouxielianzigesaijifenbang.org.cn</code>

### 实时比分与赛果

- <code>fajiajishibifen.net.cn</code>
- <code>yingchaobisaijieguo.net.cn</code>

### 赛事综合信息

- <code>yijiasaicheng.net.cn</code>

## 项目结构

```
oss-navi-core/
├── app/                                # 主应用模块
│   ├── collector/                      # 数据抓取子模块
│   │   ├── fetcher.py                 # 异步 HTTP 请求封装，含重试与超时逻辑
│   │   ├── parser.py                  # 针对不同数据源的 HTML/JSON 解析器工厂
│   │   └── scheduler.py               # 基于 APScheduler 的任务调度器
│   ├── index/                         # 索引与检索核心
│   │   ├── inverted.py               # 倒排索引构建与查询实现
│   │   ├── fingerprint.py            # 内容指纹计算（SimHash + MD5）
│   │   └── diff.py                   # 快照对比算法，输出增删改条目
│   ├── api/                           # HTTP 接口层（FastAPI）
│   │   ├── routes/                   # 路由分组：status, search, config, snapshot
│   │   ├── middlewares/              # 限流、日志、缓存中间件
│   │   └── schemas.py                # Pydantic 请求/响应模型
│   ├── models/                        # 数据模型与 ORM（SQLAlchemy）
│   │   ├── source.py                 # 数据源配置表
│   │   ├── snapshot.py               # 快照存储表
│   │   └── tag.py                    # 标签关联表
│   └── utils/                         # 通用工具函数
│       ├── network.py                # 网络连通性探测
│       ├── logger.py                 # 结构化日志配置（JSON 格式）
│       └── validator.py              # URL 校验、格式清洗
├── config/                            # 配置文件目录
│   ├── example.yaml                   # 示例配置（含所有可调参数说明）
│   └── production.yaml                # 生产环境配置（需用户自行填写）
├── scripts/                           # 运维辅助脚本
│   ├── backup.sh                     # 数据库与快照目录打包备份
│   ├── healthcheck.py                # 深度健康检查（依赖、磁盘、网络）
│   └── import_seed.py                # 首次启动时导入预置资源列表
├── tests/                             # 单元测试与集成测试
│   ├── unit/                         # 各模块独立测试用例
│   └── integration/                  # 端到端抓取与检索流程测试
├── docs/                              # 完整文档（见上文导航）
├── requirements.txt                   # 生产依赖锁定列表
├── requirements-dev.txt               # 开发额外依赖（pytest, black, mypy）
├── manage.py                          # 命令行入口（迁移、抓取、清理、导出）
├── README.md                          # 本文件
├── CHANGELOG.md                       # 版本变更记录
└── LICENSE                            # MIT 许可证
```

## 贡献指南

1. **问题反馈与提议**：请先查阅现有 Issue 列表及文档导航中的相关内容。若未覆盖，提交 GitHub Issue 并选择对应模板，描述清晰的重现步骤或提议动机。

2. **分支开发流程**：从 `main` 分支切出 `feature/xxx` 或 `fix/xxx` 工作分支，遵循 PEP 8 编码规范，所有新增函数需包含 docstring 及类型注解。

3. **测试要求**：任何代码变更需确保现有单元测试通过，并为新增逻辑补充至少一个正向测试用例。运行 `pytest tests/unit/` 验证本地修改。

4. **提交信息规范**：使用语义化提交格式，如 `feat: 添加 JSON 解析器对嵌套字段的支持`、`fix: 修复快照对比时时间戳溢出问题`。禁止提交包含敏感信息（密钥、内网地址）的变更。

5. **Pull Request 流程**：提交 PR 至 `main` 分支，需包含变更摘要、测试结果截图（如有 UI）或日志片段。至少一名维护者审核通过后合入。合入即表示贡献者同意将其代码以 MIT 协议发布。

## 常见问题

**Q：系统提示“数据源连接超时”，但浏览器可以正常访问该 URL，如何处理？**

A：首先检查运行环境是否配置了 HTTP 代理，可在 `config/production.yaml` 中的 `network.proxy` 字段设置。其次，部分数据源可能启用反爬机制，建议调整 `collector.interval` 增大抓取间隔，并配置 `collector.user_agent` 为常见浏览器标识。若问题持续，可使用 `scripts/healthcheck.py` 测试具体 URL 的响应详情。

**Q：快照对比结果中产生大量误报差异，如何优化？**

A：误报通常由动态内容（时间戳、广告轮播、随机脚本）引起。可在数据源配置中启用 `parser.clean_rules` 正则过滤规则，移除脚本块、注释和特定 class 的 DOM 节点。更稳定的做法是使用 `fingerprint.ignore_keys` 忽略 JSON 响应中的动态字段（如 `timestamp`、`nonce`）。

**Q：系统是否支持高并发查询，能否横向扩展？**

A：系统默认使用 SQLite 作为存储后端，适合单机部署和低并发场景（QPS < 200）。如需横向扩展，建议替换为 PostgreSQL 并启用 Redis 二级缓存，同时将抓取调度器与 API 服务分离部署。具体配置可参考 `docs/ops/cluster-deployment.md`。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:16:59
