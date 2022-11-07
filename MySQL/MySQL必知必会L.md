# MySQL

# 第一章 了解SQL

### 数据库≠数据库软件

### 数据库中的表

一个数据库中的表名不应该重复

模式（schema）：关于数据库和表的布局及特性的信息

主键（primary key）：一列（或==一组列==），其值能够==唯一==区分表的每个行

主键的最好习惯：
不更新主键列中的值
不重用主键列的值
不在主键列中使用可能会更改的值

# 第三章 使用MySQL

### 创建一个数据源

```sql
create schema crashcourse
```

### 切换数据源

```sql
USE crashcourse
```

### 展示数据库

```sql
SHOW DATABASES
```

### 展示数据库中表的列表（要先进入一个数据源）

```sql
SHOW TABLES;
```

### 展示表中的列

```sql
SHOW COLUMNS FROM customers
```

它对每个字段返回一行，行中包含字段名、数据 类型、是否允许NULL、键信息、默认值以及其他信息（如字段cust_id 的auto_increment）。

### 什么是自动增量auto_increment？ 

某些表列需要唯一值。例如，订单编号、 雇员ID或（如上面例子中所示的）顾客ID。在每个行添加到表中时，MySQL可以自动地为每个行分配下一个可用编号，不 用在添加一行时手动分配唯一值（这样做必须记住最后一次使用的值）。这个功能就是所谓的自动增量。如果需要它，则必 须在用CREATE语句创建表时把它作为表定义的组成部分。我们将在第21章中介绍CREATE语句

### DESCRIBE语句

MySQL支持用DESCRIBE作为SHOW COLUMNS  FROM的一种快捷方式。换句话说，

```SQL
DESCRIBE customers;是 SHOW COLUMNS FROM customers;的一种快捷方式。
```

### SHOW的所有用途

输入HELP SHOW查看更多（CLION里面不知道为什么运行不了，命令行可以）

# 第四章 检索数据

### SQL关键字不区分大小写，表区分

所以最好是关键字全大写，表小写

### 检索多个列和所有列

```sql
SELECT prod_name,prod_name,prod_desc FROM products; #列名之间用逗号分隔。
SELECT * FROM products;# *称作通配符
```

### 排除检索中的重复项

```SQL
SELECT DISTINCT vend_id FROM products; #DISTINCT是单独的关键字
```

不能部分使用DISTINCT DISTINCT关键字应用于所有列而 不仅是前置它的列。如果给出SELECT DISTINCT vend_id,  prod_price，除非指定的两个列都不同，否则所有行都将被检索出来

### 限制数量的检索

```SQL
SELECT  prod_name FROM products LIMIT 5
SELECT  prod_name FROM products LIMIT 4,3
SELECT  prod_name FROM products LIMIT 3 OFFSET 4; #等同于4,3
```

LIMIT 3, 4的含义是从行4开始的3 行还是从行3开始的4行？如前所述，它的意思是从行3开始的4 行，这容易把人搞糊涂。  由于这个原因，MySQL 5支持LIMIT的另一种替代语法。LIMIT 4 OFFSET 3意为从行3开始取4行，就像LIMIT 3, 4一样。

检索出来的==第一行为行0==而不是行1。因此，LIMIT 1, 1 将检索出第二行而不是第一行。

### 完全限定的表名

```sql
SELECT products.vend_id FROM crashcourse.products;
```

这个语句完全等同于

```sql
SELECT vend_id FROM products;
```

# 第五章 数据的排序

### 子句（clause）

 SQL语句由子句构成，有些子句是必需的，而 有的是可选的。一个子句通常由一个关键字和所提供的数据组 成。子句的例子有SELECT语句的FROM子句。

### 数据的排序

其实，检索出的数据并不是以纯粹的随机顺序显示的。如果不排序，数据一般将以它在底层表中出现的顺序显示。

```SQL
SELECT  prod_name FROM products ORDER BY prod_name;
SELECT  prod_id,prod_price,prod_name FROM products ORDER BY prod_price,prod_name;#检索3个列，并按其中两个列对结果进行排序
SELECT  prod_name FROM products ORDER BY prod_name DESC;#降序排序
SELECT  prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC,prod_name;#price降序，name升序
```

对于上述第二个例子的输出，仅在多个行具有相同的prod_price 值时才对产品按prod_name进行排序。如果prod_price列中所有的值都是唯一（不同）的，则不会按prod_name排序

例如，如果要显示雇员清单， 可能希望按姓和名排序（首先按姓排序，然后在每个姓中再按名排序）。 如果多个雇员具有相同的姓，这样做很有用。

### 区分大小写和排序顺序 

# 第六章 过滤数据

### WHERE子句

```SQL
SELECT  prod_price FROM products WHERE prod_price=2.5;
```

在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。

= 等于 <> 不等于 != 不等于 < 小于 <= 小于等于 > 大于 >= 大于等于 BETWEEN 在指定的两个值之间

不等于也可以用 != 符号

```SQL
SELECT  prod_price FROM products WHERE prod_price BETWEEN 5 AND 10;
```

BWTWEEN关键词必须和AND一起使用

### 检查单个值

```sql
SELECT  prod_name,prod_price FROM products WHERE prod_name = 'fuses';
```

### IS NULL子句

```SQL
SELECT cust_id FROM customers WHERE cust_email IS NULL
```

在通过过滤选择出不具有特定值的行时，你可能希望返回具有NULL值的行。但是，不行。因为未知具有特殊的含义，数据库不知道它们是否匹配，所以在匹配过滤 或不匹配过滤时不返回它们。

因此，在过滤数据时，一定要验证返回数据中确实给出了被过滤列具有NULL的行

# 第七章 数据过滤

## 组合WHERE子句

```SQL
SELECT prod_id,prod_price,prod_name
FROM products
WHERE vend_id = 1003 AND prod_price <= 10;

SELECT prod_id,prod_price,prod_name
FROM products
WHERE vend_id = 1003 OR prod_price <= 10;
```

### 计算次序

WHERE可包含任意数目的AND和OR操作符。允许两者结合以进行复杂和高级的过滤。

但是，组合AND和OR带来了一个有趣的问题。为了说明这个问题，来 看一个例子。假如需要列出价格为10美元（含）以上且由1002或1003制 造的所有产品。下面的SELECT语句使用AND和OR操作符的组合建立了一个 WHERE子句

```sql
SELECT prod_price,prod_name
FROM products
WHERE vend_id = 1002 OR vend_id = 1003 AND prod_price >= 10;
```

返回的行中有两行价格小于10美元，显然， 返回的行未按预期的进行过滤。为什么会这样呢？

原因在于计 算的次序。==SQL（像多数语言一样）在处理OR操作符前，优先处理AND操作符。==当SQL看到上述WHERE子句时，它理解为由供应商1003制造的任何 价格为10美元（含）以上的产品，或者由供应商1002制造的任何产品， 而不管其价格如何。

