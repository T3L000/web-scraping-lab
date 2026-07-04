## Chapter 1. How the Internet Works

### 关键概念

- 网络通信不是一条固定线路直连，而是把消息拆成 packet，通过网络动态传输。
- OSI 模型提供了理解网络分层的方式。写爬虫时主要在 application layer 工作，但后面遇到 TLS、代理、反爬、连接失败时，底层概念也会有帮助。
- IP 负责定位网络上的机器，TCP/UDP 和端口负责把数据交给机器上的具体服务。
- HTTP 是应用层协议，HTML/JSON 更像数据格式或表示方式。
- HTML 描述网页结构，CSS 描述显示样式，JavaScript 负责动态行为。

### 对爬虫的意义

- 爬虫抓到的通常是 HTML、JSON、图片、PDF 等资源，而不是浏览器渲染后的视觉页面。
- 页面上“看得见”的内容不一定直接存在于初始 HTML 中，可能是 JavaScript 后续请求来的。
- Network 面板能看到浏览器实际请求了哪些 URL、方法、状态码、headers、cookies 和 response。
- Elements 面板能帮助定位某段文字对应的 HTML 标签和属性。

### 我的理解

先把浏览器当作一个“帮我发请求、收响应、渲染页面”的工具。现在我先替代最基础的一步：发请求并读取响应。至于 JavaScript、Cookie、代理这些，先知道它们存在，不急着一次吃完。

## Chapter 2. The Legalities and Ethics of Web Scraping

### 本章定位

这一章讲法律和伦理边界。重点不是背法律条文，而是知道爬虫可能涉及版权、服务条款、服务器负载、访问授权和 robots.txt。

### 关键概念

- 公开可访问不等于可以无限制抓取、复制、再发布。
- 版权关注内容本身的复制和使用。
- Trespass to chattels 关注对别人服务器资源造成干扰或损害。
- CFAA 关注未授权访问或越权访问。
- robots.txt 是网站声明爬虫访问规则的一种约定。
- Terms of Service 是服务条款，可能限制自动访问、抓取或再分发。
- 爬虫需要节制请求频率，避免给目标网站造成压力。

### 实操原则

- 不抓取登录后才可见、明显受限制或不属于自己授权范围的数据。
- 不提交或泄露 Cookie、token、账号密码。
- 请求频率要克制，必要时加 sleep、重试间隔和缓存。
- 做学习练习时，优先使用书中示例站、公开测试站或自己搭的页面。
- 如果网站提供 API，优先考虑 API。

### 我的理解

爬虫不是“能抓就抓”。技术上能访问，只说明请求可能成功；是否应该抓、能不能存、能不能展示，是另一层问题。我后面做作品集项目时，README 里要写清楚数据来源、抓取范围和频率控制。

## Chapter 3. Applications of Web Scraping

### 本章定位

这一章讲爬虫能用于什么场景，以及如何给项目分类。它提醒我：爬虫项目不是从“我要写代码”开始，而是从“我要什么数据、用来解决什么问题”开始。

### 应用场景

- E-commerce：价格、库存、商品信息、评论等。
- Marketing：品牌提及、竞品观察、内容趋势。
- Academic Research：收集公开网页数据做研究。
- Product Building：把抓取数据作为产品的一部分。
- Travel：航班、酒店、目的地、价格变化等。
- Sales：客户线索、目录信息、公开联系方式。
- SERP Scraping：搜索结果页数据，但通常更敏感，也更容易遇到限制。

### 项目分类思路

开始写代码前先问：

- 数据源在哪里？
- 数据是否公开？
- 数据量多大？
- 页面结构是否稳定？
- 是一次性抓取，还是长期定期抓取？
- 输出给谁用？人看、程序读、还是存数据库？
- 是否需要登录、翻页、搜索、表单、JavaScript 或代理？

### 我的理解

一个好的爬虫项目，核心不是“我抓了多少页面”，而是“我稳定、清楚、合法地拿到了有用数据”。现在练习阶段先把每个小脚本写明白，比堆功能更重要。

## Chapter 4. Writing Your First Web Scraper

