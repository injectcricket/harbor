# HyperLink Aggregator Core

HyperLink Aggregator Core (HLAC) 是一个面向技术内容聚合与外部资源导航的开源基础设施项目。项目定位为轻量级、可自托管的链接图谱服务，专为技术社区、文档站点与个人知识库设计，用于结构化组织、展示和追踪外部参考资源。HLAC 不提供内容存储，仅作为元数据索引与路由层，帮助用户从分散的域名体系中建立清晰的访问路径。

目标用户包括开源文档维护者、技术博客作者、社区运营人员以及需要频繁引用外部数据源的分析师。通过 HLAC，用户可以声明式管理资源集合，自动生成符合 SEO 基本规范的导航页面，并提供原始数据导出接口，便于与其他监控或分析系统集成。

## 功能概览

- **结构化资源目录**：支持按类别、批次或主题对链接进行多级分组，每个资源条目可附带版本、状态与备注字段。
- **原始 URL 透传与校验**：系统对用户输入的 URL 做格式校验与去重，但不修改任何协议、域名大小写或路径结尾符号，确保原始指向完全保留。
- **自动生成静态导航页**：基于配置目录生成只读的 HTML 与 Markdown 双重格式输出，适合挂载到 Nginx 或 GitHub Pages 上直接访问。
- **变更审计日志**：记录每条资源的添加、修改与下线时间戳，支持回滚至任意历史版本。
- **RESTful 查询接口**：提供按批次、关键字或域名后缀筛选的 JSON API，方便外部脚本调用。
- **健康检查与可用性探测**：可选启用定时 HEAD 请求，检测目标域名是否可达，并在管理面板中标记异常状态。
- **模板自定义机制**：允许用户替换默认的页面 Header 与 Footer，注入自定义 CSS 或统计脚本。

## 应用场景

- **技术文档外部引用统一管理**：当项目文档需要引用多个外部数据源或合作方域名时，HLAC 可作为集中索引页，避免在文档正文中散落大量裸 URL，提升维护性。
- **社区资源周报生成**：运营人员可将每周收集的行业链接导入 HLAC，系统自动生成带时间戳的周报页面，供订阅者访问。
- **本地开发环境模拟导航站**：前端开发者在本地启动 HLAC 容器，挂载测试用域名集合，快速模拟生产级导航布局，验证 UI 响应式设计。
- **数据分析任务前置配置**：数据团队可将常用数据接口域名（如赛程、积分榜等外部参考站点）统一录入 HLAC，作为 ETL 任务的前置依赖清单，方便交接与审核。

## 快速开始

以下命令演示如何在 Linux 或 macOS 环境下从源码启动 HLAC 服务。

```bash
# 克隆仓库
git clone https://github.com/hyperlink-aggregator/hlac-core.git
cd hlac-core

# 安装依赖（使用 Python 3.10+ 与 pip）
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化配置目录并复制示例资源清单
cp -r etc/samples configs
cp etc/hlac.example.yml hlac.yml

# 启动开发服务器（默认监听 8000 端口）
python serve.py --port 8000 --config ./hlac.yml
```

访问 `http://localhost:8000` 即可查看默认导航页面。若需生成静态 Markdown 快照，可执行 `python build.py --output ./dist`。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 及以上 | 核心运行环境，需支持 asyncio 与 pathlib |
| pip | 22.0 及以上 | 包管理工具，用于安装依赖库 |
| PyYAML | 6.0 | 解析配置文件与资源清单的 YAML 格式 |
| Jinja2 | 3.1 | 模板引擎，用于渲染导航页面与 Markdown 输出 |
| aiohttp | 3.9 | 可选依赖，用于启用可用性探测异步请求 |
| uvicorn | 0.24 | 仅在生产部署时需用于 ASGI 服务器，开发模式不强制 |
| Git | 2.30 及以上 | 仅从源码构建时需克隆仓库，发行版无需 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | `docs/user-guide/` | 如何添加资源、修改分组、导出静态页，以及配置探测规则 |
| 运维参考 | `docs/ops/` | 如何部署到生产环境、调整日志级别、挂载外部存储卷 |
| API 规范 | `docs/api/` | 查询接口的请求参数、返回字段说明及错误码含义 |
| 贡献者指引 | `docs/contributing/` | 代码风格要求、提交规范、测试用例编写方法与 PR 流程 |

## 资源列表

