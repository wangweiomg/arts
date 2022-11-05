## ARTS - Review  补2019.2.13

## [MySQL数据类型总览](https://dev.mysql.com/doc/refman/8.0/en/data-type-overview.html)

### 11.1.1 Numeric Type Overview

* unrelated adj. 无关联的.
* specify v. 具体说明
* subtraction n. 减法
* omit v. 忽略
* synonyms n. 同义词
* precision n. 精确，准确

A summary of the numeric data types follows. For additional information about properties and storage requirements of the numeric types, see [Section 11.2, “Numeric Types”](https://dev.mysql.com/doc/refman/8.0/en/numeric-types.html), and [Section 11.8, “Data Type Storage Requirements”](https://dev.mysql.com/doc/refman/8.0/en/storage-requirements.html).

主要的数字数据类型如下。更多关于数字类型的属性和存储信息，请看11.2章，和11.8章。

*M* indicates the maximum display width for integer types. The maximum display width is 255. Display width is unrelated to the range of values a type can contain, as described in [Section 11.2, “Numeric Types”](https://dev.mysql.com/doc/refman/8.0/en/numeric-types.html). For floating-point and fixed-point types, *M* is the total number of digits that can be stored.

*M* 表示整型表示的最大宽度。最大展示宽度是255.展示宽度和一个类型能包含的值的范围无关， 11.2看描述。对浮点和定点类型，*M*是可以存储的总位数。

If you specify `ZEROFILL` for a numeric column, MySQL automatically adds the `UNSIGNED` attribute to the column.

如果你具体说明 ```零填充 ZEROFILL```一个数字类型的列，MySQL给列自动填充 ```UNSIGNED```属性。

Numeric data types that permit the `UNSIGNED` attribute also permit `SIGNED`. However, these data types are signed by default, so the `SIGNED`attribute has no effect.

数字数据类型允许 ```UNSIGNED```和 ```SIGNED```属性。 但是，这些数据类型默认是已签名的，所以SIGNED 属性没影响。

`SERIAL` is an alias for `BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE`.

`SERIAL DEFAULT VALUE` in the definition of an integer column is an alias for `NOT NULL AUTO_INCREMENT UNIQUE`.

> Warning
>
> When you use subtraction between integer values where one is of type `UNSIGNED`, the result is unsigned unless the[`NO_UNSIGNED_SUBTRACTION`](https://dev.mysql.com/doc/refman/8.0/en/sql-mode.html#sqlmode_no_unsigned_subtraction) SQL mode is enabled. See [Section 12.10, “Cast Functions and Operators”](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html).

> 警告
>
> 当在其中一个是 UNSIGNED 类型的两个整数值之间做减法，结果也会是 unsigned ， 除非 NO_UNSIGNED_SUBTRACTION SQL模式是可用的。看12.10章节。

- [`BIT[(*M*)\]`](https://dev.mysql.com/doc/refman/8.0/en/bit-type.html)

  A bit-value type. *M* indicates the number of bits per value, from 1 to 64. The default is 1 if *M* is omitted.

  一个 位值类型。 M表示每个值多少位，从1到64.默认是1

- [`TINYINT[(*M*)\] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

  A very small integer. The signed range is `-128` to `127`. The unsigned range is `0` to `255`.

  一个非常小的整型。带签名范围从-128到127. 不带签名是从0 到255

- [`BOOL`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html), [`BOOLEAN`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

  These types are synonyms for [`TINYINT(1)`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html). A value of zero is considered false. Nonzero values are considered true:

  这两个类型是 TINYINT(1) 的同义词。0值表示 false, 非0表示 true:

  ```mysql
  mysql> select if(0, 'true', 'false');
  +------------------------+
  | if(0, 'true', 'false') |
  +------------------------+
  | false                  |
  +------------------------+
  1 row in set (0.00 sec)
  
  mysql> select if(1, 'true', 'false')
      -> ;
  +------------------------+
  | if(1, 'true', 'false') |
  +------------------------+
  | true                   |
  +------------------------+
  1 row in set (0.00 sec)
  
  mysql> select if(2, 'true', 'false');
  +------------------------+
  | if(2, 'true', 'false') |
  +------------------------+
  | true                   |
  +------------------------+
  1 row in set (0.00 sec)
  ```

  

  但是，TRUE 和 FALSE 这俩只是 1 和 0 的别名：
  
  ```mysql
  mysql> select if(0 = FALSE, 'true', 'false');
  +--------------------------------+
  | if(0 = FALSE, 'true', 'false') |
  +--------------------------------+
  | true                           |
  +--------------------------------+
  1 row in set (0.00 sec)
  
  mysql> select if(1 = TRUE, 'true', 'false');
  +-------------------------------+
  | if(1 = TRUE, 'true', 'false') |
  +-------------------------------+
  | true                          |
  +-------------------------------+
  1 row in set (0.00 sec)
  
  mysql> select if(2 = TRUE, 'true', 'false');
  +-------------------------------+
  | if(2 = TRUE, 'true', 'false') |
  +-------------------------------+
  | false                         |
  +-------------------------------+
  1 row in set (0.00 sec)
  
  mysql> select if(2 = FALSE, 'true', 'false');
  +--------------------------------+
  | if(2 = FALSE, 'true', 'false') |
  +--------------------------------+
  | false                          |
  +--------------------------------+
  1 row in set (0.00 sec)
  ```
  
  后两句展示了 2 既不等于0 ，也不等于 1.

* [`SMALLINT[(*M*)\] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

  A small integer. The signed range is `-32768` to `32767`. The unsigned range is `0` to `65535`.

  一个小的整型。有符号取值范围是 -32768 - 32767 。 无符号取值范围是0 到65535

* [`MEDIUMINT[(*M*)\] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

  A medium-sized integer. The signed range is `-8388608` to `8388607`. The unsigned range is `0` to `16777215`.

  一个中型整型。有符号范围是 -8388608 - 8388607.无符号范围是0 到16777215

* [`INT[(*M*)\] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

  A normal-size integer. The signed range is `-2147483648` to `2147483647`. The unsigned range is `0` to `4294967295`.

  一个正常整型。

* [`INTEGER[(*M*)\] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

  This type is a synonym for [`INT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html).

  INT 的同义词

* [`BIGINT[(*M*)\] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)

  A large integer. The signed range is `-9223372036854775808` to `9223372036854775807`. The unsigned range is `0` to `18446744073709551615`.

  更大的整型。

  `SERIAL` is an alias for `BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE`.

  Some things you should be aware of with respect to [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) columns:

  - All arithmetic is done using signed [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) or [`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html) values, so you should not use unsigned big integers larger than`9223372036854775807` (63 bits) except with bit functions! If you do that, some of the last digits in the result may be wrong because of rounding errors when converting a [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) value to a [`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html).

  - 所有的算术运算使用有符号BIGINT 或者DOUBLE值计算，所以不应该使用超过63位的无符号大整数 除了位方法。如果你那么做，一些后位可能出错，因为转换BIGINT 值为DOUBLE时候可能会出现舍入错误。

    MySQL can handle [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) in the following cases:

    MySQL能在如下情况中处理BIGINT

    - When using integers to store large unsigned values in a [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) column.
    - In [`MIN(*col_name*)`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_min) or [`MAX(*col_name*)`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_max), where *col_name* refers to a [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) column.
    - When using operators ([`+`](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_plus), [`-`](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_minus), [`*`](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_times), and so on) where both operands are integers.
    - 当使用整型存储大的无符号值在BIGINT列。
    - 在 min(col_name)或者 max(col_name) ， 且col_name 为 BIGINT 列。
    - 当使用操作符 + - * 等都是整型时候。

  - You can always store an exact integer value in a [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) column by storing it using a string. In this case, MySQL performs a string-to-number conversion that involves no intermediate double-precision representation.

  - 你可以在BIGINT列存储一个精确的整型值用字符型。这样的话，MySQL会做字符串到数字转换，不涉及中间精度表示问题。

  - The [`-`](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_minus), [`+`](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_plus), and [`*`](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_times) operators use [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html) arithmetic when both operands are integer values. This means that if you multiply two big integers (or results from functions that return integers), you may get unexpected results when the result is larger than `9223372036854775807`.

  - ``` -, +, *```操作使用BIGINT运算当两个是整型值。这意味着当你用两个大整数相乘(或者返回整型结果的方法)， 你可能获得不希望的值，当这个值超过 ```9223372036854775807```;

* [`DECIMAL[(*M*[,*D*\])] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html)

  A packed “exact” fixed-point number. *M* is the total number of digits (the precision) and *D* is the number of digits after the decimal point (the scale). The decimal point and (for negative numbers) the `-` sign are not counted in *M*. If *D* is 0, values have no decimal point or fractional part. The maximum number of digits (*M*) for [`DECIMAL`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html) is 65. The maximum number of supported decimals (*D*) is 30. If *D* is omitted, the default is 0. If *M* is omitted, the default is 10.
  
  `UNSIGNED`, if specified, disallows negative values.
  
  All basic calculations (`+, -, *, /`) with [`DECIMAL`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html) columns are done with a precision of 65 digits.
  
  DECIMAL(M[, D]) 
  
  一个包装的精确的固定点数字。 M 是位数，D是小数位数。如果D是0，就没有小数位。最大位数 M 是65.支持的最大小数位D 是30. 如果D缺省，默认就是0。如果M缺省，默认是10.
  
  
  
* [`DEC[(*M*[,*D*\])] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html), [`NUMERIC[(*M*[,*D*\])] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html), [`FIXED[(*M*[,*D*\])] [UNSIGNED] [ZEROFILL]`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html)

  These types are synonyms for [`DECIMAL`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html). The [`FIXED`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html) synonym is available for compatibility with other database systems.

  这个类型就是DECIMAL别称。

  