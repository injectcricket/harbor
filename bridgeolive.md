# OSS-Nav

开源技术资源导航与信息聚合系统，是一个面向开发者、技术决策者与IT从业者的外链信息汇总平台。项目定位为高密度技术信息索引工具，解决用户在碎片化网络环境中检索有效技术文档、竞赛数据、官方公告时效率低下的问题。通过结构化分类与持续维护的URL资源库，提供可复用、可扩展的导航骨架，适用于个人书签管理、团队知识库建设及开源项目文档链整合。

## 功能概览

- **外链资源分类管理**：按赛事类型、地域归属、信息层级对URL进行标签化分组，支持静态分类与动态筛选。
- **信息完整性校验**：内置链接可达性检查机制，定期标记失效或重定向资源，保证资源列表可用性。
- **项目文档一体化**：将资源列表、安装依赖、目录结构、贡献流程集成于单一README，降低维护成本。
- **多层级导航表格**：通过文档导航表提供从快速入门到深度集成的路径指引，适配不同参与角色。
- **ASCII目录树可视化**：以文本图形展示项目文件组织，便于新人快速理解代码分区与职责边界。
- **批次化资源注入**：支持按批次（如第63/150批）批量导入URL，并自动归入资源列表章节，适合长期运营。
- **贡献者操作闭环**：提供从Fork、分支创建到PR提交的完整贡献流程，降低外部参与门槛。

## 应用场景

- **技术团队内部知识库构建**：团队可将本项目作为外链底座，添加内部文档链接、设计规范地址和运维监控面板，形成统一的导航入口，避免重复收藏。
- **开源项目文档站外链整合**：开源维护者可使用本项目集中管理外部参考链接（如协议解读、性能对比、社区论坛），避免在多个Markdown文件中散落维护。
- **个人开发者信息工作流优化**：开发者可将日常查阅的赛事积分榜、资格赛结果、排名公告等站点纳入本项目，通过版本控制同步多设备书签。
- **技术社区内容聚合展示**：社区运营者可利用项目结构中的分类表，将不同主题的外部资源（如安全公告、SDK下载、API参考）分组展示，提升信息可达性。
- **自动化监控与报警联动**：运维人员可扩展本项目的链接检查脚本，将失效链接输出为告警日志，对接Prometheus或邮件通知系统。

## 快速开始

执行以下命令完成项目克隆、依赖安装与本地运行。

```bash
# 克隆仓库到本地
git clone https://github.com/oss-nav/oss-nav.git
cd oss-nav

# 安装依赖（基于Python 3.9+ 虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 运行本地开发服务器（默认监听 127.0.0.1:8000）
python app.py
```

## 安装要求

系统运行所必需的依赖组件、版本约束及功能说明见下表。

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 - 3.11 | 核心运行环境，用于链接校验与静态生成 |
| Flask | 2.3.x | Web服务框架，提供本地预览与路由支持 |
| requests | 2.31.x | 发送HTTP HEAD请求，执行链接可达性检查 |
| markdown | 3.5.x | 将README.md渲染为HTML，用于文档预览模式 |
| PyYAML | 6.0.x | 解析配置文件 config.yaml，管理资源分组规则 |
| watchdog | 3.0.x | 文件系统监控，开发模式下自动重载服务 |
| pytest | 7.4.x | 单元测试框架，用于贡献前验证功能完整性 |

## 文档导航

下表按层面划分文档组织方式，指引不同需求的读者快速定位内容。

| 层面 | 目录/章节 | 回答的问题 |
|------|----------|-----------|
| 入门层 | 快速开始 + 安装要求 | 如何搭建运行环境、启动服务、验证部署成功 |
| 使用层 | 功能概览 + 应用场景 | 项目能做什么、适合什么情况使用、关键能力边界 |
| 数据层 | 资源列表 + 项目结构 | 外链分布在何处、目录如何组织、新增资源放在哪里 |
| 协作层 | 贡献指南 + 常见问题 | 如何提交改动、遇到报错如何处理、许可证约束 |

## 资源列表

本项目第63/150批次收录的外部链接按类别组织如下。所有链接均保留原始格式，未做协议补全或域名规范化处理。

赛事结果类