### 本章定位

这一章开始写第一个爬虫：用 Python 请求网页，用 BeautifulSoup 解析 HTML，并开始处理异常。


### urlopen

最小请求：

```python
from urllib.request import urlopen

html = urlopen("http://www.pythonscraping.com/pages/page1.html")
```

含义：

- `urlopen` 向指定 URL 发请求。
- 返回的是类似文件对象的 response。
- 可以用 `.read()` 读取返回内容。

### BeautifulSoup

基础解析：

```python
from urllib.request import urlopen
from bs4 import BeautifulSoup

html = urlopen("http://www.pythonscraping.com/pages/page1.html")
bs = BeautifulSoup(html.read(), "html.parser")
print(bs.h1)
```

要点：

- `BeautifulSoup` 把 HTML 文本解析成可以查询的对象。
- `bs.h1` 会找页面中第一个 `<h1>`。
- `bs.html.body.h1`、`bs.body.h1`、`bs.html.h1` 在简单页面里可能一样，但路径范围不同。

### 异常处理

`urlopen` 可能失败：

- `HTTPError`：服务器能连上，但页面返回 HTTP 错误，比如 404、500。
- `URLError`：服务器或域名无法连接。

常见结构：

```python
from urllib.request import urlopen
from urllib.error import HTTPError
from urllib.error import URLError

try:
    html = urlopen("http://www.pythonscraping.com/pages/page1.html")
except HTTPError as e:
    print(e)
except URLError:
    print("The server could not be found!")
else:
    print("It Worked!")
```

解析结果也可能为空：

```python
if bs.h1 is not None:
    print(bs.h1)
else:
    print("h1 tag was not found")
```

### 我的理解

第一个爬虫不要急着写复杂逻辑。先把四件事跑通：导入、请求、解析、异常处理。

## Chapter 5. Advanced HTML Parsing

### 本章定位

第 5 章开始真正进入“从复杂 HTML 中精确拿数据”。第 4 章是拿第一个标签，第 5 章是学会过滤、遍历、定位和提取。

这一章的核心思路是：复杂网页看起来乱，但 HTML 里通常有标签、属性、层级、兄弟节点、父节点等结构。我的目标不是把 BeautifulSoup 所有 API 背下来，而是先学会几种稳定定位方式，把不需要的内容一点点排除掉。

### 1. CSS class 和 id 对爬虫很有用

网页用 CSS 控制样式，开发者通常会给 HTML 元素加 `class` 或 `id`。这些属性原本是给样式和脚本用的，但对爬虫也很有帮助。

例如页面中可能有：

```html
<span class="green">the prince</span>
<span class="red">Heavens!</span>
```

爬虫可以根据 `class` 区分不同含义的文本。

### 2. `find_all()`

第 4 章常用：

```python
bs.h1
```

这只返回第一个匹配标签。

第 5 章重点开始用：

```python
bs.find_all("span", class_="green")
```

含义：

- 找所有 `<span>` 标签。
- 只要 `class` 是 `green` 的。
- 返回结果是列表，可以循环处理。

示例：

```python
name_list = bs.find_all("span", class_="green")

for name in name_list:
    print(name.get_text())
```

书里也会看到这种写法：

```python
bs.find_all("span", {"class": "green"})
```

这也能用。现在我更倾向写 `class_="green"`，因为它一眼能看出是在筛选 class。

### 3. `get_text()` 不要太早用

`get_text()` 会把标签结构去掉，只留下文本。

适合：

- 最后打印。
- 最后存 CSV、JSON、数据库。
- 最后做纯文本处理。

不适合：

- 还没定位完目标标签时就调用。

原因：HTML 标签和属性是定位数据的重要线索。太早变成纯文本，后面就很难继续筛选。

我的规则：

```text
先用标签结构定位，最后一步再 get_text()
```

### 4. `find()` 和 `find_all()` 的区别

```python
bs.find("span")
```

返回第一个匹配项。

```python
bs.find_all("span")
```

返回所有匹配项列表。

学习阶段可以这样选：

- 只需要一个结果：`find()`
- 需要循环处理多个结果：`find_all()`