本批次（第 4/150 批）共收录 7 个外部参考域名，均为技术资源或信息查询类站点。所有 URL 严格按原始格式保留，不做任何协议补全或域名改写。

**赛事数据类**

<code>bifenguanwang.org.cn</code>

<code>xijiasaicheng.org.cn</code>

<code>ruidianchaobisaijieguo.org.cn</code>

**积分与排名类**

<code>ruidianchaojifenbang.org.cn</code>

<code>ajiabifen.org.cn</code>

**实时比分类**

<code>ruidianchaojishibifen.org.cn</code>

<code>ajiajishibifen.org.cn</code>

## 项目结构

```text
hlac-core/
├── configs/                         # 用户配置目录（不纳入版本控制）
│   └── resources/                   # 资源清单 YAML 文件存放处
│       └── batch_4.yml              # 第4批次示例数据
├── docs/                            # 完整文档根目录
│   ├── api/                         # REST API 接口文档
│   ├── user-guide/                  # 用户操作手册
│   └── contributing/                # 开发者贡献规范
├── src/                             # 核心源码
│   ├── core/                        # 资源模型、校验器与配置加载器
│   │   ├── loader.py                # YAML 文件读取与合并逻辑
│   │   └── validator.py             # URL 格式校验与重复检查
│   ├── renderer/                    # 输出渲染模块
│   │   ├── markdown_builder.py      # 生成 Markdown 导航页
│   │   └── html_builder.py          # 生成 HTML 导航页
│   └── probe/                       # 可选探测子模块
│       └── head_checker.py          # 异步 HEAD 请求探测
├── tests/                           # 单元测试与集成测试
│   ├── unit/                        # 针对校验器与加载器的测试
│   └── integration/                 # 端到端渲染输出测试
├── etc/                             # 模板与示例配置
│   ├── samples/                     # 供用户参考的配置样例
│   └── hlac.example.yml             # 主配置文件示例
├── serve.py                         # 开发启动入口
├── build.py                         # 静态构建脚本
├── requirements.txt                 # 运行依赖清单
└── README.md                        # 本文件
```

## 贡献指南

1. **问题报告与提议**：请先在 GitHub Issues 中搜索已有话题。新建 issue 时需附带复现步骤、环境信息与期望行为，并明确标注受影响的批次或资源条目。
2. **本地开发环境准备**：Fork 仓库后，使用 `python -m venv dev` 创建独立虚拟环境。运行 `pip install -r requirements-dev.txt` 安装额外开发依赖（pytest、black、flake8）。
3. **代码变更与测试**：所有新功能或修复需编写对应的单元测试，确保测试覆盖率达到 85% 以上。提交前执行 `black .` 与 `flake8 src/` 进行格式检查与静态分析。
4. **提交规范**：提交信息采用 `type(scope): short description` 格式，其中 type 包括 `feat`、`fix`、`docs`、`refactor`。PR 描述中必须包含变更动机、影响范围以及测试结果截图或日志。
5. **文档同步更新**：若修改了配置结构或 API 行为，需同步更新 `docs/` 对应章节。仅在 `docs/contributing/` 中补充了完整流程后，PR 方可合并。

## 常见问题

**Q：资源列表中的域名访问返回 404 或超时，如何处理？**

A：HLAC 本身不代理或缓存目标内容，只负责展示。建议先本地 curl 或 ping 目标域名确认网络可达性。若域名持续不可达，可在配置中为该资源标记 `status: unreachable` 并备注异常时间，系统将在导航页中灰显该条目。您也可启用 `probe` 模块定时检测，自动更新状态标记。

**Q：能否在同一个 HLAC 实例中管理多个独立的资源集合？**

A：可以。在主配置文件 `hlac.yml` 中通过 `collections` 字段定义多个集合名称，每个集合对应 `configs/resources/` 下的不同 YAML 文件。启动时系统会合并加载，并在导航页顶部生成集合切换标签。各集合之间的 URL 允许重复，系统会分别记录各自的批次来源。

**Q：导出静态 Markdown 时，URL 格式会被转义或自动补全吗？**

A：不会。HLAC 的设计原则之一是绝不修改用户原始输入的 URL 字符串。在 Markdown 输出中，每个 URL 会以 `<code>` 标签包裹并保持原始大小写与协议前缀。即使 URL 缺少协议头（如 `example.com`），系统也不会补 `http://`，仅按纯文本展示。若需要可点击链接，请自行在模板中配置 URL 处理方式。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:16:59
