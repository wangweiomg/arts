## ARTS - Tip

## 数字的格式化

工作中有个需求，要把货币数字，改成逢千逗号分隔的格式，于是就总结下实现方式。

### 1.  DecimalFormat 

```

		BigDecimal b1 = BigDecimal.valueOf(100543000.12672);

        DecimalFormat df = new DecimalFormat("###,##0");  // 100,543,000
        DecimalFormat df2 = new DecimalFormat("###,##0."); // 100,543,000.
        DecimalFormat df3 = new DecimalFormat("###,##0.0"); //100,543,000.1
        DecimalFormat df4 = new DecimalFormat("###,##0.00"); // 100,543,000.13
        DecimalFormat df5 = new DecimalFormat("###,##0.000"); //100,543,000.127
        DecimalFormat df6 = new DecimalFormat("###,##0.0000"); //100,543,000.1267
        
 
```

由此可知，在小数规定位数后，会做四舍五入操作。

### 2. NumberFormat


```

	NumberFormat f1 = NumberFormat.getCurrencyInstance();  // 货币格式化
	System.out.println(f1.format(b1)); //￥100,543,000.13
	
	NumberFormat f2 = NumberFormat.getNumberInstance();
	System.out.println(f2.format(b1)); //100,543,000.127
	
	System.out.println(f2.format(1123)); //1,123
	System.out.println(f2.format(1123.1)); //1,123.1
	System.out.println(f2.format(1123.14)); //1,123.14
	System.out.println(f2.format(1123.145)); //1,123.145
	System.out.println(f2.format(1123.1458)); //1,123.146
	System.out.println(f2.format(1123.14581)); //1,123.146
```

由此可知，如果小数位数超过3位，就会保留3位小数，四舍五入。

如果设置了不用千分位形式：

```
		f2.setGroupingUsed(false);

        System.out.println(f2.format(1123));
        System.out.println(f2.format(1123.1));
        System.out.println(f2.format(1123.14));
        System.out.println(f2.format(1123.145));
        System.out.println(f2.format(1123.1458));
        System.out.println(f2.format(1123.14581));
        
        	
        	
        	// output
        
      		1123
			1123.1
			1123.14
			1123.145
			1123.146
			1123.146

```

### 3. JavaScript 方式实现千分位


```

function number_format(number, decimals, dec_point, thousands_sep) {
    /*
    * 参数说明：
    * number：要格式化的数字
    * decimals：保留几位小数
    * dec_point：小数点符号
    * thousands_sep：千分位符号
    * */
    number = (number + '').replace(/[^0-9+-Ee.]/g, '');
    var n = !isFinite(+number) ? 0 : +number,
        prec = !isFinite(+decimals) ? 0 : Math.abs(decimals),
        sep = (typeof thousands_sep === 'undefined') ? ',' : thousands_sep,
        dec = (typeof dec_point === 'undefined') ? '.' : dec_point,
        s = '',
        toFixedFix = function (n, prec) {
            var k = Math.pow(10, prec);
            return '' + Math.ceil(n * k) / k;
        };

    s = (prec ? toFixedFix(n, prec) : '' + Math.round(n)).split('.');
    var re = /(-?\d+)(\d{3})/;
    while (re.test(s[0])) {
        s[0] = s[0].replace(re, "$1" + sep + "$2");
    }

    if ((s[1] || '').length < prec) {
        s[1] = s[1] || '';
        s[1] += new Array(prec - s[1].length + 1).join('0');
    }
    return s.join(dec);
}

```