正确的做法：

```sql
SELECT prod_price,prod_name
FROM products
WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;
```

### IN操作符

IN操作符用来指定条件范 围，范围中的每个条件都可以进行匹配。

```sql
SELECT prod_price,prod_name
FROM products
WHERE vend_id IN (1002 , 1003)
```

### NOT操作符

WHERE子句中的NOT操作符有且只有一个功能，那就是否定它之后所跟的任何条件

```sql
SELECT prod_price,prod_name
FROM products
WHERE vend_id NOT IN (1002 , 1003)
```

# 第八章 用通配符进行过滤

### 通配符（wildcard）

 用来匹配值的一部分的特殊字符

### 搜索模式（search pattern）

由字面值、通配符或两者组合构成的搜索条件。

## LIKE操作符

为在搜索子句中使用通配符，必须使用LIKE操作符。

### %通配符

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name LIKE "jet%"
#此例使用了搜索模式'jet%'。在执行这条子句时，将检索任意以jet起头的词
```

重要的是要注意到，除了一个或多个字符外，%还能匹配0个字符。% 代表搜索模式中给定位置的0个、1个或多个字符。

### _通配符

下划线的用途与%一样，但下划线==只匹配单个字符==而不是多个字符

# 第九章 使用正则表达式搜索

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '1000' #返回值

SELECT prod_id,prod_name
FROM products
WHERE prod_name LIKE '1000' #不会返回值
```

除关键字LIKE被REGEXP替代外，这条语句看上去非常像使用 LIKE的语句（第8章）。它告诉MySQL：REGEXP后所跟的东西作 为正则表达式（与文字正文1000匹配的一个正则表达式）处理。

正如第8章所述，LIKE匹配==列的整个文本==。如果被匹配的文本仅在列值中出现一部分，LIKE将不会找到它，相应的行也不被返回（除非使用通配符）。而REGEXP在列值内进行匹配，如果被匹配的文本在列值中出现，REGEXP将会找到它，相应的行将被返回。这是一 个非常重要的差别。

### 使用OR匹配

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '1000 | 2000'
```

### 匹配字符之一

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '[123] Ton'
#不能写成 1|2|3 Ton！会被判断成1 or 2 or (3 Ton)
```

这里，使用了正则表达式[123] Ton。[123]定义一组字符，它 的意思是匹配1或2或3，因此，1 ton和2 ton都匹配且返回.
正如所见，[ ] 是另一种形式的OR语句。事实上，正则表达式[123]Ton 为[1|2|3]Ton的缩写，也可以使用后者。

### 匹配范围

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '[1-9] Ton' #相当于[123456789]
```

### 匹配特殊字符

```sql
#为了匹配特殊字符，必须用\\为前导。\\-表示查找-，\\.表示查找.，\\\表示查找\。
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '\\.'
```

\\f 换页 \\n 换行 \\r 回车 \\t 制表 \\v 纵向制表

### 匹配字符类

[:alnum:] 任意字母和数字（同[a-zA-Z0-9]） 
[:alpha:] 任意字符（同[a-zA-Z]） 
[:blank:] 空格和制表（同[\\t]） 
[:cntrl:] ASCII控制字符（ASCII 0到31和127）
[:digit:] 任意数字（同[0-9]） 
[:graph:] 与[:print:]相同，但不包括空格 
[:lower:] 任意小写字母（同[a-z]） 
[:print:] 任意可打印字符 
[:punct:] 既不在[:alnum:]又不在[:cntrl:]中的任意字符 
[:space:] 包括空格在内的任意空白字符（同[\\f\\n\\r\\t\\v]） 
[:upper:] 任意大写字母（同[A-Z]） 
[:xdigit:] 任意十六进制数字（同[a-fA-F0-9]

### 匹配多个实例

```
* 0个或多个匹配
+ 1个或多个匹配（等于{1,}）
? 0个或1个匹配（等于{0,1}）
{n} 指定数目的匹配
{n,} 不少于指定数目的匹配
{n,m} 匹配数目的范围（m不超过255）
```

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '\\([0-9] sticks?\\)' #sticks前面的空格也是要匹配的
#结果会匹配到 stick和sticks
```

正则表达式\\([0-9] sticks?\\)需要解说一下。\\(匹配)， [0-9]匹配任意数字（这个例子中为1和5），sticks?匹配stick 和sticks（s后的?使s可选，因为?匹配它前面的任何字符的0次或1次出 现），\\)匹配)。没有?，匹配stick和sticks会非常困难。

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '[[:digit:]]{4}' 
#如前所述，[:digit:]匹配任意数字，因而它为数字的一个集合。{4}确切地要求它前面的字符（任意数字）出现4次，所以[[:digit:]]{4}匹配连在一起的任意4位数字。
```

### 定位符

先前的例子都是在文本中存在即可，而不考虑文本中其对应的位置，定位符的作用就是解决这个问题

```
常见定位符
^ 文本的开始
$ 文本的结尾
[[:<:]] 词的开始
[[:>:]] 词的结尾
```

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name REGEXP '^[0-9\\.]'
#^匹配串的开始。因此，^[0-9\\.]只在.或任意数字为串中第一个字符时才匹配它们。没有^，则还要多检索出4个别的行
```

# 第十章 创建计算字段

存储在表中的数据都不是应用程序所需要的。 我们需要直接从数据库中检索出转换、计算或格式化过的数据；而不是 检索出数据，然后再在客户机应用程序或报告程序中重新格式化。

这就是计算字段发挥作用的所在了。与前面各章介绍过的列不同， 计算字段并不实际存在于数据库表中。计算字段是运行时==在SELECT语句内创建的。==

## 字段

基本上与列（column）的意思相同，经常互换使用，不过数据库列一般称为列，而术语==字段通常用在计算字段的连接上==

## 拼接（concatenate）

```sql
SELECT Concat(vend_name,'(',vend_country,')')#CONCAT在课本中是Concat，不过应该都可以
FROM vendors
ORDER BY vend_name;
```

### 删除多余空格与表格命名

```sql
SELECT Concat(Rtrim(vend_name),'(',Rtrim(vend_country),')') AS vend_title
#Rtrim函数能够删除右侧多余空格,AS vend_title是给表起一个新名字
FROM vendors
ORDER BY vend_name;
```

## 执行算数计算

```
+ 加
- 减
* 乘
/ 除
```

```SQL
SELECT prod_id,quantity,item_price,quantity*item_price AS expanded_price
FROM orderitems
WHERE order_num = 20005;
```

### 测试计算结果

SELECT提供了测试和试验函数与计算的一个 很好的办法。虽然SELECT通常用来从表中检索数据，但可以 省略FROM子句以便简单地访问和处理表达式。例如，SELECT  3*2;将返回6，SELECT Trim('abc');将返回abc，而SELECT  Now()利用Now()函数返回当前日期和时间。通过这些例子， 可以明白如何==根据需要使用SELECT进行试验。==

