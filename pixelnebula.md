# RizhiLink Hub

RizhiLink Hub 是一个面向数据分析师、运维工程师与技术研究人员的轻量级外链资源聚合与导航系统。该项目并非传统的爬虫或采集工具，而是一个人工维护的高质量技术资源索引中间件，旨在解决技术社区中优质分析站点、实时数据面板与行业动态来源分散、难以快速定位的问题。通过结构化的资源分类、极简的部署方式以及清晰的文档导航，RizhiLink Hub 帮助用户在数秒内抵达所需的技术信息源头，显著降低信息检索成本。

项目目标用户包括需要频繁查阅第三方行业数据的中小企业技术团队、独立开发者、金融科技领域的风控人员，以及对特定垂直领域（如实时比分、行业指数、辅助决策参考）有持续跟踪需求的个人研究者。RizhiLink Hub 不生成任何数据，也不对下游内容进行二次封装，其唯一职责是提供可验证、可追溯、且经过基础可用性校验的链接索引服务。项目本身采用静态页面架构，支持容器化部署，可嵌入现有内部文档站点或作为独立导航页运行。

## 功能概览

- **分类资源索引**：将收录的链接按业务领域与使用频率划分为多个逻辑分组，支持快速筛选与定位，降低用户在海量书签中的查找时间。

- **可用性状态标识**：每个收录链接均附带基础连通性检测机制，在页面展示时标记可访问状态，便于用户识别当前有效的资源入口。

- **自定义分组标签**：允许用户通过配置文件对链接添加自定义标签（如“实时”“日报”“备份站”），实现多维度分类，满足个性化组织需求。

- **搜索与过滤**：提供基于关键词和标签的实时搜索功能，支持对资源标题、描述及所属分类进行全文检索，返回高相关度结果。

- **轻量级管理面板**：内置基于环境变量的管理接口，支持增删改查操作，无需数据库即可完成资源清单的动态维护，适合快速迭代场景。

- **响应式布局**：页面适配桌面端与移动端设备，确保在不同屏幕尺寸下均能获得一致的浏览体验，满足移动办公需求。

- **开放数据导出**：支持将当前资源列表导出为 JSON 或 CSV 格式，便于用户迁移至其他工具或进行二次分析。

## 应用场景

- **技术团队内部知识库导航**：企业技术部门可将 RizhiLink Hub 部署为内部文档站的首页入口，集中存放常用的运维监控面板、日志分析工具与行业资讯站点，新成员入职时可快速了解团队常用的信息渠道。

- **实时数据跟踪工作台**：金融或体育数据分析师可将项目部署在本地服务器，集中管理多个实时数据来源（如比分、指数、预测参考），通过单一页面同时监控多个数据源的状态变化，避免频繁切换浏览器标签。

- **个人研究环境的信息中继站**：独立研究员或开源爱好者可使用该项目作为个人浏览器起始页，将长期跟踪的行业报告发布页、技术社区讨论串、以及官方统计数据库入口统一归档，配合搜索功能实现高效回溯。

- **离线环境下的资源清单备份**：对于网络隔离环境，RizhiLink Hub 可作为纯静态资源清单的载体，定期同步更新后的链接列表，确保在受限网络条件下仍能获取最新的参考入口信息。

## 快速开始

以下操作基于 Ubuntu 22.04 LTS 及 Node.js 18.x 环境。请确保系统已安装 Git 与 npm。

```bash
# 克隆项目仓库
git clone https://github.com/rizhilink/rizhilink-hub.git
cd rizhilink-hub

# 安装项目依赖
npm install --production

# 构建静态页面并启动开发服务器
npm run build
npm start
```

构建完成后，默认服务监听 `http://localhost:3000`。如需更改端口或绑定主机地址，请修改项目根目录下的 `.env` 文件中的 `PORT` 与 `HOST` 变量。生产环境部署建议使用 `npm run build` 生成静态文件后，交由 Nginx 或 Caddy 托管 `dist` 目录。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x LTS 及以上 | 项目运行时环境，用于执行构建脚本与启动服务 |
| npm | 9.x 及以上 | 包管理器，用于安装项目依赖与执行脚本命令 |
| Git | 2.25 及以上 | 版本控制工具，用于克隆仓库及后续更新 |
| 操作系统 | Linux / macOS / Windows (WSL2) | 支持主流操作系统，推荐 Linux 服务器部署 |
| 浏览器 | 现代浏览器（Chrome 90+, Firefox 88+, Edge 90+） | 页面访问客户端，需支持 ES6 与 CSS Grid |
| 网络 | 出站连通性（HTTPS） | 用于验证收录链接的可用性状态，非必须但建议 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|----------|-----------|
| 用户手册 | `docs/user-guide.md` | 如何使用分类筛选、搜索功能以及自定义标签配置方法 |
| 管理员指南 | `docs/admin-guide.md` | 如何通过管理面板增删改资源链接，以及环境变量参数说明 |
| 部署参考 | `docs/deployment.md` | 如何将项目部署至生产环境，包括 Nginx 配置示例与 Dockerfile |
| 开发指引 | `docs/development.md` | 项目代码结构解析、本地调试流程及单元测试运行方法 |
| API 参考 | `docs/api-reference.md` | 管理面板后端接口的请求格式与返回数据结构说明 |
| 常见问题 | `docs/faq.md` | 汇总社区反馈的典型问题，涵盖部署报错与链接状态误判等 |

## 资源列表

本项目的核心资源清单基于用户提供的数据整理。所有链接均按原始格式收录，未做任何协议补全或域名改写。链接按业务主题分为以下子类别，便于快速定位。

### 综合数据分析站点

- <code>yijiabifen.cn</code>

