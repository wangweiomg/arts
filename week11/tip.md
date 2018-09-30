## ARTS - Tip
## 关于Moment.js
一个好用的处理日期时间的插件: moment.js, 中文地址： [http://momentjs.cn/](http://momentjs.cn/)

在项目中引用，主要使用了两个功能，时间格式话和时区转换。

### 当前时间

```
var now = moment();  // 和 moment(new Date()) 是等价的

```

### 解析字符串

```
var date = moment("09-30-2018", "MM-DD-YYYY");

```


| 输入 | 例子 | 描述 |
| --- | --- | --- |
| YYYY | 2018| 4位年 |
| YY | 18 | 2位年 |
|M MM|12| 月份|
|D DD|1..31|日期|

### 时区

这里要用到moment-timezone.js 

```
moment.tz(new Date(), 'America/Los_Angeles').format('YYYY-MM-DD HH:mm:ss')
```
显示的是美国洛杉矶时间