# 第十一章 使用数据处理函数

## 文本处理函数

```
Left() 返回串左边的字符
Length() 返回串的长度
Locate() 找出串的一个子串
Lower() 将串转换为小写
LTrim() 去掉串左边的空格
Right() 返回串右边的字符
RTrim() 去掉串右边的空格
Soundex() 返回串的SOUNDEX值
SubString() 返回子串的字符
Upper() 将串转换为大写
```

SOUNDEX是一个将任何文 本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX考虑了类似 的发音字符和音节，使得能对串进行发音比较而不是字母比较。虽然 SOUNDEX不是SQL概念，但MySQL（就像多数DBMS一样）都提供对 SOUNDEX的支持。

```sql
SELECT cust_name,cust_contact
FROM customers
WHERE Soundex(cust_contact) = Soundex('Y Lie')
#这个检索返回的是
#Coyote Inc.,Y Lee
#因为Y Lee和Y Lie相近
```

## 日期和时间处理函数

```
AddDate() 增加一个日期（天、周等）
AddTime() 增加一个时间（时、分等）
CurDate() 返回当前日期
CurTime() 返回当前时间
Date() 返回日期时间的日期部分
DateDiff() 计算两个日期之差
Date_Add() 高度灵活的日期运算函数
Date_Format() 返回一个格式化的日期或时间串
Day() 返回一个日期的天数部分
DayOfWeek() 对于一个日期，返回对应的星期几
Hour() 返回一个时间的小时部分
Minute() 返回一个时间的分钟部分
Month() 返回一个日期的月份部分
Now() 返回当前日期和时间
Second() 返回一个时间的秒部分
Time() 返回一个日期时间的时间部分
Year() 返回一个日期的年份部分
```

日期，不管是插入或更新表值还是用WHERE子句进行过滤，日期必须为 格式yyyy-mm-dd。因此，2005年9月1日，给出为2005-09-01。虽然其他的 日期格式可能也行，但这是首选的日期格式，因为它==排除了多义性==（如， 04/05/06是2006年5月4日或2006年4月5日或2004年5月6日或……）

```SQL
SELECT cust_id,order_num
FROM orders
WHERE order_date = '2005-09-01'
```

但是，使用WHERE order_date = '2005-09-01'可靠吗？order_  date的数据类型为datetime。这种类型存储日期及时间值。样例表中 的值全都具有时间值00:00:00，但实际中很可能并不总是这样。如果 用当前日期和时间存储订单日期（因此你不仅知道订单日期，还知道 下 订 单 当 天 的 时 间 ）， 怎 么 办 ？ 比 如 ， ==存 储 的 order_date 值 为 2005-09-01 11:30:05，则WHERE order_date = '2005-09-01'失败==。 即使给出具有该日期的一行，也不会把它检索出来，因为WHERE匹配失败

为此，必须使用Date() 函数。Date(order_date)指示MySQL仅提取列的日期部分:

```sql
SELECT cust_id,order_num
FROM orders
WHERE Date(order_date) = '2005-09-01'
```

### 日期范围检索

```sql
SELECT cust_id,order_num
FROM orders
WHERE Date(order_date) BETWEEN '2005-09-01' AND '2005-9-30'
```

同样也是用BETWEEN操作

还有一种方法无需记住每个月有多少天

```sql
SELECT cust_id,order_num
FROM orders
WHERE Year(order_date)=2005 AND Month(order_date)=9;
```

### 日期中的引号

当使用年月日组合时，要加上单引号，不然会被当做是数字相减，而只表示年或者月或者日时，不需要单引号（也可以加，都行）

## 数值处理函数

```
Abs() 返回一个数的绝对值
Cos() 返回一个角度的余弦
Exp() 返回一个数的指数值
Mod() 返回除操作的余数
Pi() 返回圆周率
Rand() 返回一个随机数
Sin() 返回一个角度的正弦
Sqrt() 返回一个数的平方根
Tan() 返回一个角度的正切
```

# 第十二章 汇总数据

## 聚集函数

```
AVG() 返回某列的平均值
COUNT() 返回某列的行数
MAX() 返回某列的最大值
MIN() 返回某列的最小值
SUM() 返回某列值之和
```

```sql
SELECT AVG(prod_price) AS AVG_price
FROM products
WHERE vend_id = 1003#可以指定某一个vend_id下的平均值
```

### 聚集不同的值

```SQL
SELECT AVG(DISTINCT prod_price) AS AVG_price #DISTINCT关键字指明了不会将重复数据纳入计算
FROM products
WHERE vend_id = 1003
```

虽然DISTINCT从技术上可 用于MIN()和MAX()，但这样做实际上没有价值。一个列中的 最小值和最大值不管是否包含不同值都是相同的。

# 第十三章 分组数据

## 创建分组 GROUP BY

```sql
SELECT vend_id,COUNT(*) AS num_prods
FROM products
GROUP BY vend_id #可以COUNT(*)的输出按vend_id分组
##输出：
vend_id,num_prods
1001,3
1002,2
1003,7
1005,2

##如果删掉GROUP BY的输出
vend_id,num_prods
1001,14

#因为num_prods直接将COUNT全部输出了
```

GROUP BY子句指示MySQL分组数据，然后==对每个组==而不是整个结果集进行聚集。

```
 GROUP BY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
 GROUP BY子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在SELECT中使用表达式，则必须在
GROUP BY子句中指定相同的表达式。不能使用别名。
 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
 GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。
```

```sql
按照 c列来分组
select count(a),c from test group by c（可以想想把c列作为类似key主键，让它唯一，看看分几组）

按照 b c两个条件来分组
select count(a),b,c from test group by b,c （可以想想把b和c列一起作为类似key主键，让它唯一，看看分几组）

按照 c b 顺序分组
select count(a),b,c from test group by c,b （可以想想把c和b列一起作为类似key主键，让它唯一，看看分几组）

bc顺序和cb顺序的区别不大，但是对输出的顺序有影响，要控制输出顺序的话可能要加上排序函数 ORDER BY

一般在使用 GROUP BY 子句时，应该也给出 ORDER BY 子句。这是保证数据正确排序的唯一方法。千万不要仅依赖 GROUP BY 排序数据。
```

## 过滤分组 HAVING

```SQL
SELECT cust_id,COUNT(*) AS orders
FROM orders
GROUP BY cust_id
HAVING COUNT(*)>=2
```

这里WHERE子句不起作用，因为==过滤是基于分组聚集值==而不是特定行值的

### WHERE和HAVING的区别

HAVING和WHERE的差别 这里有另一种理解方法，==WHERE在数据分组前进行过滤，HAVING在数据分组后进行过滤。==这是一个重要的区别，WHERE排除的行不包括在分组中。这可能会改变计 算值，从而影响HAVING子句中基于这些值过滤掉的分组

# 第十四章 使用子查询

