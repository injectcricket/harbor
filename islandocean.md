# LinkVault Navigator

LinkVault Navigator 是一个面向技术团队与独立开发者的结构化外链资源汇总与管理平台。该项目并非传统意义上的爬虫或书签工具，而是一个基于静态站点生成逻辑的资源索引中间件，专注于将散落于网络的高价值技术文档、社区论坛、数据看板与基础设施状态页进行语义化组织与版本化呈现。项目目标用户包括运维工程师、技术调研人员、架构师以及开源项目维护者，旨在解决多源信息碎片化、查找效率低下以及外链失效不可追溯等核心痛点。

项目核心定位为“技术资源的入口层”，不存储任何第三方文件内容，仅保留元数据与访问路径。通过定义统一的资源清单规范（URL Manifest Specification），LinkVault Navigator 支持对原始链接进行标签注入、状态探活（被动模式）与访问路径重写，从而在不改变目标资源原有访问方式的前提下，提供一层轻量级的可观测性与导航增强。项目本身不依赖任何外部数据库，所有资源配置均通过声明式文件管理，便于版本控制与团队协作。

## 功能概览

- **声明式资源注册**：通过 YAML 与 JSON 双格式支持定义资源条目，包含必填字段（原始 URL、类别、优先级、检测间隔）与可选字段（维护人、备注、备用镜像）。所有变更可追溯。

- **被动健康检查引擎**：基于 HTTP 状态码与响应时间百分位（P95）对已注册外链进行定期探活，结果以结构化日志形式输出，支持对接 Prometheus 等监控系统。不主动修改目标资源。

- **多维度分类视图**：内置“技术社区”、“官方文档”、“数据看板”、“状态监控”、“学习资源”五类一级分类，支持用户自定义标签组合，生成多级筛选视图。

- **访问路径重写与短链映射**：提供可选的路径前缀重写规则（例如将原始长路径映射为 `/r/{id}` 形式），便于内部系统集成或印刷物料使用。映射表存储在本地 SQLite 中。

- **变更审计与快照对比**：每次资源清单变更（增删改）自动生成差异报告（diff），并保留最近 10 次快照，支持回滚至任意历史版本。

- **静态站点生成模式**：内置模板引擎（Go Template），可将资源列表渲染为纯静态 HTML 页面，适用于内网文档中心或 GitHub Pages 发布。输出产物不包含任何动态脚本。

- **CLI 交互工具**：提供命令行子命令，包括 `validate`（校验清单格式）、`sync`（更新映射缓存）、`serve`（启动本地管理 API）和 `build`（生成静态站点）。

## 应用场景

- **团队内部技术雷达维护**：技术委员会可利用 LinkVault Navigator 统一管理季度推荐的技术栈文档链接、社区讨论帖与实验性项目地址，新成员入职时通过一份清单即可获取全部关键入口，避免信息孤岛。

- **微服务依赖状态总览**：运维团队可将各中间件（如 Redis、Kafka、Nginx）的官方状态页、版本发布公告栏与性能基线看板注册至系统，配置定时探活后，异常状态通过 Webhook 推送至飞书或钉钉群，实现被动监控覆盖。

- **开源项目外部引用审计**：开源项目维护者可将项目中引用的所有第三方库文档、API 参考站点与镜像源地址纳入统一管理，每次发版前运行 `validate` 命令检查所有外链的可达性，有效减少因外部链接失效导致的 issue 反复。

- **离线文档中心前置导航**：对于网络隔离环境，管理员可通过本项目的静态站点生成功能，将外链清单导出为内部导航页，配合本地 DNS 重定向或反向代理，使隔离区用户仍能通过统一入口访问已同步的内部镜像资源。

- **技术调研信息聚合**：调研人员在进行竞品分析或方案选型时，可将数十个候选产品的官网、技术白皮书、API 测试台与社区反馈链接集中管理，利用分类视图快速横向对比，避免浏览器标签页爆炸。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL2 环境，需提前安装 Git 与 Go 1.21+ 或 Docker（二选一）。

使用 Git 克隆项目主仓库并进入工作目录：

```bash
git clone https://github.com/linkvault/navigator.git
cd navigator
```

安装项目依赖（若使用 Go 环境）：

```bash
go mod download
go build -o bin/linkvault ./cmd/linkvault
```