- <code>ouguanzigesaibisaijieguo.org.cn</code>
- <code>oulianzigesaibifen.org.cn</code>
- <code>ouguanzigesaibisaijieguo.org.cn</code>
- <code>ouguanzigesaijishibifen.org.cn</code>

积分与排名类

- <code>beimailiansaibeijifenbang.org.cn</code>
- <code>ouguanzigesaijifenbang.org.cn</code>

赛程信息类

- <code>beimailiansaibeisaicheng.org.cn</code>

## 项目结构

项目采用分层目录设计，兼顾资源隔离与扩展性。以下为完整ASCII目录树，每行附带职责注释。

```
oss-nav/
├── app.py                      # Flask应用入口，注册路由与启动服务
├── config.yaml                 # 全局配置文件，定义资源分组与检查间隔
├── requirements.txt            # Python依赖清单，用于pip批量安装
├── README.md                   # 项目主文档（即本文档）
├── .gitignore                  # Git忽略规则，排除虚拟环境和缓存
├── resources/                  # 外链资源存储目录
│   ├── batches/                # 按批次存放原始URL列表（如batch_63.json）
│   ├── validated/              # 链接检查通过后的缓存结果（JSON格式）
│   └── failed/                 # 失效链接记录，含失败时间与状态码
├── src/                        # 核心源码目录
│   ├── checker.py              # 链接可达性检查器，支持并发HEAD请求
│   ├── parser.py               # 解析资源列表，生成结构化分组字典
│   ├── renderer.py             # 将资源数据渲染为Markdown表格或HTML
│   └── watcher.py              # 文件变更监听，触发增量重新检查
├── templates/                  # Jinja2模板目录（用于Web预览模式）
│   ├── base.html               # 基础骨架模板
│   └── nav.html                # 资源导航页面模板
├── tests/                      # 单元测试目录
│   ├── test_checker.py         # 测试链接检查逻辑（含超时与重试）
│   ├── test_parser.py          # 测试解析器对异常URL的处理
│   └── test_integration.py     # 端到端流程测试（检查+渲染）
└── docs/                       # 扩展文档目录
    ├── api.md                  # 内部函数接口说明（供开发者参考）
    └── deployment.md           # 生产环境部署建议（Nginx + Gunicorn）
```

## 贡献指南

欢迎外部贡献者参与本项目，建议遵循以下步骤以保证协作质量。

1. **提交Issue讨论**：在发起Pull Request前，先于Issues区说明拟修改内容（如新增资源类别、调整检查策略），避免重复劳动。
2. **Fork并创建特性分支**：将仓库Fork至个人账户，基于`main`分支新建`feature/xxx`或`fix/xxx`格式的分支。
3. **遵循代码与文档规范**：Python代码需通过`pytest`全部用例，README新增或修改的资源列表需按原格式（裸域名或协议完整）追加。
4. **更新资源批次记录**：若新增外链，需在`resources/batches/`下对应批次文件中追加，并同步更新本文档的“资源列表”章节。
5. **发起Pull Request**：填写PR模板中的变更摘要、测试结果、影响范围，等待维护者审阅。

## 常见问题

**Q：运行app.py时提示“ModuleNotFoundError: No module named 'flask'”如何处理？**

A：请确认已激活虚拟环境（`source venv/bin/activate`）且执行过`pip install -r requirements.txt`。若仍失败，尝试手动安装`pip install flask==2.3.3`，并检查Python版本是否在3.9至3.11之间。

**Q：链接检查器返回大量“超时”或“连接拒绝”，是否代表所有链接均不可用？**

A：部分站点可能限制HEAD请求或存在防爬策略，检查器默认超时为5秒并重试1次。若确认目标站可正常访问，可调整`config.yaml`中的`timeout`和`retry`参数。检查结果以`resources/validated/`中的缓存为准，无效链接会移入`failed/`目录。

**Q：能否直接使用本项目作为生产环境公网服务？**

A：本项目设计定位为静态资源导航与开发辅助工具，未包含用户认证、请求限流、HTTPS强制跳转等生产安全特性。如需公网部署，建议前置Nginx反向代理并启用SSL证书，同时参考`docs/deployment.md`进行加固配置。

## 许可证

MIT License

Copyright (c) 2026 OSS-Nav Contributors

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

> 外链数量: 7 | 生成时间: 2026-07-22 18:17:45