```SQL
SELECT cust_id
FROM orders
WHERE order_num IN (SELECT order_num
                    FROM orderitems
                    WHERE prod_id = 'TNT2')
```

子查询返回两个订单号：20005和20007。然后，这两个值以IN操作符要求的逗号分隔的格式传递给外部查询的WHERE子句。

虽然子查询一般与IN操作符结合使用，但也可以用于测试等于（=）、 不等于（<>）等。

## 作为计算字段使用子查询

```sql
SELECT cust_name,cust_state,(SELECT COUNT(*)
                             FROM orders
                             WHERE orders.cust_id = customers.cust_id) AS orders #注意这里的限定列名
FROM customers
ORDER BY cust_name
```

### 相关子查询

子查询中的WHERE子句与前面使用的WHERE子句稍有不同，因为它使用了完全限定列名（在第4章中首次提到）。下面的语句告诉SQL比较 orders表中的cust_id与当前正从customers表中检索的cust_id。

这种类型的子查询称为相关子查询。任何时候只要列名可能有多义 性，就必须使用这种语法（表名和列名由一个句点分隔）。

# 第十五章 联结表

联结：将多个表中的数据用一个SELECT就能选出

重要的是，要理解联结不是物理实体。换句 话说，它在实际的数据库表中不存在。联结由MySQL根据需 要建立，它存在于查询的执行当中。

```sql
SELECT vend_name,prod_name,prod_price
FROM vendors,products
WHERE vendors.vend_id = products.vend_id
ORDER BY vend_name,prod_name
```

这里，最大的差别是所指定的两个列（prod_name 和prod_price）在一个表中，而另一个列（vend_name）在另一个表中。

在联结两个表时，你实际上做 的是将第一个表中的每一行与第二个表中的每一行配对。==WHERE子句作为过滤条件==（本来是笛卡尔积），它只包含那些匹配给定条件（这里是联结条件）的行。

## 联结多个表

```sql
SELECT vend_name,prod_name,prod_price,quantity
FROM vendors,products,orderitems
WHERE vendors.vend_id = products.vend_id
	AND orderitems.pro_id = products.pro_id
	AND order_num = 20005
ORDER BY vend_name,prod_name
```

## 笛卡尔积

```sql
SELECT vend_name,prod_name,prod_price
FROM vendors,products
ORDER BY vend_name,prod_name
```

去掉了WHERE的过滤条件，就是笛卡尔积——检索出的行的数目将是第一个表中的行数乘 以第二个表中的行数。

## 内部联结

```sql
SELECT vend_name,prod_name,prod_price
FROM vendors INNER JOIN products
ON vendors.vend_id = products.vend_id
```

这里的 INNER JOIN …… ON 代替了WHERE，这样写的好处就是比WHERE更加容易读懂

## 子查询与联结查询的转换

```SQL
SELECT cust_name,cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'TNT2'));

SELECT cust_name,cust_contact
FROM  customers,orders,orderitems
WHERE customers.cust_id = orders.cust_id
AND orderitems.order_num = orders.order_num
AND prod_id = 'TNT2'
```

这两个子查询的效果是一样的，但是联结的效率会更高

# 第十六章 创建高级联结

## 创建表别名

```sql
SELECT cust_name,cust_contact
FROM  customers AS c,orders AS o,orderitems AS oi
WHERE c.cust_id = o.cust_id
AND oi.order_num = o.order_num
AND prod_id = 'TNT2'
```

## 自联结

假如你发现某物品（其ID为DTNTR）存在问题，因此想知道生产该物 品的供应商生产的其他物品是否也存在这些问题。此查询要求首先找到 生产ID为DTNTR的物品的供应商，然后找出这个供应商生产的其他物品。

```SQL
SELECT prod_id,prod_name
FROM products
WHERE vend_id = (SELECT vend_id
                 FROM products
                 WHERE prod_id = 'TNT2')
```

可以改成自联结

```SQL
SELECT p1.prod_id,p1.prod_name
FROM products AS p1, products AS p2
WHERE p1.vend_id = p2.vend_id
AND p2.pro_id = 'TNT2'
```

## 自然联结

就是一种自动帮你把含有相同元素（例如主键id）的行联结起来，从而排除重复出现的行

```sql
SELECT  cust_id,prod_id,vend_id
FROM  customers NATURAL JOIN products NATURAL JOIN vendors
```

## 外部联结

与内部联结最大的不同之处是，它可以将不存在的行包括进来

```sql
select customers.cust_id,orders.order_num
from customers LEFT OUTER JOIN orders
ON customers.cust_id = orders.cust_id
```

左右外连接的不同在于LEFT是左边不为空，右边为空，右连接反之

## 带聚集函数的联结

```SQL
SELECT customers.cust_name,customers.cust_id,
       COUNT(orders.order_num) AS num_order
FROM customers INNER JOIN orders
ON customers.cust_id = orders.cust_id
GROUP BY  customers.cust_name
```

# 第十七章 组合查询

有两种基本情况，其中需要使用组合查询： 
 在单个查询中从不同的表返回类似结构的数据
 对单个表执行多个查询，按单个查询返回数据

```sql
SELECT vend_id,prod_id,prod_price
FROM products
WHERE prod_price<=5
UNION
SELECT vend_id,prod_id,prod_price
FROM products
WHERE vend_id IN (1001,1002)
#将两个SELECT的结果输出在一个表中
```

在这个例子中，UNION不如使用OR

```sql
SELECT vend_id,prod_id,prod_price
FROM products
WHERE prod_price<=5
OR vend_id IN (1001,1002)
```

## UNION的规则

 UNION必须由两条或两条以上的SELECT语句组成，语句之间用关 键字UNION分隔（因此，如果组合4条SELECT语句，将要使用3个 UNION关键字）。 
 UNION中的每个查询必须包含相同的列、表达式或聚集函数（不过各个列不需要以相同的次序列出）。 
 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以 隐含地转换的类型（例如，不同的数值类型或不同的日期类型）

## UNION查询会自动去掉重复行

可以改为使用

```sql
UNION ALL
```

进而保留重复行

## 对UNION组合使用排序

只能够在最后加ORDER BY，每个组合查询只能有一个排序语句

# 第十八章 全文本搜索

先前的正则匹配虽然方便，但是存在性能低下以及较低智能的问题，全文本搜索就能解决这个问题

## 启用全文本搜索

```sql
CREATE TABLE productnotes
(
  note_id    int           NOT NULL AUTO_INCREMENT,
  prod_id    char(10)      NOT NULL,
  note_date datetime       NOT NULL,
  note_text  text          NULL ,
  PRIMARY KEY(note_id),
  FULLTEXT(note_text) #全文本搜索
) ENGINE=MyISAM;
```

为了进行全文本搜索， MySQL根据子句FULLTEXT(note_text)的指示对它进行索引。这里的 FULLTEXT索引单个列，如果需要也可以指定多个列。

