## ARTS - Review
## [用python和beautifulSoup来抓取网站](https://medium.freecodecamp.org/how-to-scrape-websites-with-python-and-beautifulsoup-5946935d93fe)

因特网上的信息多到任何一个人穷尽一生都无法全部吸收的地步。你需要的不是操作这些信息，而是一个额外的方法来存储，组织、分析它。

你需要网页爬虫。

网页爬虫自动提取数据并且用一个你能轻松理解的格式展现。在本教程中，我们将聚焦它在金融市场的应用，但是网页爬虫可以用在很多场景下。

如果你是一个狂热的投资者，每天都接触很多价格会非常痛苦，尤其当你需要的信息需要通过好几个网页来查找。我们通过建立一个网页爬虫来自动从网上获取股票指数来让数据抽取变得简单。

### 起步
我们准备用Python作为我们的爬虫语言，附带使用一些简单强大的库， BeautifulSoup.

+ 对于Mac 用户，Python已经在OS X 系统预装过了。打开终端 输入 ``` python --version```.你会看到你的python版本是 2.7.x
+ 对于Windows用户，请安装python通过 [Python](https://www.python.org/downloads/)

下一步就是用 pip 获取 BeautifulSoup 库，pip是一个Python的包管理工具.

在终端输入：

```

easy_install pip  
pip install BeautifulSoup4

```

**注意** 如果在命令行执行失败，尝试加 ```sudo```在每行前面


### 基础
在我们调到代码之前，先理解下HTML的基础和一些爬虫规则。

#### HTML 标签
如果你早已理解HTML标签，感到无压力请跳过这段。

```


<!DOCTYPE html>  
<html>  
    <head>
    </head>
    <body>
        <h1> First Scraping </h1>
        <p> Hello World </p>
    <body>
</html>

```
这是基础的HTML网页句式。每个```<tag>```提供一个网页里的块。

1. ```<!DOCTYPE html>```: HTML文档必须以类型声明开始。
2. HTML文档包含在```<html>```和```</html>```之间。
3. HTML的 meta 和 script 声明在```<head>```和```</head>```之间
4. HTML文档显示的内容在```<body>```和 ```</body>```之间。
5. 标题用 ```<h1>```到```<h6>```标签定义。
6. 段落用```<p>```标签定义。


其他有用的标签包括 ```<a>``` 用来做超链接， ```<table>``` 列表，```<tr>```列表行 ```<td>```列表列。

HTML标签通常伴随id class 属性。id属性表示HTML标签的一个唯一id，它的值必须在HTML文档中唯一。
class 属性通常定义相同风格的HTML标签。我们可以利用这些id 和class 帮助我们定位我们想要的数据。

对于更多HTML 标签的信息，请关注 W3CSchools .

#### 爬虫规则

1. 你应该在爬虫前检查目标网站的术语和条件信息。注意阅读数据使用的法律声明。通常你抓取的数据不应该用来做商业应用。
2. 不要太有侵略性的请求网站数据(也被称为垃圾邮件)， 这可能导致网站崩溃。确定你的程序行为合理(像一个人一样)。每秒请求一个网页是一个好的方式。
3. 网站布局可能不时变化，所以确定再次访问网站，如有必要重写你的代码。


### 检索页面

我们从[Bloomberg Quote](http://www.bloomberg.com/quote/SPX:IND)网站获取一个页面作为例子。
就像一些人跟随的股票市场，我们想从这个页面获得索引名称为(S&P 500) 和他的价格。首先，右键点击打开你的浏览器检查网页。

用你的鼠标覆盖到价格上，你将会看到一个蓝块围绕它。如果你点击它，关联的HTML将会在浏览器控制台被选中。

我们能从结果中看到价格包含在几个HTML标签里，```<div class="basic-quote">``` → ```<div class="price-container up">``` → ```<div class="price">.```

同样的，你可以覆盖点击名字“S&P 500Index”， 他被包含在```<div class="basic-quote">``` 和``` <h1 class="name">.```

现在我们在class标签的帮助下知道了我们数据的唯一位置。

### 跳进代码里
现在我们知道我们需要的数据在哪，我们可以开始编码我们的网页爬虫。打开你的文本编辑器。

首先，导入我们将要使用的所有的库。

```

# import libraries
import urllib2
from bs4 import BeautifulSoup

```

下一步，声明网页地址：

```

# specify the url
quote_page = 'https://www.bloomberg.com/quote/SPX:IND'
```
然后使用 python urllib2 获得声明的url的HTML页面。

```
# query website
page = urllib2.urlopen(quote_page)

```

最后，解析页面到BeautifulSoup 格式，所一我们可以用BeautifulSoup.

```
# parse html 
soup = BeautifulSoup(page, 'html.parser')

```
现在我们有一个 soup变量，包含页面的HTML。这里就是我们开始编程提取数据的起点。

记着我们唯一那层数据吗？BeautifulSoup可以帮助我们进入这一层用 find() 抽出。在这个样例，HTML 类 ```name```是这个页面唯一的，我们可以简单的查找```<div class="name">```

```

# take out div of name and get its value
name_box = soup.find('h1', attrs={'class': 'name'})
```
我们获得标签后，可以通过text 来获内容

```
name = name_box.text.strip() # strip() is used to remove starting and trailing
print name
```
同样我们可以获取价格：

```
# get the index price
price_box = soup.find(‘div’, attrs={‘class’:’price’})
price = price_box.text
print price
```

你运行程序，会获得当前页面的价格和 S&P 500 索引。

### 导出到Excel CSV
现在我们有了数据，是时候去保存他们了。表格的逗号分隔格式(CSV)是一个好选择。能够在Excel打开所以你能够非常容易查看和操作它。

但是首先，我们不得不导入Python csv模块和datetime模块来获得记录时间。在导入模块插入这些。

```
import csv
from datetime import datetime
```
你的代码底部，添加代码把数据写入csv文件

```

# open a csv file with append, so old data will not be erased
with open(‘index.csv’, ‘a’) as csv_file:
 writer = csv.writer(csv_file)
 writer.writerow([name, price, datetime.now()])
```

如果你运行你的程序，你会输出一个 index.csv 的文件，你可以用Excel打开，看到一行数据。

所以如果你每天运行这程序，你将会轻松获得 S&P 500 Index 的价格，不必再通过网站。

### 深入（高级用法）
#### 多索引
抓取一个索引对你来说是不够的，对吧？我们可以尝试同事抓取多个索引。

首先修改 quote_page 为一个url 数组。

```
quote_page = [‘http://www.bloomberg.com/quote/SPX:IND', ‘http://www.bloomberg.com/quote/CCMP:IND']
```

我们改变数据抽取代码为一个 for 循环， 依次处理URL储存到 data 元组中。


```
# for loop
data = []
for pg in quote_page:
 # query the website and return the html to the variable ‘page’
 page = urllib2.urlopen(pg)
# parse the html using beautiful soap and store in variable `soup`
 soup = BeautifulSoup(page, ‘html.parser’)
# Take out the <div> of name and get its value
 name_box = soup.find(‘h1’, attrs={‘class’: ‘name’})
 name = name_box.text.strip() # strip() is used to remove starting and trailing
# get the index price
 price_box = soup.find(‘div’, attrs={‘class’:’price’})
 price = price_box.text
# save the data in tuple
 data.append((name, price))
```

也可以一行行的存：

```
# open a csv file with append, so old data will not be erased
with open(‘index.csv’, ‘a’) as csv_file:
 writer = csv.writer(csv_file)
 # The for loop
 for name, price in data:
 writer.writerow([name, price, datetime.now()])
```

返回程序你可以同时获取两个索引。

### 高级爬虫技术

BeautifulSoup 简单，适合小规模爬虫。但是如果你对大规模爬虫感兴趣，你可以考虑使用其他：
1. Scrapy ,一个强大的python 爬虫框架
2. 尝试集成你的代码用一些公共API 数据抓取效率比抓取网页高太多。例如，看看 [Facebook Graph API](https://developers.facebook.com/docs/graph-api), 能帮你获取facebook页面不展示的隐藏数据。
3. 考虑使用MySQL的后端数据库来存储你的巨量数据。


### 采用DRY方法

DRY代表“Don't Repeat Yourself”(不要重复), 像[这个人](https://www.businessinsider.com/programmer-automates-his-job-2015-11)一样让每天任务自动化。
一些有趣的其他项目如跟踪你的Facebook好友的在线时间(在他们许可下)， 或者抓取一个论坛的主题列表然后进行自然语言处理(是当前人工智能的一个火热主题)。

如果你有任何问题，请在下面自由评论。

---
### 参考
http://www.gregreda.com/2013/03/03/web-scraping-101-with-python/
http://www.analyticsvidhya.com/blog/2015/10/beginner-guide-web-scraping-beautiful-soup-python/