复制默认配置模板并编辑资源清单（示例文件位于 `config/sample.manifest.yaml`）：

```bash
cp config/sample.manifest.yaml config/my.manifest.yaml
vim config/my.manifest.yaml
```

运行本地管理服务（默认监听 8080 端口）：

```bash
./bin/linkvault serve --config config/my.manifest.yaml --port 8080
```

访问本地 API 根路径 `/api/v1/resources` 可获取已注册资源列表。若需生成静态站点，执行：

```bash
./bin/linkvault build --config config/my.manifest.yaml --output ./public
```

生成产物位于 `./public/index.html`，可直接使用浏览器打开或部署至任意静态托管服务。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Go 编译器 | 1.21 及以上 | 项目主语言，用于编译 CLI 工具与核心逻辑。若使用预编译二进制包可跳过此项。 |
| Git | 2.25 及以上 | 用于克隆仓库及版本管理。部分 CI 场景下需支持子模块更新。 |
| SQLite3 | 3.35 及以上（嵌入式） | 用于存储映射关系与审计日志。项目通过 `modernc.org/sqlite` 驱动纯 Go 实现，无需单独安装系统库，但若启用 CGO 则需系统级 SQLite 头文件。 |
| GNU Make | 3.81 及以上（可选） | 用于执行 Makefile 中的自动化任务（如格式化、测试、交叉编译）。非强制，但推荐。 |
| Docker Engine | 20.10 及以上（可选） | 若选择容器化部署方式，需安装 Docker。项目提供多阶段构建 `Dockerfile`。 |
| 网络连通性 | 出站 443/80 端口可达 | 被动探活功能需访问外部资源，若内网环境需配置代理或关闭探活功能。 |

## 文档导航