在定义之后，MySQL自动维护该索引。在增加、更新或删除行时， 索引随之自动更新。

## 不要在导入数据时使用FULLTEXT 

更新索引要花时间，虽然 不是很多，但毕竟要花时间。如果正在导入数据到一个新表， 此时不应该启用FULLTEXT索引。应该首先导入所有数据，然后再修改表，定义FULLTEXT。这样有助于更快地导入数据（而且使索引数据的总时间小于在导入每行时分别进行索引所需 的总时间）。

## 执行全文本搜索

在索引之后，使用两个函数Match()和Against()执行全文本搜索， 其中Match()指定被搜索的列，Against()指定要使用的搜索表达式

```sql
SELECT note_text
FROM productnotes
WHERE MATCH(note_text) AGAINST('rabbit') #传递给Match()的值必须与FULLTEXT()定义中的相同。如果指定多个列，则必须列出它
```

此SELECT语句检索单个列note_text。由于WHERE子句，一个全 文本搜索被执行。Match(note_text)指示MySQL针对指定的列进行搜索，==Against('rabbit')指定词rabbit作为搜索文本。==由于有两行包含词rabbit，这两个行被返回。

## 全文本搜索和LIKE的区别

上述代码也能用LIKE的方式取代

```SQL
SELECT note_text
FROM productnotes
WHERE note_text LIKE "%rabbit%"
```

这条SELECT语句同样检索出两行，但次序不同（虽然并不总是 出现这种情况）。

上述两条SELECT语句都不包含ORDER BY子句。==后者（使用LIKE）以 不特别有用的顺序返回数据。==前者（使用全文本搜索）返回以文本匹配的良好程度排序的数据。两个行都包含词rabbit，但包含词rabbit作为 第3个词的行的等级比作为第20个词的行高。这很重要。全文本搜索的一 个重要部分就是对结果排序。具有较高等级的行先返回（因为这些行很可能是你真正想要的行）。

可以通过在SELECT中而不是WHERE中使用MATCH来查看排序优先级

```sql
SELECT note_text,MATCH(note_text) AGAINST('rabbit') AS rank
FROM productnotes
```

包含词rabbit 的两个行每行都有一个rank值，==文本中词靠前的行的等级值比词靠后的行的等级值高。==

## 查询扩展

查询扩展用来设法放宽所返回的全文本搜索结果的范围。考虑下面 的情况。你想找出所有提到anvils的注释。只有一个注释包含词anvils， 但你还想找出可能与你的搜索有关的所有其他行，==即使它们不包含词anvils==

```sql
SELECT note_text
FROM productnotes
WHERE MATCH(note_text) AGAINST('anvils' WITH QUERY EXPANSION )
```

这次返回了7行。第一行包含词anvils，因此等级最高。第二行与anvils无关，但因为它包含第一行中的两个词（customer 和recommend），所以也被检索出来。第3行也包含这两个相同的词，但它们在文本中的位置更靠后且分开得更远，因此也包含这一行，但等级为 第三。第三行确实也没有涉及anvils（按它们的产品名）

## 布尔文本搜索

以布尔方式，可以提供关于如下内容的细节：
 要匹配的词； 
 要排斥的词（如果某行包含这个词，则不返回该行，即使它包含 其他指定的词也是如此）；
 排列提示（指定某些词比其他词更重要，更重要的词等级更高）； 
 表达式分组； 
 另外一些内容

布尔方式不同于迄今为 止使用的全文本搜索语法的地方在于，==即使没有定义FULLTEXT索引，也可以使用它。==但这是一种==非常缓慢==的操作

```SQL
SELECT note_text
FROM productnotes
WHERE MATCH(note_text) AGAINST('heavy' IN BOOLEAN MODE )
```

此全文本搜索检索包含词heavy的所有行（有两行）。其中使用 了关键字IN BOOLEAN MODE，但实际上没有指定布尔操作符， 因此，其结果==与没有指定布尔方式的结果相同==

```
SELECT note_text
FROM productnotes
WHERE MATCH(note_text) AGAINST('heavy -rope*' IN BOOLEAN MODE )
```

这次只返回一行。这一次仍然匹配词heavy，但-rope* 明确地指示MySQL排除包含rope*

### 布尔操作符

```
+ 包含，词必须存在
- 排除，词必须不出现
> 包含，而且增加等级值
< 包含，且减少等级值
() 把词组成子表达式（允许这些子表达式作为一个组被包含、排除、排列等）
~ 取消一个词的排序值
* 词尾的通配符
"" 定义一个短语（与单个词的列表不一样，它匹配整个短语以便包含或排除这个短语）
```

## 全文本搜索的一些说明

 在索引全文本数据时，短词被忽略且从索引中排除。短词定义为 那些具有3个或3个以下字符的词（如果需要，这个数目可以更改）。 
 MySQL带有一个内建的非用词（stopword）列表，这些词在索引全文本数据时总是被忽略。如果需要，可以覆盖这个列表（请参 阅MySQL文档以了解如何完成此工作）。 
 许多词出现的频率很高，搜索它们没有用处（返回太多的结果）。 因此，MySQL规定了一条50%规则，如果一个词出现在50%以上 的行中，则将它作为一个非用词忽略。50%规则不用于IN BOOLEAN  MODE。 
 如果表中的行数少于3行，则全文本搜索不返回结果（因为每个词 或者不出现，或者至少出现在50%的行中）。 
 忽略词中的单引号。例如，don't索引为dont。 
 不具有词分隔符（包括日语和汉语）的语言不能恰当地返回全文 本搜索结果。 
 如前所述，仅在MyISAM数据库引擎中支持全文本搜索。

# 第十九章 插入数据

## INSERT语句

顾名思义，INSERT是用来插入（或添加）行到数据库表的。插入可 以用几种方式使用： 
 插入完整的行； 
 插入行的一部分； 
 插入多行； 
 插入某些查询的结果。

```sql
INSERT INTO customers
VALUES(10001, 'Coyote Inc.', '200 Maple Lane', 'Detroit', 'MI', '44444', 'USA', 'Y Lee', NULL);#可以为空值
```

INSERT语句一般==不会产生输出==

## 虽然这种语法很简单，但并不安全，应该尽量避免使用。

上面的SQL 语句高度依赖于表中列的定义次序，并且还依赖于其次序容易获得的信 息。即使可得到这种次序信息，也不能保证下一次表结构变动后各个列 保持完全相同的次序。因此，编写依赖于特定列次序的SQL语句是很不安 全的。如果这样做，有时难免会出问题。

### 更安全的方式

```sql
INSERT INTO customers(cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country, cust_contact, cust_email)
VALUES(10001, 'Coyote Inc.', '200 Maple Lane', 'Detroit', 'MI', '44444', 'USA', 'Y Lee', NULL);
```

其优点是，即使表的结构改变， 此INSERT语句仍然能正确工作。

