# Web Scraping Lab

这是我的 Python 爬虫学习仓库，用来记录从基础网页解析到自动化抓取、数据清洗、存储和项目化实践的过程。

本仓库只放我自己的练习、笔记、复盘和小项目。书籍配套代码保留在本地参考目录，不直接复制到这里。

## Repository Structure

```text
web-scraping-lab/
  docs/               学习路线、模板、约定
  notes/              章节笔记和问题记录
  exercises/          按章节拆分的小练习
  projects/           可展示的小项目
  data/               本地数据说明和少量样例
  tests/              后续练习或项目的测试
```

## Current Focus

当前阶段先练基础能力：

1. 用 `urllib` 或 `requests` 获取页面。
2. 用 `BeautifulSoup` 解析 HTML。
3. 处理异常、编码、空结果和网络失败。
4. 把结果保存为 CSV 或 JSON。
5. 给每个练习写清楚目标、运行方式和结果说明。

现在先从 `exercises/chapter03/urlopen_error_handling.ipynb` 开始。

## Docs

- `docs/learning-plan.md`: 阶段路线。
- `docs/workflow.md`: 日常学习和提交方式。
- `docs/note-template.md`: 笔记模板。
- `docs/project-template.md`: 项目 README 模板。

## Learning Rule

每个练习至少包含：

- 目标：这次练什么。
- 输入：爬取或解析的数据来源。
- 输出：生成什么结果。
- 运行方式：从命令行如何启动。
- 复盘：遇到的问题和改进点。

## Reference Material

本地参考材料：

- `E:\Download\working\爬虫学习\python-scraping`
- `E:\Download\working\爬虫学习\LOCAL_LEARNING_GUIDE.md`
- `E:\Download\working\爬虫学习\DEPENDENCY_IMPORTS.md`

## Notes

这个仓库会逐步从学习记录变成作品集。早期重点不是项目数量，而是每个练习都能说明清楚、可以复现、能看出思考过程。
