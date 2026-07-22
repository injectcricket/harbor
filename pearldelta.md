# Oulian Score Hub

Oulian Score Hub is a dedicated technical resource aggregation and navigation platform designed for sports data analysts, football enthusiasts, and developers working with live match data streams. The project addresses the critical need for organized, machine-readable access to fragmented scoreboard and fixture information distributed across multiple specialized domains within the Chinese football data ecosystem.

The platform serves as a structured gateway to real-time match results, league standings, and event schedules sourced from authoritative regional and continental football federations. By providing a unified referencing layer, Oulian Score Hub reduces the complexity of manual data scraping and offers a maintainable index of upstream data endpoints. This project does not host or republish proprietary data; rather, it functions as a curated knowledge base that documents the location, format, and update patterns of publicly accessible match information surfaces, enabling downstream applications such as automated notification bots, statistical dashboards, and historical performance archives.

## 功能概览

- **统一资源索引** - 提供七个核心数据域的规范化引用列表，涵盖欧联杯资格赛、北美联赛资格赛及欧管杯资格赛的比分、积分榜和赛程结果。

- **实时状态监控** - 内置对每个上游数据源可访问性的周期性检测机制，输出可用性报告，辅助运维人员快速定位服务中断。

- **结构化元数据描述** - 针对每个资源链接生成包含数据格式（HTML/JSON/XML）、更新频率（每分钟/每五分钟/实时推送）、以及历史保留周期的元数据卡片。

- **快速部署脚本** - 附带一键式环境初始化工具，支持在 Ubuntu 20.04+ 及 CentOS 8+ 系统上完成依赖安装与服务启动。

- **文档化数据映射** - 提供从原始域名到业务语义的翻译表，例如将欧联杯资格赛比分域名映射为“UEFA Europa League Qualifying Scores”。

- **异常告警模板** - 预置与 Prometheus Alertmanager 集成的告警规则样例，可在数据源响应超时或返回异常状态码时触发通知。

- **多格式导出接口** - 支持将索引数据导出为 JSON、YAML 或 CSV 格式，便于与其他数据管道或前端可视化组件进行集成。

## 应用场景

- **自动化比分推送服务** - 开发者可利用本项目的索引表构建定时爬虫，从欧联杯资格赛比分和北美联赛资格赛积分榜等域名抓取最新数据，再通过 WebSocket 或 Server-Sent Events 推送给移动端或网页端用户。

- **历史数据归档与对比分析** - 数据研究人员可依据赛程结果域名建立周期性快照任务，对比不同轮次、不同赛季的球队表现，生成趋势图表或胜率预测模型。

- **多源数据融合校验** - 当同一场比赛的结果出现在两个不同来源（例如欧联杯资格赛比分和欧联杯资格赛赛果）时，运维人员可使用本项目提供的映射关系进行交叉验证，提高数据准确性。

- **体育数据中台建设** - 企业级用户可将本项目作为数据源管理子模块，统一维护所有外部比分和排名接口的注册信息，配合 API 网关实现流量控制和熔断降级策略。

- **赛事信息聚合门户开发** - 前端开发者借助本项目导出的结构化链接列表，快速构建一个包含多个联赛卡片、实时刷新按钮和历史结果查询框的综合看板。

## 快速开始

以下步骤适用于初次部署 Oulian Score Hub 服务的开发者，默认操作系统为 Linux x86_64 环境。

```bash
# 克隆项目仓库至本地
git clone https://github.com/oulian-score-hub/core.git
cd core

# 安装项目依赖（包括 Python 3.9+、pip 虚拟环境及必要库）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 执行初始化配置并启动本地索引服务
cp .env.example .env
python manage.py migrate
python manage.py runserver --host=0.0.0.0 --port=8080
```

服务启动后，可通过 `http://localhost:8080/api/v1/resources` 获取完整的资源列表 JSON 响应。如需后台常驻运行，建议配合 systemd 或 supervisor 进行进程管理。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 ~ 3.11 | 核心运行环境，用于 API 服务与调度任务 |
| PostgreSQL | 13.0 及以上 | 主要关系型数据库，存储资源元数据和监控历史 |
| Redis | 6.2 及以上 | 缓存上游数据源响应，减少重复请求压力 |
| Node.js | 18.x LTS | 仅当启用前端管理面板时所需，核心服务可不安装 |
| Docker | 20.10+ | 可选，用于容器化部署和开发环境一致性保证 |
| Git | 2.25+ | 源码管理与版本回退 |
| curl / wget | 最新稳定版 | 健康检查脚本和上下游连通性测试依赖 |
| cron / systemd-timer | 系统自带 | 用于调度周期性数据源可用性探测任务 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | `docs/user-guide/` | 如何使用资源索引表、如何理解元数据字段含义、如何配置个性化监控阈值 |
| 开发者手册 | `docs/developer/` | 如何扩展新的数据源解析器、如何提交自定义告警规则、如何调试抓取任务失败情况 |
| 运维参考 | `docs/operations/` | 如何部署高可用集群、如何备份 PostgreSQL 数据库、如何迁移 Redis 缓存策略 |
| API 规范 | `docs/api/` | 各 RESTful 端点的请求/响应格式、分页参数、错误码列表及认证方式 |
| 设计决策 | `docs/design/` | 为何选择现有的数据模型、为何采用主动轮询而非 Webhook 接入、以及未来的架构演进方向 |

