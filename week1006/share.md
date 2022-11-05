## ARTS - Share

## Java8的分组与排序实践

假设有一个课程类，Course， 里面包含学生ID userId， 学科名称name, 和成绩grade:

```

public class Course {
    private Integer userId;
    private String name;
    private Integer grade;
    
    // setters and getters 
}

```

初始化一些数据

```

		 Course eng = new Course(1, "eng", 80);
        Course chi = new Course(1, "chi", 75);

        Course chi2 = new Course(2, "chi", 77);

        Course eng3 = new Course(3, "eng", 105);
        Course chi3 = new Course(3, "chi", 110);

        List<Course> courseList = Lists.newArrayList(eng, chi, chi2, eng3, chi3);
        
        

```


### 1. 需要按学生id分组

```
		
		Map<Integer, List<Course>> map = courseList.stream().collect(Collectors.groupingBy(Course::getUserId));

        System.out.println(map);
        
        
   	// output
      {
        1=[Course{userId=1, name='eng', grade=80}, Course{userId=1, name='chi', grade=75}], 
        2=[Course{userId=2, name='chi', grade=77}], 
        3=[Course{userId=3, name='eng', grade=105}, Course{userId=3, name='chi', grade=110}]
      }
        
        
        
```


### 2. 现在需要取得每个学生的总成绩

我们要对map的 value遍历，取得各科成绩和后存入新集合。取得各科成绩和 的过程可以使用reduce函数

于是就是：

```


List<Integer> gradeList = map.values().stream().map(i -> i.stream().map(Course::getGrade).reduce(0, Integer::sum)).collect(Collectors.toList());
        System.out.println(gradeList);
        
 // output
  [155, 77, 215]

```


### 3. 我们需要对学生总成绩排序

我们对学生总成绩排序，并返回分组map，使用Comparator

```

Map<Integer, List<Course>> sortedMap = map.entrySet().stream().sorted(Comparator.comparing(i -> i.getValue().stream().map(Course::getGrade).reduce(0, Integer::sum)))
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (e1, e2) -> e1, LinkedHashMap::new));


```



