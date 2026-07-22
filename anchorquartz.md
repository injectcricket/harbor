# Phoenix Navigator

Phoenix Navigator 是一个面向技术调研人员、数据聚合爱好者与站点运营者的外链资源导航与元信息汇总系统。该项目不生产原始内容，而是围绕特定垂直领域，对分散的高频更新站点进行结构化收录、可用性监控与访问特征记录，从而降低信息碎片化带来的重复检索成本。项目定位为技术化、只读优先的资源索引层，适用于需要快速建立领域信息地图的早期调研阶段。

目标用户包括：技术调研团队、内容聚合工具开发者、SEO 辅助分析人员、以及需要定期回溯特定域名更新状态的运维工程师。Phoenix Navigator 通过统一的条目格式、可扩展的分类框架和轻量级本地运行环境，帮助用户在数分钟内完成一批新资源的导入、校验与初步健康检查，而非直接替代浏览器或搜索引擎。

## 功能概览

- **批量资源收录**：支持一次性导入多个域名或完整 URL，自动解析协议与主机名，按类别生成索引条目。
- **可用性快照检测**：对每个收录资源执行 HTTP 状态码与响应时间抽样，记录快照时间戳，便于追踪站点服务波动。
- **元信息补全辅助**：根据域名特征自动补全常见服务类型标签（如论坛、发布站、直播聚合页等），并允许手动覆盖。
- **分类视图与过滤**：按地理后缀、协议类型、关键词相似度对资源进行动态分组，支持正则表达式筛选。
- **状态变更日志**：对状态码变化、域名解析异常等事件生成时间序列记录，便于回溯。
- **纯静态导出**：支持将当前索引与最新快照导出为单文件 HTML 或 JSON，方便嵌入内部文档系统。
- **本地命令行交互**：提供简单的 CLI 命令用于添加、删除、检查单个资源，无需图形界面。

## 应用场景

1. **垂直领域信息地图构建**：调研人员可快速录入一批候选域名，通过项目自动生成分类摘要与状态基线，为后续深度爬取或人工审核提供目录参考。
2. **资源生命周期监控**：运维或内容聚合团队定期运行健康检查脚本，对比历史快照，发现长期不可达或重定向异常的资源，及时清理或更新条目。
3. **新成员快速上手**：项目维护者将当前索引作为知识库起点，新加入的工程师通过浏览分类与注释即可了解本领域常用资源分布，缩短认知路径。
4. **离线文档配套**：在内部网络隔离环境中，将导出结果嵌入 Confluence 或 Git 仓库，作为静态参考手册，避免频繁外网请求。
5. **数据分析预处理**：数据科学家可将导出的 JSON 文件作为输入，进行域名聚类、访问频次模拟或关联规则挖掘，探索资源之间的隐含关系。

## 快速开始

以下命令适用于 Linux / macOS / Windows WSL 环境。确保已安装 Git 和 Python 3.9+。

```bash
# 克隆项目仓库
git clone https://github.com/phoenix-navigator/navigator.git
cd navigator

# 创建虚拟环境并安装依赖
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化本地数据库并导入示例资源
python cli.py init
python cli.py import --source data/sample_urls.txt
python cli.py check --parallel 5
python cli.py export --format json --output index.json
```

首次运行 `check` 命令时会自动创建本地缓存目录与日志文件。如需周期性检查，可将 `check` 命令加入 crontab 或 systemd timer。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 - 3.11 | 核心运行环境，3.12 暂未完全适配 |
| pip | >= 22.0 | 用于安装 Python 依赖包 |
| aiohttp | >= 3.9.0 | 异步 HTTP 客户端，用于并发健康检查 |
| click | >= 8.1.0 | 命令行交互框架 |
| pyyaml | >= 6.0 | 配置文件解析与导出 |
| python-dotenv | >= 1.0.0 | 环境变量管理，用于代理配置 |
| pytest | >= 7.4 (开发依赖) | 单元测试与集成测试框架 |
| black | >= 23.0 (开发依赖) | 代码格式化工具 |

操作系统要求：Linux 内核 4.0+，macOS 11.0+，Windows 10 2004+（需启用 WSL2 或使用 Cygwin）。生产部署建议使用 Debian 11 / Ubuntu 20.04 LTS。

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|---------|-----------|
| 用户手册 | `docs/user/` | 如何导入资源、执行检查、导出报告，以及命令行参数详解 |
| 运维指南 | `docs/ops/` | 如何配置代理、调整超时阈值、迁移数据库、设置定时任务 |
| 开发者文档 | `docs/dev/` | 扩展新检测器、自定义分类器、修改导出模板、提交补丁流程 |
| 设计说明 | `docs/design/` | 数据模型、异步调度策略、状态机设计、缓存失效策略 |