## 插入检索中的数据

```sql
INSERT INTO new_table(field1,field2,...) 
SELECT field1,field2,... FROM old_table ...
```

# 第二十章 更新和删除数据

## 更新数据UPDATE

 更新表中特定行； 
 更新表中所有行。

### 不要省略WHERE子句

 在使用UPDATE时一定要注意细心。因为稍不注意，就会更新表中所有行。

```sql
UPDATE customers
SET cust_emal = 'jsdajal'
	cust_name = 'sdasd'
WHERE cust_id = 10005
```

UPDATE语句以WHERE子句结束，它告诉MySQL更新哪一行。==没有WHERE子句，MySQL将会用这个电子邮件地址更新customers表中所有行，==这不是我们所希望的。

### UPDATE中也可以使用子查询

UPDATE语句中可以使用子查询，使得能用SELECT语句检索出的数据更新列数据。

### IGNORE关键字

如果用UPDATE语句更新多行，并且在更新这些行中的一行或多行时出一个现错误，则整个UPDATE操作被取消（错误发生前更新的所有行被恢复到它们原来的值）。为即使是发生错误，也继续进行更新，可使用IGNORE关键字，如下所示： 

```sql
UPDATE IGNORE customers…
```

### 删除某列

为了删除某个列的值，可设置它为NULL（假如表定义允许NULL值）。 如下进行：

```sql
UPDATE customers
SET cust_emal = NULL
WHERE cust_id = 10005
```

## 删除数据DELETE

DELETE的用法和UPDATE相似，只是不需要加上SET

# 第二十一章 创建和操纵表

## 创建表

一般有两种创建表的方法： 
 使用具有交互式创建和管理表的工具（如第2章讨论的工具）； 
 表也可以直接用MySQL语句操纵

```sql
CREATE TABLE customers
(
  cust_id      int       NOT NULL AUTO_INCREMENT,
  cust_name    char(50)  NOT NULL ,
  cust_address char(50)  NULL ,
  cust_city    char(50)  NULL ,
  cust_state   char(5)   NULL ,
  cust_zip     char(10)  NULL ,
  cust_country char(50)  NULL ,
  cust_contact char(50)  NULL ,
  cust_email   char(255) NULL ,
  PRIMARY KEY (cust_id)
) ENGINE=InnoDB;
```

### 语句格式

以何种缩进格式安排SQL语句没有规定， 但我强烈推荐采用某种缩进格式。

### 创建的表必须不存在于当前数据库中

## 使用NULL值

### 关键字NOT NULL

阻止插入没有值的列。如果 试图插入没有值的列，将返回错误，且插入失败。

## 主键

正如所述，主键值必须唯一。即，表中的每个行必须具有唯一的主键值。如果主键使用单个列，则它的值必须唯一。==如果使用多个列，==则 这些列的组合值必须唯一。

```sql
CREATE TABLE orderitems
(
  order_num  int          NOT NULL DEFAULT 1,
  order_item int          NOT NULL ,
  prod_id    char(10)     NOT NULL ,
  quantity   int          NOT NULL ,
  item_price decimal(8,2) NOT NULL ,
  PRIMARY KEY (order_num, order_item) #主键含有多个列
) ENGINE=InnoDB;
```

## AUTO_INCREMENT

AUTO_INCREMENT告诉MySQL，本列每当增加一行时自动增量。每次 执行一个INSERT操作时，MySQL自动对该列增量（从而才有这个关键字 AUTO_INCREMENT），给该列赋予下一个可用的值。这样给每个行分配一个 唯一的cust_id，从而可以用作主键值。

### 每个表只允许一个AUTO_INCREMENT列，且必须被索引（成为主键）

## 默认值DEFAULT

在设置数据时，可以先预定义一个默认值，使其在创建表时就拥有这个值

## 引擎类型

ENGINE=InnoDB;

在你使用CREATE TABLE语句时，该引擎具体创建表，而在你使用SELECT 语句或进行其他数据库处理时，该引擎在内部处理你的请求。多数时候， 此引擎都隐藏在DBMS内，不需要过多关注

为什么要发行多种引擎呢？因为它们具有各自不同的功能和特性， 为不同的任务选择正确的引擎能获得良好的功能和灵活性

当然，你完全可以忽略这些数据库引擎。如果省略ENGINE=语句，则 使用默认引擎（很可能是MyISAM），多数SQL语句都会默认使用它。但并 不是所有语句都默认使用它，这就是为什么ENGINE=语句很重要的原因 （也就是为什么本书的样列表中使用两种引擎的原因）。

### 以下是几个需要知道的引擎：

  InnoDB是一个可靠的事务处理引擎（参见第26章），它不支持全文 本搜索； 
 MEMORY在功能等同于MyISAM，但由于数据存储在内存（不是磁盘） 中，速度很快（特别适合于临时表）； 
 MyISAM是一个性能极高的引擎，它支持全文本搜索（参见第18章）， 但不支持事务处理。

## 更新表

更新表并不等于更新数据，更新表是更新表的定义

```SQL
ALTER TABLE vendors
ADD vend_phone CHAR(20)

ALTER TABLE vendors
DROP TABLE vend_phone #删除某一项数据
```

### ALTER一般用于定义外键

```sql
ALTER TABLE vendors
ADD CONSTRAINT fk_orderitems_orders
FOREIGN KEY (order_num) REFERENCES orders (order_num)
```

### 在使用ALTER前最好进行备份

因为如果不小心删除了重要列很麻烦

## 删除表

DROP即可

## 重命名表

```SQL
RENAME TABLE customers TO cust
```

# 第二十二章 使用视图

视图是虚拟的表，视图只包含==使用时==动态查询的数据

```SQL
CREATE VIEW productcustomers AS
SELECT cust_name,cust_contact,pro_id
FROM customers,orders,orderitems
WHERE customers.cust_id = orders.cust_id
AND orderitems.order_num = orders.order_num
```

在视图创建之后，可以用与表基本相同的方式利用它们。可以对视 图执行SELECT操作，过滤和排序数据，将视图联结到其他视图或表，甚 至能添加和更新数据

重要的是知道视图仅仅是用来查看存储在别处的数据的一种设施。 ==视图本身不包含数据==，因此它们返回的数据是从其他表中检索出来的。 在添加或更改这些表中的数据时，视图将返回改变过的数据

## 视图的限制

下面是关于视图创建和使用的一些最常见的规则和限制。 
 与表一样，视图必须唯一命名（不能给视图取与别的视图或表相 同的名字）。 
 对于可以创建的视图数目没有限制。 
 为了创建视图，必须具有足够的访问权限。这些限制通常由数据 库管理人员授予。 
 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造 一个视图。 
 ORDER BY可以用在视图中，但如果从该视图检索数据SELECT中也 含有ORDER BY，那么该视图中的ORDER BY将被覆盖。 
 视图不能索引，也不能有关联的触发器或默认值。 
 视图可以和表一起使用。例如，编写一条联结表和视图的SELECT 语句