### 5. 通过属性查找

常见写法：

```python
bs.find_all("span", {"class": "green"})
```

或者：

```python
bs.find_all(attrs={"class": "green"})
```

如果查找 `class`，BeautifulSoup 也支持：

```python
bs.find_all("span", class_="green")
```

注意这里是 `class_`，因为 `class` 是 Python 关键字。

### 6. BeautifulSoup 中常见对象

这一章提到几类对象：

- `BeautifulSoup`：整个解析后的文档对象。
- `Tag`：一个 HTML 标签，例如 `<span class="green">...</span>`。
- `NavigableString`：标签里的文本。
- `Comment`：HTML 注释。

对新手来说，先记住：

```text
bs 是整棵树
tag 是树上的一个节点
tag.get_text() 拿节点里的文本
tag.attrs 看节点属性
```

### 7. HTML 树结构

HTML 是树状结构：

- parent：父节点。
- children：直接子节点。
- descendants：所有后代节点。
- siblings：同级兄弟节点。

这个概念非常重要，因为复杂页面往往不能只靠一个标签名定位，要利用上下文关系。

### 8. children 和 descendants

`children` 只看直接下一层：

```python
for child in bs.find("table").children:
    print(child)
```

`descendants` 会看所有更深层后代。

区别：

```text
children = 亲儿子
descendants = 子孙后代
```

BeautifulSoup 的很多查找默认会搜索后代，不只是一层。

### 9. siblings

兄弟节点是同一个父节点下面的同级元素。

常见写法：

```python
for sibling in tag.next_siblings:
    print(sibling)
```

适用场景：

- 表格行。
- 列表项。
- 标题后面跟着正文。

注意：`next_siblings` 只找后面的兄弟，不包含自己，也不找前面的兄弟。

### 10. parents

父节点查找用于“先定位到一个小元素，再反向找到它所在的大结构”。

常见写法：

```python
tag.parent
```

适用场景：

- 先找到图片，再找图片所在的商品卡片。
- 先找到价格文本，再回到父级容器找商品名。

### 11. 正则表达式

正则表达式用于匹配文本模式。第 5 章不是让我们成为正则专家，而是让我们能把正则和 BeautifulSoup 结合起来。

常见用途：

- 匹配一类 URL。
- 匹配图片文件名。
- 匹配邮箱、数字、特定格式字符串。
- 匹配具有某种规律的属性值。

基本提醒：

- 正则很强，但可读性容易变差。
- 能用清楚的标签和属性定位时，不要过度依赖正则。
- 写正则时要先在小样本上试。

#### 正则速查

我现在不用一次背完，先够用就行。遇到图片路径、链接、邮箱、价格这类“长得有规律”的字符串，再拿正则出来。

常用符号：

| 写法 | 含义 | 例子 |
| --- | --- | --- |
| `.` | 任意一个字符 | `a.c` 可匹配 `abc`、`a1c` |
| `*` | 前一个内容出现 0 次或多次 | `ab*` 可匹配 `a`、`ab`、`abb` |
| `+` | 前一个内容出现 1 次或多次 | `ab+` 可匹配 `ab`、`abb`，不匹配 `a` |
| `?` | 前一个内容出现 0 次或 1 次 | `https?` 可匹配 `http`、`https` |
| `\d` | 一个数字 | `\d` 匹配 `0` 到 `9` |
| `\d+` | 一个或多个数字 | 可匹配 `1`、`23`、`2024` |
| `\w` | 字母、数字、下划线 | 可匹配 `a`、`A`、`3`、`_` |
| `\s` | 空白字符 | 空格、换行、制表符等 |
| `[]` | 字符集合，匹配其中一个 | `[abc]` 匹配 `a` 或 `b` 或 `c` |
| `[^]` | 反向集合，不匹配其中字符 | `[^0-9]` 匹配非数字 |
| `{m,n}` | 出现次数范围 | `\d{2,4}` 匹配 2 到 4 个数字 |
| `^` | 字符串开头 | `^http` 匹配以 `http` 开头 |
| `$` | 字符串结尾 | `jpg$` 匹配以 `jpg` 结尾 |
| `()` | 分组 | `(jpg\|png)` 匹配 `jpg` 或 `png` |
| `\|` | 或 | `cat\|dog` 匹配 `cat` 或 `dog` |

