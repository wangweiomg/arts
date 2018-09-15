## ARTS - Tip

### 关于 js 接收前端有序map 变无序的问题

在实际开发中，本人遇到了一个后台排序map，传给前端变无序的问题。于是开始测试这个问题。

继续使用之前的一个例子 [week6 share: Java8的分组与排序实践](https://github.com/wangweiomg/arts/blob/master/week6/share.md)

### 造数据
建一个 课程类Course, 分组获得map,再排序，写一个控制类方法：

```

@RequestMapping("/map")
    @ResponseBody
    public Map<Integer, List<Course>> listMap() {

        Course eng = new Course(1001, "eng", 80);
        Course chi = new Course(1001, "chi", 75);

        Course chi2 = new Course(1002, "chi", 77);

        Course eng3 = new Course(1003, "eng", 105);
        Course chi3 = new Course(1003, "chi", 110);

        List<Course> courseList = Lists.newArrayList(eng, chi, chi2, eng3, chi3);
        Map<Integer, List<Course>> map = courseList.stream().collect(Collectors.groupingBy(Course::getUserId));

        System.out.println(map);

        List<Integer> gradeList = map.values().stream().map(i -> i.stream().map(Course::getGrade).reduce(0, Integer::sum)).collect(Collectors.toList());
        System.out.println(gradeList);

        Map<Integer, List<Course>> sortedMap = map.entrySet().stream().sorted(Comparator.comparing(i -> i.getValue().stream().map(Course::getGrade).reduce(0, Integer::sum)))
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (e1, e2) -> e1, LinkedHashMap::new));


        System.out.println(sortedMap);

        return sortedMap;


    }
```

根据总分进行排序，结果就是 1002， 1001， 1003

```
{1002=[Course(userId=1002, name=chi, grade=77)], 1001=[Course(userId=1001, name=eng, grade=80), Course(userId=1001, name=chi, grade=75)], 1003=[Course(userId=1003, name=eng, grade=105), Course(userId=1003, name=chi, grade=110)]}

```

我们用浏览器调试，js代码ajax请求来测试：


```

$(function () {
        var url = '/hello/map';

        $.get(url, data =>{
            console.log(data);
        });
    });
    
    
 // output
 
 {1001: Array(2), 1002: Array(1), 1003: Array(2)}
1001: (2) [{…}, {…}]
1002: [{…}]
1003: (2) [{…}, {…}]
__proto__: Object

```

根据输出发现顺序明显变了，说明转成js对象后小再调整下。
我们了解到js 的一个Object.entries方法：

>The Object.entries() method returns an array of a given object's own enumerable property [key, value] pairs, in the same order as that provided by a for...in loop (the difference being that a for-in loop enumerates properties in the prototype chain as well).
> 


```

console.log(Object.entries(data));
// output
(3) [Array(2), Array(2), Array(2)]
0: (2) ["1001", Array(2)]
1: (2) ["1002", Array(1)]
2: (2) ["1003", Array(2)]

```
这样的话就可以使用sort方法了

```
$.get(url, data =>{
            const r = Object.entries(data).sort(
                ([x1, x2], [y1, y2])=>
                    x2.reduce((x, {grade}) => x + grade, 0)
                    -
                    y2.reduce((x, {grade}) => x + grade, 0)
            );

            console.log(r);


        });
        
        
//output
(3) [Array(2), Array(2), Array(2)]
0: (2) ["1002", Array(1)]
1: (2) ["1001", Array(2)]
2: (2) ["1003", Array(2)]


```
结果输出正常。

接下来可以顺便看看 share 之 reduce 方法总结。


