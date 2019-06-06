## ARTS - Review 补2019.2.20

## MySQL数据类型(2)

* temporal adj. 时间的
* guarantee n. 保证
* fractional adj. 很小的 ，分数的
* omit  v. 删除，省略
* assignment n. 分配
* equivalent adj. 相等的
* explicitly adv. 明确地

### 11.1.2 Date and Time Type Overview

日期时间类型概览

A summary of the temporal data types follows. For additional information about properties and storage requirements of the temporal types, see [Section 11.3, “Date and Time Types”](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-types.html), and [Section 11.8, “Data Type Storage Requirements”](https://dev.mysql.com/doc/refman/8.0/en/storage-requirements.html). For descriptions of functions that operate on temporal values, see [Section 12.7, “Date and Time Functions”](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html).

For the [`DATE`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) and [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) range descriptions, “supported” means that although earlier values might work, there is no guarantee.

对于 DATE 和 DATETIME  范围描述 ， "支持"意思是尽管值更早可能工作，但是不保证。

MySQL permits fractional seconds for [`TIME`](https://dev.mysql.com/doc/refman/8.0/en/time.html), [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html), and [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) values, with up to microseconds (6 digits) precision. To define a column that includes a fractional seconds part, use the syntax `*type_name*(*fsp*)`, where *type_name* is [`TIME`](https://dev.mysql.com/doc/refman/8.0/en/time.html), [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html), or [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html), and *fsp* is the fractional seconds precision. For example:

```sql
CREATE TABLE t1 (t TIME(3), dt DATETIME(6));
```

MySQL允许很小的秒数如 TIME， DATETIME 和TIMESTAMP 值，精确到微秒(6位小数)。 定义一个包含很小秒数的列，使用 type_name  fsp 标志  , type_name 是 TIME 、DATETIME 或 TIMESTAMP， fsp 是精确数。例如：

```sql
CREATE TABLE t1 (t TIME(3), dt DATETIME(6));
```



The *fsp* value, if given, must be in the range 0 to 6. A value of 0 signifies that there is no fractional part. If omitted, the default precision is 0. (This differs from the standard SQL default of 6, for compatibility with previous MySQL versions.)

fsp值如果定义的话，必须是0-6.一个0值表示没有小数位。任何TIMESTAMP 或DATETIME类型在表中的列可以自动初始化和更新属性。

Any [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) or [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) column in a table can have automatic initialization and updating properties.

- [`DATE`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)

  A date. The supported range is `'1000-01-01'` to `'9999-12-31'`. MySQL displays [`DATE`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) values in `'YYYY-MM-DD'` format, but permits assignment of values to [`DATE`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) columns using either strings or numbers.

  DATE 。 一个日期。支持范围是 1000-01-01 到9999-12-31. mysql显示 DATE 值是yyyy-MM-dd格式，但是允许使用字符或数字分配DATE列值。

- [`DATETIME[(*fsp*)\]`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)

  A date and time combination. The supported range is `'1000-01-01 00:00:00.000000'` to `'9999-12-31 23:59:59.999999'`. MySQL displays [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) values in `'*YYYY-MM-DD hh:mm:ss*[.*fraction*]'` format, but permits assignment of values to [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) columns using either strings or numbers.

  An optional *fsp* value in the range from 0 to 6 may be given to specify fractional seconds precision. A value of 0 signifies that there is no fractional part. If omitted, the default precision is 0.

  Automatic initialization and updating to the current date and time for [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) columns can be specified using `DEFAULT` and `ON UPDATE`column definition clauses, as described in [Section 11.3.4, “Automatic Initialization and Updating for TIMESTAMP and DATETIME”](https://dev.mysql.com/doc/refman/8.0/en/timestamp-initialization.html).

  DATETIME 是DATE 和TIME 合并的。支持范围是1000-01-01 00:00:00.000000 到  9999-12-31 23:59:59.999999。MySQL显示DATETIME 值使用  YYYY-MM-DD  hh:mm:ss*[.fraction*] 格式 ，但是允许传入字符和数字。

  一个可选的 fsp 值范围是0-6可以精确到小秒数。0值表示灭有小数位。如果缺省，默认是0.

  自动初始化和更新当前日期和时间当前列可以使用DEFAULT 和ON UPDATE 列定义语句。

- [`TIMESTAMP[(*fsp*)\]`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)

  A timestamp. The range is `'1970-01-01 00:00:01.000000'` UTC to `'2038-01-19 03:14:07.999999'` UTC. [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) values are stored as the number of seconds since the epoch (`'1970-01-01 00:00:00'` UTC). A [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) cannot represent the value `'1970-01-01 00:00:00'`because that is equivalent to 0 seconds from the epoch and the value 0 is reserved for representing `'0000-00-00 00:00:00'`, the “zero”[`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) value.

  An optional *fsp* value in the range from 0 to 6 may be given to specify fractional seconds precision. A value of 0 signifies that there is no fractional part. If omitted, the default precision is 0.

  The way the server handles `TIMESTAMP` definitions depends on the value of the [`explicit_defaults_for_timestamp`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_explicit_defaults_for_timestamp) system variable (see[Section 5.1.8, “Server System Variables”](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html)).

  If [`explicit_defaults_for_timestamp`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_explicit_defaults_for_timestamp) is enabled, there is no automatic assignment of the `DEFAULT CURRENT_TIMESTAMP` or `ON UPDATE CURRENT_TIMESTAMP` attributes to any [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) column. They must be included explicitly in the column definition. Also, any [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) not explicitly declared as `NOT NULL` permits `NULL` values.

  If [`explicit_defaults_for_timestamp`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_explicit_defaults_for_timestamp) is disabled, the server handles `TIMESTAMP` as follows:

  Unless specified otherwise, the first [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) column in a table is defined to be automatically set to the date and time of the most recent modification if not explicitly assigned a value. This makes [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) useful for recording the timestamp of an [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html) or [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html) operation. You can also set any [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) column to the current date and time by assigning it a `NULL` value, unless it has been defined with the `NULL`attribute to permit `NULL` values.

  Automatic initialization and updating to the current date and time can be specified using `DEFAULT CURRENT_TIMESTAMP` and `ON UPDATE CURRENT_TIMESTAMP` column definition clauses. By default, the first [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) column has these properties, as previously noted. However, any [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html) column in a table can be defined to have these properties.

  时间戳。 时间戳范围是1970-01-01 00:00:01.000000 UTC  到2038-01-19 03:14:07.99999 UTC. TIMESTAMP的值存储为从1970-01-01 00:00:00 至今的毫秒数。TIMESTAMP不能代表1970-01-01 因为那个0指的是0时代， 值0指的是0000-00-00 00:00:00

  可选的fsp值从0-6提供小数位数。0代表无小数位，默认0.

  服务器处理TIMESTAMP的方式是通过定义  explicit_defaults_for_timestamp 系统变量来设置的。如果该变量可用，不会自动处理 DEFAULT CURRENT_TIMESTAMP 和 UPDATE CRUUENT_TIMESTAMP.他们必须明确包含在定义语句里。所以任何没有明确定义NOT NULL 的列可以允许NULL 值。

  如果该属性禁用，服务器处理时间戳如下：

  除非明确定义，第一个 TIMESTAMP列定义为自动设置为最近的日期时间值 如果不精确要求一个值的话。这使得在INSERT 和 UPDATE 很有用。你也可以设置任意列为TIMESTAMP 通过分配他NULL来记录当前时间和日期，除非已经定义为NULL否则不会允许NULL值。

  自动初始化和更新为当前时间可以使用  DEFAULT CURRENT_TIMESTAMP 和 ON UPDATE CURRENT_TIMESTAMP  定义语句。默认第一个时间戳列有这些属性。然而，任何表中的TIMESTAMP列都可以定义这俩属性。

  

  

- [`TIME[(*fsp*)\]`](https://dev.mysql.com/doc/refman/8.0/en/time.html)

  A time. The range is `'-838:59:59.000000'` to `'838:59:59.000000'`. MySQL displays [`TIME`](https://dev.mysql.com/doc/refman/8.0/en/time.html) values in `'*hh:mm:ss*[.*fraction*]'` format, but permits assignment of values to [`TIME`](https://dev.mysql.com/doc/refman/8.0/en/time.html) columns using either strings or numbers.

  An optional *fsp* value in the range from 0 to 6 may be given to specify fractional seconds precision. A value of 0 signifies that there is no fractional part. If omitted, the default precision is 0.

  一个时间类型。MySQL显示TIME值是  hh:mm:ss 格式，但是允许使用数字和字符传入。

  一个可选的fsp值代表精确位数，默认0.

- [`YEAR[(4)\]`](https://dev.mysql.com/doc/refman/8.0/en/year.html)

  A year in four-digit format. MySQL displays [`YEAR`](https://dev.mysql.com/doc/refman/8.0/en/year.html) values in `YYYY` format, but permits assignment of values to [`YEAR`](https://dev.mysql.com/doc/refman/8.0/en/year.html) columns using either strings or numbers. Values display as `1901` to `2155`, and `0000`.

  For additional information about [`YEAR`](https://dev.mysql.com/doc/refman/8.0/en/year.html) display format and interpretation of input values, see [Section 11.3.3, “The YEAR Type”](https://dev.mysql.com/doc/refman/8.0/en/year.html).

  年是4位格式。MySQL显示YEAR 是 YYYY格式，但是允许字符和数字。

  Note

  MySQL 8.0 does not support the [`YEAR(2)`](https://dev.mysql.com/doc/refman/8.0/en/year.html) data type permitted in older versions of MySQL. For instructions on converting to [`YEAR(4)`](https://dev.mysql.com/doc/refman/8.0/en/year.html), see [YEAR(2) Limitations and Migrating to YEAR(4)](https://dev.mysql.com/doc/refman/5.7/en/migrating-to-year4.html) in [MySQL 5.7 Reference Manual](https://dev.mysql.com/doc/refman/5.7/en/).

The [`SUM()`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_sum) and [`AVG()`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_avg) aggregate functions do not work with temporal values. (They convert the values to numbers, losing everything after the first nonnumeric character.) To work around this problem, convert to numeric units, perform the aggregate operation, and convert back to a temporal value. Examples:

```sql
SELECT SEC_TO_TIME(SUM(TIME_TO_SEC(time_col))) FROM tbl_name;
SELECT FROM_DAYS(SUM(TO_DAYS(date_col))) FROM tbl_name;
```

注意

MySQL8.0 不支持YEAR(2) 数据类型。

SUM() 和 AVG() 聚合函数对时间类型不起作用。(他们把值转换为受罪，失去了第一恶非数值字符的一切)。 为了解决这个，可以转为数字单位，进行聚合操作，再转回时间类型。例如：

```sql
SELECT SEC_TO_TIME(SUM(TIME_TO_SEC(time_col))) FROM tbl_name;
SELECT FORM_DAYS(SUM(TO_DAYS(date_col)) FROM tbl_name;
```