## 使用视图

在理解什么是视图（以及管理它们的规则及约束）后，我们来看一 下视图的创建。 
 视图用CREATE VIEW语句来创建。 
 使用SHOW CREATE VIEW viewname；来查看创建视图的语句。 
 用DROP删除视图，其语法为DROP VIEW viewname;。 
 更新视图时，可以先用DROP再用CREATE，也可以直接用CREATE OR  REPLACE VIEW。如果要更新的视图不存在，则第2条更新语句会创 建一个视图；如果要更新的视图存在，则第2条更新语句会替换原 有视图。

```sql
SELECT cust_name,cust_contact
FROM productcustomers #这个表并不存在与数据库中
WHERE prod_id = 'TNT1'
```

视图极大地简化了复杂SQL语句的使用。利用视图，==可一 次性编写基础的SQL，然后根据需要多次使用==

## 用视图修改输出格式

```
SELECT Concat(cust_name,'(',cust_contact,')')
FROM productcustomers 
WHERE prod_id = 'TNT1'
```

## 更新视图

通常，视图是可更新的（即，可以对它们使用INSERT、UPDATE和 DELETE）。更新一个视图将更新其基表（可以回忆一下，视图本身没有数 据）。如果你对视图增加或删除行，==实际上是对其基表增加或删除行。==

基本上可以说，如果MySQL不 能正确地确定被更新的基数据，则不允许更新（包括插入和删除）

# 第二十三章 使用存储过程

存储过程就是别的语言里面的==函数==

## 存储过程的执行

```SQL
CALL productpricing(@pricelow,@pricehigh,@priceaverage)
#执行名为productpricing的存储过程，它计算并返回产品的最低、最高和平均价格。
```

CALL接受存储过程的名字以及需要传递给它的任意参数。

## 创建存储过程

==再次创建同名过程时，必须先删除（DROP）前一个！==

```sql
CREATE PROCEDURE productpricing()
BEGIN
	SELECT Avg(prod_price) AS priceaverage
	FROM products
END
```

在MySQL处理这段代码时，它创建一个新的存储过程productpricing。==没有返回数据==，因为这段代码并未调用存储过程，这里只是为 以后使用而创建它。

使用上述过程的方法是

```sql
CALL productpricing() #一定要带括号！
```

## 删除存储过程

```sql
DROP PROCEDURE productpricing #不用带括号！
```

### 仅当存在时删除

 如果指定的过程不存在，则DROP PROCEDURE 将产生一个错误。当过程存在想删除它时（如果过程不存在也 不产生错误）可使用DROP PROCEDURE IF EXISTS。

## 使用参数

```SQL
CREATE PROCEDURE productpricing(
    OUT p1 DECIMAL(8,2)， #关键字OUT指出相应的参数用来从存储过程传出一个值（返回给调用者）。
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
)
BEGIN
    SELECT Min(prod_price)INTO p1
    FROM products;
    SELECT Max(prod_price)INTO ph
    FROM products;
    SELECT Avg(prod_price)INTO pa
    FROM products;
END;
```

MySQL支持IN（传递给存储过程）、OUT（从存 储过程传出，如这里所用）和INOUT（对存储过程传入和传出）类型的参数。

### 参数的数据类型 

存储过程的参数允许的数据类型与表中使用 的数据类型==相同。==

## 使用变量

### 变量名

所有MySQL变量都必须以@开始

### 显示检索出的数据

```sql
CALL productpricing(@pricelow,@pricehigh,@priceaverage)
SELECT @pricelow,@pricehigh
```

### 参数输入

```SQL
CREATE PROCEDURE ordertotal (
    IN onumber INT, #参数输入
    OUT otota7 DECIMAL(8,2)
)
BEGIN
    SELECT Sum(item_price*quantity)
    FROM orderitems
    WHERE order_num = onumberINTO ototal;
END;
```

```
CALL odertotal(20005,@total)
```

## 智能存储过程

```sql
-- Name: ordertotal
-- Parameters : onumber = order number
-- taxable = o if not taxable，1 if taxableototal = order total variable
CREATE PROCEDURE ordertota1(
    IN onumber INT,
    IN taxab1e BOOLEAN ,
    OUT ototal DECIMAL(8,2)
)COMMENT '0btain order total，optional1y adding tax' #类似其他语言中的说明行
BEGIN
    -- Declare variable for total
    DECLARE tota1 DECIMAL(8,2);
    -- Declare tax percentage
    DECLARE taxrate INT DEFAULT 6;
    -- Get the order total
    SELECT Sum(item_price*quantity)FROM orderitems
    WHERE order_num = onumberINTO total;
    -- Is this taxable?
    IF taxable THEN
    -- Yes, so add tax rate to the total
    SELECT tota1+(tota1/100*taxrate) INTO total;
    END IF;
    -- And fina1ly，save to out variableSELECT tota1 INTO ototal;
END;
```

## 在一个存储过程中使用另一个存储过程

可以内嵌CALL，进而反复调用别的函数

# 第二十四章 使用游标

使用简单的SELECT语句，例如，没有办法得到第一行、下一行或前10行，也不存在每次一行 地处理所有行的简单方法（相对于成批地处理它们）

有时，需要在检索出来的行中前进或后退一行或多行。这就是使用 游标的原因。游标（cursor）是一个存储在MySQL服务器上的数据库查询， 它不是一条SELECT语句，而是被该语句检索出来的结果集。

游标主要用于交互式应用，其中用户需要滚动屏幕上的数据，并对数据进行浏览或做出更改。

## 游标的说明

 在能够使用游标前，必须声明（定义）它。这个过程实际上没有检索数据，它只是定义要使用的SELECT语句。
 一旦声明后，必须打开游标以供使用。这个过程用前面定义的 SELECT语句把数据实际检索出来。 
 对于填有数据的游标，根据需要取出（检索）各行。 
 在结束游标使用时，必须关闭游标。

## 创建游标

```SQL
CREATE PROCEDURE processorders()
BEGIN
    DECLARE ordernumbers CURSOR
    FOR
    SELECT order_num FROM orders;
END;
```

### 打开和关闭游标

```sql
OPEN ordernumbers;
CLOSE ordernumbers;
```

## 游标数据的使用

```sql
CREATE PROCEDURE processorders()
BEGIN
    DECLARE  o INT;
    DECLARE ordernumbers CURSOR
        FOR
        SELECT order_num FROM orders;
    OPEN ordernumbers;
    FETCH ordernumbers INTO o; #将游标读取的数据输入到o中，自动读取第一行
    CLOSE ordernumbers;
END;
```

每次调用FETCH都会向下一行==（类似迭代器）==

## 利用游标循环处理行

这个建议看课本，好多复杂的东西

P178

## CONTINUE HANDLER

```sql
DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;
```