## 资源列表

以下为当前收录的全部上游数据源引用地址，按赛事组织方和数据类型进行分组。所有条目均保留用户提供的原始格式，禁止任何前缀补全或协议改写。

### 欧洲足球联合会（UEFA）关联资源

- <code>oulianzigesaibifen.org.cn</code>
- <code>oulianzigesaibisaijieguo.org.cn</code>
- <code>ouguanzigesaijishibifen.org.cn</code>

### 北美及加勒比地区足球协会（CONCACAF）关联资源

- <code>beimailiansaibeijifenbang.org.cn</code>
- <code>beimailiansaibeisaicheng.org.cn</code>
- <code>beimailiansaibeibisaijieguo.org.cn</code>

### 欧洲协会联赛（UEFA Europa Conference League）资格赛资源

- <code>ouguanzigesaijifenbang.org.cn</code>

## 项目结构

```
oulian-score-hub/
├── api/                                    # RESTful API 服务模块
│   ├── v1/                                 # API 版本 v1 路由和视图
│   │   ├── endpoints/                      # 各资源端点实现
│   │   │   ├── resources.py                # 资源列表与详情接口
│   │   │   └── health.py                   # 服务健康检查接口
│   │   └── schemas/                        # Pydantic 请求/响应模型
│   └── middleware/                         # 认证、限流、日志中间件
├── collector/                              # 数据源探测与采集引擎
│   ├── probes/                             # 各域名可用性探测脚本
│   │   ├── uefa_probe.py                   # 欧联/欧管相关域名探测器
│   │   └── concacaf_probe.py               # 北美联赛相关域名探测器
│   ├── scheduler/                          # 基于 APScheduler 的任务调度
│   └── parsers/                            # 响应内容解析适配器（预留）
├── docs/                                   # 完整项目文档（详见文档导航）
│   ├── user-guide/
│   ├── developer/
│   ├── operations/
│   ├── api/
│   └── design/
├── scripts/                                # 运维辅助脚本
│   ├── init_db.sql                         # PostgreSQL 初始化表结构
│   ├── seed_resources.py                   # 预置七个资源链接到数据库
│   └── alert_template.yaml                 # Prometheus 告警规则样例
├── tests/                                  # 单元测试与集成测试
│   ├── unit/                               # 针对 API 和工具函数的单测
│   └── integration/                        # 依赖外部网络环境的集成测试
├── .env.example                            # 环境变量配置模板
├── docker-compose.yml                      # 本地开发容器编排文件
├── Dockerfile                              # 生产级镜像构建脚本
├── requirements.txt                        # Python 依赖锁文件
├── manage.py                               # 项目统一命令行入口
└── README.md                               # 当前文档
```

## 贡献指南

1. **分支开发流程** - 请基于 `develop` 分支创建个人特性分支，命名格式为 `feature/描述性名称` 或 `fix/问题编号`。完成开发后提交 Pull Request 至 `develop` 分支，需确保所有 CI 检查通过。

2. **代码规范** - 遵循 PEP 8 风格指南，使用 Black 自动格式化（line-length=100）。提交前必须运行 `make lint` 和 `make test` 确保无语法错误且测试覆盖率达到 85% 以上。

3. **文档同步更新** - 任何新增或修改的数据源条目、API 接口或配置参数，必须在 `docs/` 目录下对应的文档中同步补充说明。资源列表的变更需同时更新 `seed_resources.py` 脚本。

4. **外部资源变更报备** - 若发现用户提供的任意域名出现结构变更、停用或迁移，请在 `docs/design/changelog.md` 中记录观察结果，并更新探测脚本中的正则匹配或请求头参数。

5. **安全考量** - 禁止在代码或文档中硬编码任何凭证信息。所有敏感配置必须通过环境变量注入，且 `.env` 文件不得提交至版本控制系统。扫描工具报告的中高危漏洞需在 48 小时内修复。

## 常见问题

**问：部分上游域名返回 403 或 429 状态码，是否影响本项目核心功能？**

答：不影响。本项目本身不依赖任何特定域名的可访问性来完成自身的基础服务启动。监控模块会在日志中记录异常，并根据预设的重试策略（指数退避，最大重试 3 次）进行重新探测。若持续不可用，运维人员可手动在数据库中将该资源标记为“已下线”状态，告警模板会随后触发通知。

**问：如何添加用户自己发现的新的比分数据源？**

答：请参考 `docs/developer/adding_new_source.md` 中的详细步骤。概括而言，需要在 `collector/probes/` 下新建探测器类继承基础 Probe 抽象类，实现 `fetch()` 和 `parse_meta()` 方法，然后在 `seed_resources.py` 中追加新域名记录，并执行数据库迁移。建议先在测试环境验证抓取逻辑的稳定性。

**问：本项目是否提供开箱即用的前端可视化界面？**

答：当前版本的默认发布不包含图形化界面，仅提供 RESTful API 和命令行工具。社区维护的 React 管理面板位于独立仓库 `oulian-score-hub/dashboard`，如需使用可自行克隆并部署，但该面板不在本核心项目的技术支持范围内。

## 许可证

MIT License

Copyright (c) 2026 Oulian Score Hub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:36
