## ARTS - Tip

## JavaScript Date对象与时区

### GMT 和 UTC
我们先来解释下两个概念：
GMT 时间就是格林尼治时间，
> 格林尼治平时（英语：Greenwich Mean Time，GMT）是指位于英国伦敦郊区的皇家格林尼治天文台当地的平太阳时，因为本初子午线被定义为通过那里的经线。

> 自1924年2月5日开始，格林尼治天文台负责每隔一小时向全世界发放调时信息。

> 格林尼治平时的正午是指当平太阳横穿格林尼治子午线时（也就是在格林尼治上空最高点时）的时间。由于地球每天的自转是有些不规则的，而且正在缓慢减速，**因此格林尼治平时基于天文观测本身的缺陷，已经被原子钟报时的协调世界时（UTC）所取代。**


以下是UTC时间：
> 协调世界时（英语：Coordinated Universal Time，法语：Temps Universel Coordonné，简称UTC）是最主要的世界时间标准，其以原子时秒长为基础，在时刻上尽量接近于格林尼治标准时间。中华民国采用CNS 7648的《资料元及交换格式–资讯交换–日期及时间的表示法》（与ISO 8601类似）称之为世界协调时间。中华人民共和国采用ISO 8601:2000的国家标准GB/T 7408-2005《数据元和交换格式 信息交换 日期和时间表示法》中亦称之为协调世界时。
> 

我们要获取时间：

```
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳
```

注意，这里 new Date() 浏览器会解析成当地时间。

new Date(value);
这里 value 可以是字符串，但该字符串必须能被 Date.parse(value) 解析，
也可以是 时间戳。 



时间戳是各个时区都固定的

> 时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。
> 


还有注意点：
比如 getDate() 方法 返回月份的某一天
> 所指的月份中的某一天，使用本地时间。返回值是 1 ~ 31 之间的一个整数。

也就是说， getDate / getMonth / getFullYear  都是跟当地时区有关的，我们用到时区转化时候，就要先转化成 utc 时间，使用 getUTCDate() ... 再加偏移量。


使用：

```
var d = new Date('2018/08/07 12:33:45'); // 本地时间

// 把本地时间戳 转化成 UTC时间戳； d.getTimezoneOffset 返回时区偏移分钟数
var utctime = d.getTime() + d.getTimezoneOffset() * 60000;   

// 转化成 GMT+3 区时间是

var d = new Date(utctime + 3 * 3600 * 1000);


```