这条语句定义了一个CONTINUE HANDLER，它是在条件出现时被执行 的代码。这里，它指出当SQLSTATE '02000'出现时，SET done=1。SQLSTATE  '02000'是一个未找到条件，==当REPEAT由于没有更多的行供循环而不能继续时，出现这个条件。==

# 第二十五章 使用触发器

触发器是MySQL响应以下任意语句而 自动执行的一条MySQL语句（或位于BEGIN和END语句之间的一组语 句）： 
 DELETE； 
 INSERT； 
 UPDATE。

## 创建触发器

创建时要给出四条信息

 唯一的触发器名； 
 触发器关联的表； 
 触发器应该响应的活动（DELETE、INSERT或UPDATE）； 
 触发器何时执行（处理之前或之后）。

```SQL
CREATE TRIGGER newproduct AFTER INSERT ON products
FOR EACH ROW SELECT 'Product added'
```

CREATE TRIGGER用来创建名为newproduct的新触发器。触发器 可在一个操作发生之前或之后执行，这里给出了AFTER INSERT， 所以此触发器将在INSERT语句成功执行后执行。这个触发器还指定FOR EACH ROW，因此代码对每个插入行执行。在这个例子中，文本Product added将对每个插入的行显示一次。

## 触发器仅支持表，每个表最多6个触发器

不支持在视图上使用，每个表最多支持6个触发器（每条INSERT、UPDATE 和DELETE的之前和之后）。

## 删除触发器

```SQL
DROP TRIGGER newproduct
```

## INSERT触发器

 在INSERT触发器代码内，==可引用一个名为NEW的虚拟表，访问被插入的行；==
 在BEFORE INSERT触发器中，NEW中的值也可以被更新（允许更改被插入的值）； 
 对于AUTO_INCREMENT列，NEW在INSERT执行之前包含0，在INSERT 执行之后包含新的自动生成值。

```sql
CREATE TRIGGER neworder AFTER INSERT ON orders
FOR EACH ROW SELECT NEW.order_num;
```

此代码创建一个名为neworder的触发器，它按照AFTER INSERT  ON orders执行。在插入一个新订单到orders表时，MySQL生 成一个新订单号并保存到order_num中。触发器从NEW. order_num取得这个值并返回它。

## DELETE触发器

 在DELETE触发器代码内，你==可以引用一个名为OLD的虚拟表，访问被删除的行==
 OLD中的值全都是只读的，不能更新。

```sql
CREATE TRIGGER deleteorder BEFORE DELETE ON ordersFOR EACH ROW
BEGIN
    INSERT INTO archive_orders(order_num，order_date，cust_id)
    VALUES(OLD.order_num，OLD.order_date，OLD.cust_id);
END;
```

在任意订单被删除前将执行此触发器。它使用一条INSERT语句==将OLD中的值（要被删除的订单）保存到一个名为archive_  orders的存档表中==（为实际使用这个例子，你需要用与orders相同的列创建一个名为archive_orders的表）

## UPDATE触发器

 在UPDATE触发器代码中，你可以==引用一个名为OLD的虚拟表==访问以前（UPDATE语句前）的值，引用一个名为NEW的虚拟表访问新 更新的值； 
 在BEFORE UPDATE触发器中，NEW中的值可能也被更新（允许更改 将要用于UPDATE语句中的值）； 
 OLD中的值全都是只读的，不能更新。

## 触发器的其他限制

 与其他DBMS相比，MySQL 5中支持的触发器相当初级。未来的 MySQL版本中有一些改进和增强触发器支持的计划。 
 创建触发器可能需要特殊的安全访问权限，但是，触发器的执行 是自动的。如果INSERT、UPDATE或DELETE语句能够执行，则相关 的触发器也能执行。 
 应该用触发器来保证数据的一致性（大小写、格式等）。在触发器 中执行这种类型的处理的优点是它总是进行这种处理，而且是透 明地进行，与客户机应用无关。 
 触发器的一种非常有意义的使用是创建审计跟踪。使用触发器， 把更改（如果需要，甚至还有之前和之后的状态）记录到另一个 表非常容易。 
 遗憾的是，MySQL触发器中不支持CALL语句。这表示不能从触发 器内调用存储过程。所需的存储过程代码需要复制到触发器内

# 第二十六章 管理事务处理

## 并非所有引擎都支持事务处理

MyISAM和InnoDB是两种最常使用 的引擎。前者不支持明确的事务处理管理，而后者支持。这 就是为什么本书中使用的样例表被创建来使用InnoDB而不是 更经常使用的MyISAM的原因。

## 事务处理（transaction processing）

可以用来维护数据库的完整性，它保证成批的MySQL操作要么完全执行，要么完全不执行。

 事务（transaction）指一组SQL语句； 
 回退（rollback）指撤销指定SQL语句的过程； 
 提交（commit）指将未存储的SQL语句结果写入数据库表； 
 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），你可以对它发布回退（与回退整个事务处理不同）

```sql
SELECT *FROM ordertotals; #检查该表不为空
START TRANSACTION; #事务的起点
DELETE FROM ordertotals;
SELECT *FROM ordertotals;#在事务中删除表，此时表为空
ROLLBACK;#回到START的上一行
SELECT *FROM ordertotals;#此时表不为空
```

### 可以回退的语句

事务处理用来管理INSERT、UPDATE和 DELETE语句。你不能回退SELECT语句。（这样做也没有什么意 义。）你==不能回退CREATE或DROP操作==。事务处理块中可以使用这两条语句，但如果你执行回退，它们不会被撤销

## 使用COMMIT

```sql
START TRANSACTION;
DELETE FROM orderitems WHERE order_num = 20010;
DELETE FROM orders WHERE order_num = 20010;#如果第一条DELETE起作用，但第二条失败，则DELETE不会提交（实际上，它是被自动撤销的）。
COMMIT;
```

一般的MySQL语句都是直接针对数据库表执行和编写的。这就是 所谓的隐含提交（implicit commit），即提交（写或保存）操作是自动 进行的。 

但是，==在事务处理块中，提交不会隐含地进行。为进行明确的提交， 使用COMMIT语句==

## 使用保留点

简单的ROLLBACK和COMMIT语句就可以写入或撤销整个事务处理。但 是，只是对简单的事务处理才能这样做，更复杂的事务处理可能需要==部分提交或回退。==

在上述的例子中，如果发生错误， 只需要返回到添加orders行之前即可，不需要回退到customers表（如果 存在的话）。

```SQL
SAVEPOINT DELETE1 #设置保留点
ROLLBACK DELETE1
```

# 第二十七章 全球化和本地化

MySQL处理不同字符集和语言的基础知识。有需要再看

# 第二十八章 安全管理

讲的是关于Mysql关于用户管理的知识

# 第二十九章 数据库维护

关于维护数据库的知识，检查表，备份表的一些方法

# 第三十章 改善性能

一些改善性能的技巧