容易忘的点：

- 正则里的 `.` 不是普通点，而是“任意字符”。
- 要匹配真正的点号，写 `\.`。
- Python 字符串里建议用 raw string，也就是前面加 `r`。

例如：

```python
pattern = r"\.\./img/gifts/img\d+\.jpg"
```

这段可以拆开看：

```text
\.\.       匹配 ..
/img/gifts/img
           匹配固定路径
\d+        匹配一个或多个数字
\.jpg      匹配 .jpg
```

所以它匹配：

```text
../img/gifts/img1.jpg
../img/gifts/img2.jpg
../img/gifts/img10.jpg
```

不匹配：

```text
../img/gifts/logo.jpg
../img/gifts/img1.png
```

#### Python `re` 常用方法

我现在最常用这几个：

```python
import re
```

`re.compile()`：先把规则编译出来，后面重复使用。

```python
image_pattern = re.compile(r"\.\./img/gifts/img\d+\.jpg")
```

`re.search()`：只要字符串里有一段匹配就算成功。

```python
result = re.search(r"img\d+\.jpg", "../img/gifts/img3.jpg")
print(result.group())
```

`re.match()`：必须从字符串开头就匹配。

```python
re.match(r"http", "https://example.com")
```

`re.findall()`：找出所有匹配片段，返回列表。

```python
prices = re.findall(r"\$\d+\.\d{2}", "A: $15.00, B: $9.99")
print(prices)
```

`re.sub()`：替换匹配内容。

```python
clean = re.sub(r"\s+", " ", "hello    world")
print(clean)
```

我的记法：

```text
search  = 找有没有
match   = 从头开始找
findall = 全部拿出来
sub     = 找到并替换
compile = 先把规则准备好
```

#### 爬虫里常见正则

图片：

```python
r"\.\./img/gifts/img\d+\.jpg"
```

链接：

```python
r"^https?://"
```

价格：

```python
r"\$\d+\.\d{2}"
```

邮箱，简单够用版：

```python
r"[\w.-]+@[\w.-]+\.\w+"
```

只匹配 `.jpg` 或 `.png`：

```python
r"\.(jpg|png)$"
```

提醒：邮箱、URL 这类东西真要做得非常严谨会很复杂。学习阶段先够用，后面真实项目再按数据情况加强。

### 12. 正则和 BeautifulSoup 结合

BeautifulSoup 的查找参数可以放正则对象：

```python
import re

images = bs.find_all("img", {"src": re.compile(r"\.\./img/gifts/img\d+\.jpg")})
```

含义：

- 找所有 `<img>`。
- 只要 `src` 符合指定模式。

适合处理“属性值有规律，但不是完全固定”的情况。

也可以先把正则单独命名，这样更好读：

```python
image_pattern = re.compile(r"\.\./img/gifts/img\d+\.jpg")
images = bs.find_all("img", {"src": image_pattern})

for image in images:
    print(image.get("src"))
```

我的习惯：如果正则超过一眼能看懂的长度，就先命名成 `xxx_pattern`。

如果是按文本内容查找，新版 BeautifulSoup 推荐用 `string=`，不要继续照旧书或旧教程写 `text=`：

```python
matches = bs.find_all(string="the prince")
```

模糊一点可以写：

```python
matches = bs.find_all(string=lambda s: s and "prince" in s)
```

我这里要记住：`text=` 不是马上不能用，但会有弃用警告；以后自己的代码统一写 `string=`。

如果要用正则匹配文本，也可以这样：

```python
matches = bs.find_all(string=re.compile(r"prince"))
```

如果想忽略大小写：

```python
matches = bs.find_all(string=re.compile(r"prince", re.IGNORECASE))
```

#### 什么时候该用正则

适合用：

- 属性值有明显规律，比如 `img1.jpg`、`img2.jpg`。
- 文本里要提取价格、日期、邮箱、编号。
- 标签的 `href`、`src`、`id` 有统一前缀或后缀。

