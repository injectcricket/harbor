# WebLink Navigator

WebLink Navigator 是一个面向技术调研人员、SEO 分析师和站点运维工程师的轻量级外链资源聚合与导航系统。该项目定位于解决多源异构链接数据的统一收录、分类展示与快速检索问题，特别适用于需要频繁查阅特定垂直领域站点清单的日常工作流。目标用户包括但不限于运维工程师、爬虫策略开发者、内容聚合平台运营者以及数据分析岗位的从业人员。通过标准化的目录结构、静态化生成机制和低依赖的运行环境，本项目能够以最小化的维护成本支撑日均数百次的链接查阅需求，同时为后续扩展为动态链接监测系统预留了清晰的数据接口。

## 功能概览

- 静态化链接目录生成：基于配置文件自动生成分类索引页面，支持多层次目录结构，输出纯静态 HTML 文件，便于直接部署至任意 Web 服务器或对象存储服务。

- 多维度标签过滤系统：为每条外链资源分配领域标签、地区标签和更新频率标签，支持前端按标签组合进行筛选，提升大规模链接集合下的信息定位效率。

- 链接状态健康检查：集成定时任务模块，可对收录的链接进行 HTTP 头探测，自动标记响应超时或状态码异常的条目，辅助运维人员及时清理失效资源。

- 访问统计与热度排序：记录各链接的点击次数与最后访问时间，支持按热度降序排列，帮助团队识别高频使用的核心资源，优化导航布局。

- 批量导入与导出接口：提供基于 CSV 和 JSON Lines 格式的批量链接导入功能，同时支持将当前收录数据完整导出为标准表格文件，便于与其他数据分析系统对接。

- 自定义分类模板：允许用户根据业务需要新增、编辑或删除分类节点，每个分类可独立配置展示样式与排序规则，适配不同团队的命名习惯与工作流。

- 全文检索支持：集成轻量级倒排索引模块，支持对链接标题、描述和标签字段进行关键词检索，并高亮匹配结果，显著提升在数百条链接中的查找效率。

## 应用场景

- 行业竞品站点日常巡检：市场分析人员可通过本系统集中查阅同类竞品网站的更新动态，利用热度排序快速定位近期活跃度最高的站点，辅助制定竞争策略。

- 爬虫规则维护参考库：爬虫工程师可将需要采集的各类目标站点统一录入系统，按数据源类型分类管理，便于在规则调整时快速查找对应站点的结构特征。

- 外链建设效果追踪：SEO 运营团队使用本系统记录已投放的外链资源，配合健康检查功能定期验证链接有效性，及时发现被删除或改版的链接，避免权重流失。

- 技术文档引用源管理：技术写作团队可将常用规范文档、API 参考站点和标准库手册收录进系统，作为团队统一的引用来源库，确保文档中引用的外部资源长期可访问。

- 内部知识库导航节点：企业可将本系统部署为内部知识门户的子模块，集中展示部门常用第三方工具、数据平台和协作服务的入口，替代分散在邮件和聊天记录中的零散链接。

## 快速开始

以下指令适用于 Linux/macOS 环境以及 Windows WSL 子系统。请确保在执行前已安装 Git 和 Python 3.8 及以上版本。

```bash
# 克隆项目仓库至本地
git clone https://github.com/weblink-navigator/weblink-navigator.git
cd weblink-navigator

# 安装项目依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 执行初始化数据迁移并启动开发服务器
python manage.py migrate
python manage.py runserver
```

启动后，访问控制台输出的本地地址即可进入导航首页。默认管理员账号为 admin，初始密码在首次启动时自动生成并打印在终端日志中，请及时修改。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.8 ~ 3.11 | 核心运行环境，低于 3.8 版本将无法解析类型注解语法 |
| SQLite | 3.31 以上 | 默认数据库引擎，用于存储链接元数据和访问日志，生产环境可切换至 PostgreSQL |
| Git | 2.25 以上 | 用于克隆仓库和后续拉取更新，旧版本可能无法处理某些分支命名规范 |
| pip | 21.0 以上 | Python 包管理工具，用于安装 requirements.txt 中声明的依赖库 |
| curl | 7.68 以上 | 健康检查模块依赖的命令行工具，用于发送 HTTP 探测请求 |
| tzdata | 2022a 以上 | 时区数据包，确保访问时间戳记录时区正确，部分系统需单独安装 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | /docs/user-guide/ | 如何添加新链接、如何创建自定义分类、如何查看访问统计数据 |
| 运维手册 | /docs/ops-guide/ | 如何备份数据库、如何配置生产环境 Web 服务器、如何设置定时健康检查任务 |
| 开发者指南 | /docs/dev-guide/ | 项目整体架构设计、核心数据模型说明、如何扩展新的导入导出格式 |
| 设计决策 | /docs/design-decisions/ | 为什么选择静态化方案、标签系统的设计权衡、数据库索引策略的考量 |

## 资源列表

### 综合推荐类

<code>leisujinrituijian.asia</code>

<code>xueyuanyuansaiguo.asia</code>

### 比分与预测类

<code>xueyuanyuanzuqiubifenwang.asia</code>

<code>dszuqiuyuce.com.cn</code>

### 版权与赛事类

<code>zuqiudsbanquanchang.com.cn</code>

### 推荐与积分类

<code>zuqiudstuijian.com.cn</code>

