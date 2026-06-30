# Learning Plan

## Stage 1: Basic Fetching And Parsing

Goal: 能够获取网页、解析 HTML，并提取稳定字段。

Practice:

- 读取一个页面并打印标题。
- 提取所有链接。
- 提取表格或列表数据。
- 保存为 CSV 或 JSON。

Deliverable:

- `exercises/chapter04/`
- `exercises/chapter05/`
- 对应章节笔记放在 `notes/`

## Stage 2: Crawling Flow

Goal: 能够从一个入口页面继续发现和访问下一批页面。

Practice:

- 控制抓取范围。
- 记录已访问 URL。
- 处理重复链接。
- 给请求设置合理间隔。

Deliverable:

- `exercises/chapter06/`
- `exercises/chapter07/`

## Stage 3: Structured Projects

Goal: 把零散脚本整理成小项目。

Practice:

- 明确输入、输出和运行命令。
- 拆分抓取、解析、保存逻辑。
- 增加基础测试。
- 增加 README。

Deliverable:

- `projects/quotes_spider/`
- `projects/static_site_catalog/`

## Stage 4: Tools And Real-World Topics

Goal: 接触 Scrapy、Selenium、数据库、图片识别和代理等更接近真实场景的内容。

Practice:

- Scrapy 项目结构。
- Selenium 浏览器自动化。
- MySQL 或 SQLite 存储。
- 图片与验证码处理的边界。
- API 和代理服务的认证方式。

Deliverable:

- `projects/scrapy_wiki_notes/`
- `projects/browser_automation_demo/`

## Review Checklist

- 代码能否从 README 里的命令启动。
- 数据输出是否有样例说明。
- 异常处理是否清楚。
- 是否避免提交密钥、Cookie 和大量原始数据。
- 是否写了这次练习的复盘。