先别用：

- 明明可以用 `id`、`class_` 精确找到。
- 只是为了少写两行 BeautifulSoup。
- 正则写完自己都看不懂。

我的顺序：

```text
id/class 能解决 -> 不用正则
属性有规律 -> 用正则
文本格式有规律 -> 用正则
正则超过一行 -> 先拆开或写注释
```

### 13. 访问属性

拿一个标签的所有属性：

```python
tag.attrs
```

拿某个具体属性：

```python
tag.attrs["src"]
```

更稳一点可以用：

```python
tag.get("src")
```

区别：

- `tag.attrs["src"]`：没有 `src` 时会报错。
- `tag.get("src")`：没有 `src` 时返回 `None`。

我现在更倾向先写：

```python
src = tag.get("src")
```

因为爬网页时经常会遇到“这个标签不是我以为的那个标签”。用 `get()` 至少不会因为缺一个属性直接炸掉。

常见例子：

```python
image = bs.find("img")
print(image.get("src"))
```

如果我想看这个标签到底有哪些属性：

```python
print(image.attrs)
```

记法：

```text
get_text() 取标签里面夹着的文字
get("src") 取标签开口里写着的属性
```

### 14. lambda 表达式

lambda 是匿名函数，可以作为筛选条件传给 BeautifulSoup。

示例：

```python
bs.find_all(lambda tag: len(tag.attrs) == 2)
```

含义：

- 对每个标签执行这个函数。
- 如果返回 `True`，就保留这个标签。
- 这里筛选的是“属性数量等于 2 的标签”。

lambda 适合写简单条件。条件一复杂，就应该改成普通函数，避免难读。

按文本内容筛选时也能用 lambda：

```python
bs.find_all(string=lambda s: s and "prince" in s)
```

这里的 `s and` 是保护：如果 `s` 是 `None`，后面的 `"prince" in s` 就不会继续执行。

我的规则：

```text
一眼能看懂 -> 可以 lambda
需要解释半天 -> 写 def
```

### 15. 我问过但很容易忘的点

#### `class_` 不是 `_class`

正确：

```python
bs.find_all("span", class_="green")
```

也可以：

```python
bs.find_all("span", attrs={"class": "green"})
```

不要写：

```python
bs.find_all("span", _class="green")
```

我本地 `beautifulsoup4 4.15.0` 实测 `_class` 找不到结果，并且会警告这很像把 `class_` 拼错了。

#### `text=` 换成 `string=`

旧写法：

```python
bs.find_all(text="the prince")
```

新版推荐：

```python
bs.find_all(string="the prince")
```

如果看到书里或网上旧代码有 `text=`，我自己写的时候改成 `string=`。

#### Chrome 里的 `tbody` 不一定等于 BeautifulSoup 里的树

Chrome Elements 面板可能会自动补全 DOM，例如：

```html
<table>
  <tbody>
    <tr>...</tr>
  </tbody>
</table>
```

但 BeautifulSoup 从原始 HTML 解析时，可能看到的是：

```html
<table>
  <tr>...</tr>
</table>
```

所以在书里这个 `giftList` 表格中：

```python
for child in bs.find("table", {"id": "giftList"}).children:
    print(child)
```

打印出来主要是 `tr`，不是 `tbody`。以后不确定时，以 BeautifulSoup 实际解析结果为准：

```python
table = bs.find("table", {"id": "giftList"})
print(table.prettify())
```

或者只看直接子标签名字：

```python
from bs4 import Tag

for child in table.children:
    if isinstance(child, Tag):
        print(child.name)
```

#### `children`、`siblings`、`parent` 的方向

```text
children = 往下一层找
siblings = 在同一层横着找
parent   = 往上一层找
```

`next_siblings` 不包含自己，只拿后面的同级节点。例如从表头行开始：

```python
for row in bs.find("table", {"id": "giftList"}).tr.next_siblings:
    print(row)
```

这会跳过表头，打印后面的商品行。