<code>zuqiudsjifenbang.cn</code>

## 项目结构

```
weblink-navigator/
├── manage.py                  # 项目入口管理脚本，集成启动、迁移、测试等命令
├── requirements.txt           # 生产环境与开发环境通用依赖声明
├── config/                    # 全局配置目录
│   ├── settings.py            # Django 基础配置，含数据库、时区、语言等
│   ├── urls.py                # 根路由映射，注册所有应用的路由入口
│   └── wsgi.py                # 生产环境 WSGI 接入点
├── apps/                      # 所有自定义应用存放目录
│   ├── links/                 # 链接管理核心应用
│   │   ├── models.py          # 定义 Link, Category, Tag 等数据模型
│   │   ├── views.py           # 首页渲染、分类浏览、检索接口等视图函数
│   │   ├── admin.py           # 后台管理界面定制，支持批量导入导出操作
│   │   └── management/        # 自定义管理命令
│   │       └── commands/      # 包含 check_links 和 import_csv 等命令行工具
│   ├── stats/                 # 访问统计与健康检查应用
│   │   ├── models.py          # 定义 ClickLog, HealthRecord 模型
│   │   ├── tasks.py           # 定时任务函数，执行链接探测与日志轮转
│   │   └── middleware.py      # 记录点击事件的请求中间件
│   └── search/                # 全文检索应用，基于纯 Python 实现倒排索引
│       ├── indexer.py         # 索引构建与更新逻辑
│       └── querier.py         # 检索解析与结果排序
├── static/                    # 静态资源目录，含 CSS, JavaScript, 图片等
│   ├── css/                   # 基于 Flexbox 的响应式布局样式表
│   └── js/                    # 前端交互脚本，含标签过滤和检索框自动补全
├── templates/                 # Django 模板文件
│   ├── base.html              # 基础骨架模板，定义导航栏与页脚
│   └── links/                 # 链接相关页面模板
│       ├── index.html         # 首页展示分类卡片与热门链接
│       └── detail.html        # 单条链接详细信息页，含访问跳转入口
├── data/                      # 数据存储目录
│   ├── db.sqlite3             # SQLite 数据库文件（默认位置）
│   └── fixtures/              # 初始数据固件，用于快速演示与测试
├── logs/                      # 日志文件目录
│   ├── access.log             # 用户访问日志，记录点击与检索行为
│   └── health.log             # 健康检查任务运行日志，记录探测结果
└── scripts/                   # 辅助脚本目录
    ├── backup.sh              # 数据库与配置文件备份脚本
    └── deploy.sh              # 生产环境一键部署脚本，含依赖安装与服务重启
```

## 贡献指南

1. 在 GitHub 仓库页面点击 Fork 按钮，将项目复制至个人账户下，随后使用 git clone 将 fork 后的仓库拉取到本地开发环境，并配置 upstream 远程地址以便同步上游更新。

2. 在本地新建功能分支，分支命名采用 feature/ 或 fix/ 前缀加简要描述，例如 feature/add-export-json，随后在该分支上进行代码修改和新增功能开发。

3. 提交代码前运行完整的测试套件，确保所有现有测试用例通过，并为新增功能补充对应的单元测试或集成测试，测试文件放置于各应用下的 tests 目录中。

4. 编写清晰的提交信息，遵循常规提交规范，第一行为简短摘要，空一行后填写详细改动说明。同时更新 docs 目录下受影响的相关文档，确保文档与代码功能保持一致。

5. 将本地分支推送至个人远程仓库，随后通过 GitHub 界面发起 Pull Request 至主仓库的 develop 分支。请求中应描述改动目的、实现方式以及可能的兼容性影响，等待项目维护者审核反馈。

## 常见问题

问：启动服务后，首页无法显示任何链接条目，只看到空白分类。如何排查？

答：这种情况通常是因为尚未导入初始数据。请执行 python manage.py loaddata fixtures/initial_links.json 加载预置示例数据。若仍无内容，请检查数据库迁移是否完整执行，可使用 python manage.py makemigrations 和 python manage.py migrate 重新迁移。此外，确认 settings.py 中 DATABASES 配置的数据库路径具有写入权限。

问：健康检查任务频繁报告大量链接超时，但实际通过浏览器可以正常访问。如何调整探测参数？

答：健康检查模块默认超时时间为 3 秒，且不跟随重定向。部分站点响应较慢或启用了防爬机制。建议在 config/settings.py 中调整 HEALTH_CHECK_TIMEOUT 和 HEALTH_CHECK_FOLLOW_REDIRECT 变量。同时可修改 HEALTH_CHECK_INTERVAL 变量以降低检查频率，避免因请求过密被目标服务器临时屏蔽。

问：如何将系统迁移至生产环境，并改用 PostgreSQL 数据库？

答：首先在服务器上安装 PostgreSQL 客户端开发库，然后使用 pip install psycopg2-binary 安装驱动。在 settings.py 中将 DATABASES 的 ENGINE 改为 django.db.backends.postgresql，并提供 NAME、USER、PASSWORD、HOST 和 PORT 参数。运行 python manage.py migrate 创建表结构，最后使用 python manage.py dumpdata 导出旧数据后，再通过 python manage.py loaddata 导入至新数据库。建议在维护窗口期进行操作，并提前备份原有数据库文件。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-07-22 18:18:32
