## ARTS - Share

## 说说 super-csv

官网地址是 http://super-csv.github.io/

有些系统需要csv格式的文件，所以就发现了这样一个框架.其实直接使用逗号分隔，数据换行也是可以的，框架带来了一定的便利性。

我们先看Writer, 一般是Jdbc 数据库读数据转化成JavaBean ，然后生成csv文件。

先定义一个客户bean

```
public @Data @AllArgsConstructor @NoArgsConstructor class CustomerBean {

    private String no;
    private String name;
    private Date birthDate;
    private Boolean deleteFlag;
    private Integer age;
    private String email;
    
}

```

然后使用 CsvBeanWriter 来写文件：

```
private static void writeWithCsvBeanWriter() throws IOException {

        CustomerBean jack = new CustomerBean("1", "jack", new Date(), true, 20, null);
        CustomerBean bob = new CustomerBean("2", "bob", new Date(), false, 12, "123@gmail.com");

        List<CustomerBean> list = Arrays.asList(jack, bob);
        ICsvBeanWriter writer = null;
        try {
            writer = new CsvBeanWriter(new FileWriter("target/cust.csv"), CsvPreference.STANDARD_PREFERENCE);
            
            // 这里注意要和 bean里面名字一样
            String[] header = {"no", "name", "BirthDate", "deleteFlag", "Age", "Email"};

            writer.writeHeader(header);

            for (CustomerBean bean : list) {
                writer.write(bean, header);

            }
        } finally {

            if (writer != null) {
                writer.close();
            }
        }

    }

```

结果就是：

```

no,name,BirthDate,deleteFlag,Age,Email
1,jack,Thu Aug 30 18:15:28 CST 2018,true,20,
2,bob,Thu Aug 30 18:15:28 CST 2018,false,12,123@gmail.com

```


有时我们需要对结果约束，就使用到了CellProcessor， 提前构造好

```

private static CellProcessor[] getProcessors() {
        final CellProcessor[] processors = new CellProcessor[] {

                new UniqueHashCode(), 
                new NotNull(), // 非空
                new FmtDate("yyyy-MM-dd"), // 日期格式化
                new Optional(new FmtBool("已删除", "正常")), // 转义布尔类型
                new LMinMax(18L, 150L), // 限定数据范围
                new Optional()
        };

        return processors;
    }


```

我们设置了一些Customer的参数，如年龄 18-150岁

```
writer.write(bean, header, getProcessors());
```
运行报异常 

```

Exception in thread "main" org.supercsv.exception.SuperCsvConstraintViolationException: 12 does not lie between the min (18) and max (150) values (inclusive)
processor=org.supercsv.cellprocessor.constraint.LMinMax
context={lineNo=3, rowNo=3, columnNo=5, rowSource=[2, bob, Thu Aug 30 18:20:22 CST 2018, false, 12, 123@gmail.com]}
	at org.supercsv.cellprocessor.constraint.LMinMax.execute(LMinMax.java:156)
	
```

根据异常信息知道 age 12不符合定义。

CsvListWriter类似，只是把Bean换成list

```
List<CustomerBean> list = Arrays.asList(jack, bob);
        ICsvListWriter writer = null;
        try {
            writer = new CsvListWriter(new FileWriter("target/cust.csv"), CsvPreference.STANDARD_PREFERENCE);
            String[] header = {"no", "name", "BirthDate", "deleteFlag", "Age", "Email"};

            writer.writeHeader(header);

            writer.write(list);
            
            // 或者加上校验
            //writer.write(list, getProcessors());
        } finally {

            if (writer != null) {
                writer.close();
            }
        }
```
