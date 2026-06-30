# Workflow

## Daily Study Flow

1. 读一小节书或示例代码。
2. 在 `notes/` 写下目标和关键点。
3. 在 `exercises/` 用自己的方式重写最小示例。
4. 运行代码并记录输出。
5. 在笔记里写复盘。
6. 提交一次 Git commit。

## Commit Style

推荐格式：

```text
docs: add chapter04 notes
exercises: add first scraper demo
projects: initialize quotes spider
tests: add parser test
```

## What To Commit

可以提交：

- 自己写的代码。
- 自己写的笔记。
- 小型样例数据。
- 项目 README。
- 测试文件。

不要提交：

- 密钥、Cookie、账号密码。
- 大量原始抓取数据。
- 浏览器驱动缓存。
- 虚拟环境目录。
- 直接复制的大段书籍代码。

## When A Practice Becomes A Portfolio Project

满足这些条件后，可以把练习移动或重构到 `projects/`：

- 有明确目标。
- 有可运行入口。
- 有输出样例或字段说明。
- 有异常处理。
- README 能让别人复现。
- 代码不是单纯照抄示例。