### 实时数据与指数面板

- <code>xueyuanyuanjinrituijian.asia</code>
- <code>xueyuanyuanbifenzhibo.asia</code>
- <code>xueyuanyuanbifen.asia</code>

### 行业参考与辅助决策

- <code>rizhilianzhugongbang.asia</code>
- <code>rizhilianqianzhan.asia</code>
- <code>rizhilianjishibifen.asia</code>

## 项目结构

```
rizhilink-hub/
├── src/                              # 核心源代码目录
│   ├── assets/                       # 静态资源（图片、字体、样式表）
│   │   ├── styles/                   # 全局 CSS 与主题变量定义
│   │   └── images/                   # 图标与占位图资源
│   ├── components/                   # UI 组件库（Vue/React 风格，按功能划分）
│   │   ├── LinkCard/                 # 单个链接卡片组件（含状态标识）
│   │   ├── SearchBar/                # 搜索输入框与过滤条件面板
│   │   └── CategoryTabs/             # 分类标签切换组件
│   ├── data/                         # 资源数据管理模块
│   │   ├── links.json                # 主资源清单（含标题、URL、分类、标签）
│   │   └── schema.js                 # 资源数据校验规则与默认分组定义
│   ├── services/                     # 业务逻辑服务层
│   │   ├── linkValidator.js          # 链接连通性检测与状态缓存
│   │   └── searchEngine.js           # 本地全文索引与搜索排序算法
│   ├── admin/                        # 管理面板相关代码
│   │   ├── panel.html                # 管理界面 HTML 模板
│   │   └── manager.js                # 增删改操作及数据持久化逻辑
│   └── main.js                       # 应用入口文件，负责初始化与路由绑定
├── public/                           # 公共静态目录，构建后直接复制
│   └── favicon.ico                   # 站点图标
├── docs/                             # 项目文档（详见文档导航章节）
│   ├── user-guide.md
│   ├── admin-guide.md
│   ├── deployment.md
│   ├── development.md
│   ├── api-reference.md
│   └── faq.md
├── tests/                            # 单元测试与集成测试用例
│   ├── unit/                         # 组件与服务单元测试
│   └── e2e/                          # 端到端浏览器自动化测试
├── scripts/                          # 辅助脚本（数据迁移、批量检测等）
│   ├── validate-links.js             # 批量验证所有收录链接的可用性
│   └── export-csv.js                 # 将资源清单导出为 CSV 格式
├── .env.example                      # 环境变量模板文件（含 PORT、HOST、ADMIN_KEY）
├── package.json                      # npm 依赖与脚本命令定义
├── webpack.config.js                 # 构建工具配置（用于生产环境打包）
├── Dockerfile                        # 容器化构建描述文件
├── LICENSE                           # MIT 许可证文本
└── README.md                         # 项目介绍与快速入口（即本文档）
```

## 贡献指南

本项目欢迎社区提交改进建议与代码贡献。所有贡献者需遵守以下流程，以确保项目质量与一致性。

1. **提交 Issue 讨论**：在发起任何 Pull Request 之前，请先在 GitHub Issues 中描述您希望解决的问题或新增的功能，等待项目维护者的确认与指导意见，避免无效工作。

2. **Fork 并创建功能分支**：从主仓库 Fork 代码后，请基于 `main` 分支创建以 `feature/` 或 `fix/` 为前缀的新分支，例如 `feature/add-export-json`。请勿直接在 `main` 分支上修改。

3. **编写或更新测试用例**：对于新增功能或缺陷修复，需在 `tests/` 目录下补充相应的单元测试或集成测试，确保代码覆盖率不低于当前基线。

4. **遵循代码风格**：JavaScript 代码需符合 ESLint 配置（基于 Standard 风格），CSS 使用 BEM 命名规范。提交前请运行 `npm run lint` 与 `npm run test` 进行本地验证。

5. **提交 Pull Request**：将分支推送至您的 Fork 仓库，然后向主仓库的 `main` 分支发起 Pull Request。PR 描述需清晰说明变更内容、关联的 Issue 编号以及测试结果摘要。PR 合并前需至少一位维护者批准。

## 常见问题

**问：项目启动后提示“无法连接到验证服务”，但链接本身可以正常访问，是什么原因？**

答：链接可用性检测默认使用 HEAD 请求并设置 3 秒超时。部分站点可能拒绝 HEAD 请求或响应较慢，导致状态误判。您可以在 `src/services/linkValidator.js` 中调整超时时间（`timeout` 参数）或切换为 GET 请求。若您不需要该功能，可在配置文件中将 `ENABLE_VALIDATION` 环境变量设为 `false` 以关闭检测。

**问：如何批量更新资源链接？是否支持从外部文件导入？**

答：项目不内置数据库，所有资源存储在 `src/data/links.json` 文件中。您可以直接编辑该 JSON 文件进行批量增删改，格式参考已有条目。同时，项目提供了 `scripts/import-csv.js` 辅助脚本（需自行创建），可读取特定格式的 CSV 并覆盖或合并至现有清单。管理面板也支持逐条操作，适合小规模维护。

**问：部署到生产环境后，修改 links.json 是否需要重启服务？**

答：若您使用 `npm start` 以开发模式运行，修改 JSON 文件后页面会自动热重载。若使用 `npm run build` 生成的静态文件部署，则修改 `links.json` 后需重新执行构建命令并替换 `dist` 目录，或者将 `links.json` 作为外部资源通过 AJAX 加载（需调整代码）。推荐生产环境采用静态构建方式，并配合 CI/CD 流程自动重建。

## 许可证

MIT License

Copyright (c) 2026 RizhiLink Hub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:27
