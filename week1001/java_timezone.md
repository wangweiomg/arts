## ARTS
### Tip

### 场景与需求
项目使用国内一家公司的接口服务，该接口服务根据北京时间调用次数收费的，我们项目部署在国外服务器上，需求是统计接口每天调用量。

### 分析
我们要统计接口调用量，要把时区切换到北京时区。

### 实现方式
使用java8 java.time包 实现最方便。

代码：

```

	@Test
    public void test1() {

        ZoneId shanghaiZone = ZoneId.of(ZoneId.SHORT_IDS.get("CTT"));
        System.out.println(LocalDateTime.now(shanghaiZone));

        ZoneId zoneId = ZoneId.systemDefault();
        System.out.println(zoneId);
        LocalDateTime now = LocalDateTime.now(zoneId);
        System.out.println(now);

    }
        
```

打印结果：

```
2018-07-14T15:53:24.818
Asia/Shanghai
2018-07-14T15:53:24.822
```

### 备注

ZoneId 的SHORT_IDS 是该类缓存的时区代码，源码如下：

```
public static final Map<String, String> SHORT_IDS;
    static {
        Map<String, String> map = new HashMap<>(64);
        map.put("ACT", "Australia/Darwin");
        map.put("AET", "Australia/Sydney");
        map.put("AGT", "America/Argentina/Buenos_Aires");
        map.put("ART", "Africa/Cairo");
        map.put("AST", "America/Anchorage");
        map.put("BET", "America/Sao_Paulo");
        map.put("BST", "Asia/Dhaka");
        map.put("CAT", "Africa/Harare");
        map.put("CNT", "America/St_Johns");
        map.put("CST", "America/Chicago");
        map.put("CTT", "Asia/Shanghai");
        map.put("EAT", "Africa/Addis_Ababa");
        map.put("ECT", "Europe/Paris");
        map.put("IET", "America/Indiana/Indianapolis");
        map.put("IST", "Asia/Kolkata");
        map.put("JST", "Asia/Tokyo");
        map.put("MIT", "Pacific/Apia");
        map.put("NET", "Asia/Yerevan");
        map.put("NST", "Pacific/Auckland");
        map.put("PLT", "Asia/Karachi");
        map.put("PNT", "America/Phoenix");
        map.put("PRT", "America/Puerto_Rico");
        map.put("PST", "America/Los_Angeles");
        map.put("SST", "Pacific/Guadalcanal");
        map.put("VST", "Asia/Ho_Chi_Minh");
        map.put("EST", "-05:00");
        map.put("MST", "-07:00");
        map.put("HST", "-10:00");
        SHORT_IDS = Collections.unmodifiableMap(map);
    }
```