| 层面 | 目录/章节 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/user-guide/manifest-syntax.md` | 如何编写资源清单 YAML 文件？必填字段和可选字段有哪些？标签命名规范是什么？ |
| 用户手册 | `docs/user-guide/health-check.md` | 被动健康检查的工作机制是什么？探活超时如何设置？如何解读检查结果日志？ |
| 用户手册 | `docs/user-guide/static-generation.md` | 静态站点生成支持哪些主题？如何自定义模板？输出目录结构是怎样的？ |
| 运维参考 | `docs/operations/api-endpoints.md` | 管理 API 提供了哪些路由？请求与响应的 JSON 结构为何？鉴权如何配置？ |
| 运维参考 | `docs/operations/configuration.md` | 全局配置文件（`config.yaml`）支持哪些参数？环境变量覆盖规则是什么？ |
| 设计文档 | `docs/design/architecture.md` | 项目整体架构图、模块划分、数据流方向以及扩展点设计。 |
| 贡献指南 | `CONTRIBUTING.md` | 如何提交补丁？代码风格检查工具是什么？测试用例如何编写与运行？ |

## 资源列表

本列表包含项目批次 148/150 的全部原始外链，按类别划分。所有链接均保持用户提供的原始格式，未做任何协议补全或域名规范化处理。

技术社区与讨论区

- <code>yingchaosaicheng.net.cn</code>
- <code>ruidianchaosaicheng.net.cn</code>

数据看板与统计平台

- <code>fenchaojifenbang.net.cn</code>
- <code>fenchaobifen.net.cn</code>

官方文档与开发者门户

- <code>bajiajifenbang.net.cn</code>
- <code>dejiasaicheng.net.cn</code>

基础设施状态与监控

- <code>yijiajishibifen.net.cn</code>

## 项目结构

项目遵循 Go 语言标准布局，核心代码与配置分离，文档与脚本独立存放。

```
navigator/
├── cmd/
│   └── linkvault/                # 主入口命令行程序，解析子命令并初始化环境
├── internal/
│   ├── core/                     # 核心业务逻辑：资源注册、校验、快照对比
│   ├── health/                   # 被动探活引擎：HTTP 客户端封装、超时控制、状态记录
│   ├── storage/                  # 存储层：SQLite 迁移、CRUD 操作、审计日志写入
│   ├── render/                   # 静态站点渲染：模板解析、输出目录生成、静态资产复制
│   └── api/                      # RESTful API 路由：Gin 框架初始化、中间件、请求验证
├── pkg/
│   └── types/                    # 可导出的数据结构定义，供外部 import 使用（如 Manifest 结构体）
├── config/
│   ├── default.yaml              # 全局默认配置（端口、日志级别、探活间隔）
│   └── sample.manifest.yaml      # 资源清单示例，包含注释说明与字段模板
├── docs/                         # 完整文档目录，结构与上述文档导航章节一一对应
├── scripts/
│   ├── pre-commit.sh             # Git pre-commit 钩子，执行格式化与静态检查
│   └── docker-entrypoint.sh      # 容器启动脚本，用于初始化数据库与迁移
├── test/
│   ├── fixtures/                 # 测试用的模拟清单文件与预期输出
│   └── integration/              # 集成测试用例，依赖真实 SQLite 文件与临时端口
├── web/
│   └── templates/                # 静态站点的 Go 模板文件（base, index, detail）
├── Makefile                      # 统一任务入口：编译、测试、覆盖率、容器构建
├── Dockerfile                    # 多阶段构建文件，基于 Alpine 镜像
├── go.mod                        # Go 模块依赖定义，含间接依赖与替换指令
└── README.md                     # 项目主文档（即本文件）
```

## 贡献指南

项目遵循常规 GitHub Pull Request 工作流，所有贡献需遵守开发者原意证书（DCO）签署要求。

1.  **分支与提交规范**：从 `main` 分支创建特性分支，命名格式为 `feature/{issue-id}-{short-desc}` 或 `fix/{issue-id}-{short-desc}`。提交信息需符合 Conventional Commits 规范（如 `feat: add timeout flag for health check`），每个提交仅包含一个逻辑变更。

2.  **代码风格与静态检查**：提交前需运行 `make lint`，确保通过 golangci-lint 所有检查项（配置位于 `.golangci.yml`）。同时执行 `make fmt` 自动格式化代码。CI 流水线将阻止未通过检查的 PR 合并。

3.  **单元测试与集成测试**：所有新增功能或修复必须附带对应的测试用例。单元测试文件置于对应包目录下（`*_test.go`），集成测试置于 `test/integration`。运行 `make test` 确保全量测试通过，覆盖率不低于 80%。

4.  **文档同步更新**：若变更涉及用户可见功能（如新增 CLI 参数、修改 API 响应结构），需同步更新 `docs/` 下对应文档，并在 PR 描述中标注文档变更位置。文档采用 Markdown 格式，术语需与代码保持一致。

5.  **签署开发者原意证书**：每个提交需包含 `Signed-off-by: Your Name <email>` 行（可通过 `git commit -s` 自动添加），以证明您有权贡献且同意项目许可证条款。

## 常见问题

**Q1：被动健康检查是否会频繁访问目标资源，导致对方服务器压力过大？**

被动探活引擎默认采用线性间隔策略，每个资源的最小检查间隔为 5 分钟，且所有探活请求均携带 `User-Agent: LinkVault-HealthCheck/1.0` 头部，便于目标服务识别。并发请求数由配置项 `health.max_concurrent` 限制（默认 4）。此外，引擎仅发送 HEAD 请求（若目标支持），否则降级为 GET 并提前中断响应体读取，最大下载限制为 128KB。实际运行中，单个资源的日均请求量不超过 288 次（按 5 分钟间隔计算），不会对常规 Web 服务造成压力。

**Q2：静态站点生成后，页面中的链接是原始地址还是重写后的短链？**

生成行为由清单中的 `render.use_rewritten` 布尔值控制。若设置为 `true`，则所有资源链接替换为 `/r/{id}` 短链格式，并同时生成一份 `redirects.json` 映射文件供反向代理使用。若设置为 `false`（默认），则直接输出原始 URL。两种模式均会保留资源元数据（标题、分类、标签）的渲染，不影响页面导航结构。

**Q3：如何在无法出网的内网环境中使用健康检查功能？**

可通过配置 `health.mode: "off"` 完全关闭探活引擎，此时项目仅作为静态资源索引与映射服务使用。若仍需部分探活能力，可配置 `health.proxy` 字段指向内网可访问的 HTTP 代理，或使用 `health.allow_domains` 白名单限定仅对特定域名发起请求。对于完全隔离环境，建议仅使用 `build` 子命令生成静态站点，不启动任何后台协程。

## 许可证

MIT License

Copyright (c) 2026 LinkVault Contributors

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

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:41