`parent` 常用于“我先找到了一个很具体的小标签，但真正要的数据在它旁边或外层”：

```python
price = (
    bs.find("img", {"src": "../img/gifts/img1.jpg"})
    .parent
    .previous_sibling
    .get_text()
)
```

这段路线是：

```text
img -> 父级 td -> 前一个兄弟 td -> 取价格文字
```

#### 不要迷信长链式查找

这类代码很脆：

```python
bs.find_all("table")[4].find_all("tr")[2].find_all("td")[3]
```

网页多一个表格、多一行、多一列，就可能拿错。

我自己的顺序：

```text
先找 id / class
再找稳定属性
再看父子兄弟关系
最后才考虑第几个、第几层
```

### 16. 本章易错点

- 把 `find_all()` 返回的列表当成单个标签用。
- 太早调用 `get_text()`，导致后面无法继续用标签和属性定位。
- 忘记 `class` 在 Python 里要写成 `class_` 或放进 `attrs` 字典。
- 看到书里或旧教程的 `text=` 直接照抄，新版 BeautifulSoup 会提示改用 `string=`。
- 用 `next_siblings` 时以为会包含当前节点。
- 访问属性时直接写 `tag.attrs["src"]`，但有些标签没有 `src`。
- 正则写得太宽，匹配到不该要的内容。
- 正则写得太窄，网页稍微变化就匹配不到。
- 忘记把普通点号写成 `\.`，导致 `.` 变成“任意字符”。
- 忘记用 raw string，反斜杠越写越乱。
- 只看 Chrome Elements 面板，没有核对 BeautifulSoup 实际解析出来的结构。
- 链式查找写太长，代码当时能跑，过几天自己都看不懂。

### 17. 本章练习建议

先不要急着写大爬虫，可以做这些小练习：

1. 打开 `warandpeace.html` 示例页面。
2. 用 `find_all("span", class_="green")` 提取人名。
3. 用 `find_all("span", class_="red")` 提取台词。
4. 对每个结果分别尝试打印 tag 本身和 `get_text()` 后的文本，观察区别。
5. 找一个 `<img>` 标签，查看 `attrs` 和某个具体属性。
6. 用 `find_all(string="the prince")` 找纯文本，再改成 lambda 模糊匹配。
7. 试一个正则筛选图片链接。
8. 用 `children`、`next_siblings`、`parent` 分别观察 HTML 树关系。
9. 用 `prettify()` 对比 Chrome Elements 面板和 BeautifulSoup 的解析结果。

### 18. 我的阶段总结

第 5 章开始，爬虫从“能拿页面”进入“能精确拿数据”。现在最值得练熟的是：

```text
find()
find_all()
attrs
get_text()
children
siblings
parent
re.compile()
```

我的优先级：

1. 先用标签名、`class`、`id` 定位。
2. 定位不够时，再利用父子、兄弟关系。
3. 属性值有规律时，再加入正则。
4. 最后一步再转成纯文本或保存。

现阶段我的写法尽量保持简单：能用清楚的 `find_all("span", class_="green")` 就不要上来写复杂正则。正则是工具，不是炫技点。

## Chapter 6. Writing Web Crawlers

待读完后补充。

## Chapter 7. Web Crawling Models

待读完后补充。

## Chapter 8. Scrapy

待读完后补充。

## Chapter 9. Storing Data

待读完后补充。

## Chapter 10. Reading Documents

待读完后补充。

## Chapter 11. Working with Dirty Data

待读完后补充。

## Chapter 12. Reading and Writing Natural Languages

待读完后补充。

## Chapter 13. Crawling Through Forms and Logins

待读完后补充。

## Chapter 14. Scraping JavaScript

待读完后补充。

## Chapter 15. Crawling Through APIs

待读完后补充。

## Chapter 16. Image Processing and Text Recognition

待读完后补充。

## Chapter 17. Avoiding Scraping Traps

待读完后补充。

## Chapter 18. Testing Your Website with Scrapers

待读完后补充。

## Chapter 19. Web Scraping in Parallel

待读完后补充。

## Chapter 20. Web Scraping Proxies

待读完后补充。