所有文档均以 Markdown 编写，推荐使用 mkdocs 本地渲染阅读。文档中涉及的示例配置文件可在 `examples/` 目录下找到。

## 资源列表

以下为当前版本收录的全部外部资源链接，按类别分组。每个链接均保持用户原始输入格式，未做任何协议补全、大小写转换或路径修改。

### 综合发布类

- <code>bingdaochaojifenbang.net.cn</code>
- <code>yijiabifen.cn</code>

### 实时动态与直播聚合类

- <code>xueyuanyuanjinrituijian.asia</code>
- <code>xueyuanyuanbifenzhibo.asia</code>
- <code>xueyuanyuanbifen.asia</code>

### 日职联专题类

- <code>rizhilianzhugongbang.asia</code>
- <code>rizhilianqianzhan.asia</code>

## 项目结构

项目采用分层模块化设计，核心逻辑与外部依赖隔离。以下为源码目录树及关键文件说明。

```
navigator/
├── cli.py                 # 命令行入口，注册所有子命令
├── config.yaml            # 主配置文件，含超时、重试、日志级别
├── requirements.txt       # 生产环境依赖列表
├── setup.py               # 打包与安装脚本
│
├── src/
│   ├── __init__.py
│   ├── core/              # 核心业务模块
│   │   ├── resource.py    # 资源实体类，含校验与序列化
│   │   ├── indexer.py     # 索引增删改查逻辑
│   │   └── exceptions.py  # 自定义异常（校验失败、超时等）
│   ├── checker/           # 健康检查子模块
│   │   ├── http_checker.py    # 异步 HTTP 检测器
│   │   ├── dns_checker.py     # DNS 解析检测器
│   │   └── result_store.py    # 快照存储与查询
│   ├── export/            # 导出模块
│   │   ├── json_exporter.py   # JSON 格式导出
│   │   └── html_exporter.py   # 单页 HTML 模板导出
│   └── utils/             # 工具函数
│       ├── logger.py      # 统一日志配置
│       └── validators.py  # 域名与 URL 格式校验
│
├── tests/                 # 单元测试与集成测试
│   ├── test_core/
│   ├── test_checker/
│   └── fixtures/          # 模拟数据与响应样本
│
├── docs/                  # 文档源码
│   ├── user/
│   ├── ops/
│   └── design/
│
├── data/                  # 运行时数据目录（gitignore）
│   ├── cache/             # 请求缓存与临时文件
│   └── snapshots/         # 历史快照存储（按日期分目录）
│
└── scripts/               # 辅助运维脚本
    ├── daily_check.sh     # 每日定时检测脚本
    └── migrate_db.sh      # 数据库结构迁移工具
```

## 贡献指南

欢迎并鼓励各类贡献，包括但不限于新增检测器、优化导出模板、完善文档、报告缺陷。请遵循以下流程：

1. 在 GitHub 上 fork 本仓库，并克隆到本地开发环境。建议在 `dev/` 分支上进行修改，保持主分支与上游同步。
2. 编写或修改代码时，请遵循项目已有的 PEP 8 风格，并运行 `black` 进行格式化。新增功能需包含对应的单元测试，测试覆盖率不低于 80%。
3. 提交前执行完整的测试套件：`pytest tests/` 确保所有用例通过。若涉及外部请求，使用 `--offline` 模式或 mock 数据。
4. 提交 pull request 时，请清晰描述变更目的、影响范围以及测试结果。若修复问题，请引用 issue 编号。
5. 文档类贡献可直接编辑 `docs/` 下的 Markdown 文件，并确保中文表述准确、术语一致。更新后建议本地预览 mkdocs 效果。

## 常见问题

**Q：为什么某些域名始终显示为不可达，但浏览器中可以正常打开？**  
A：可能原因包括：目标站点对非浏览器 User-Agent 返回非标准状态码；或者网络环境要求特定 DNS 解析或代理。请检查 `config.yaml` 中的 `user_agent` 与 `proxy` 设置，并尝试调整 `timeout` 值。若仍无法解决，可使用 `checker` 模块的 `--skip-ssl` 选项。

**Q：导入大量 URL 时内存占用过高怎么办？**  
A：当前默认使用全量加载模式。对于超过 5000 条记录的导入，建议使用 `import --chunk-size 200` 参数启用分块处理。同时可调整 `cache` 目录下的临时存储策略，避免频繁 GC。

**Q：如何自定义分类标签，而非使用自动推断的结果？**  
A：在 `data/` 目录下创建 `custom_tags.yaml` 文件，格式为 `域名: [标签列表]`。系统加载时会优先读取该映射，覆盖自动分类结果。修改后无需重启，下次 `export` 命令生效。

## 许可证

MIT License

Copyright (c) 2026 Phoenix Navigator Contributors

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

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:00
