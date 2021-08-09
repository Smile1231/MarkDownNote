# ``Java``中 ``String.format()``的使用


``JDK1.5``开始``String``类中提供了一个非常有用的方法``String.format(String format, Object ... args)``
查看源码得知其实是调用了``Java.util.Formatter.format(String, Object...)``方法


![1620305441320.png](./img/1620305441320.png)


> 首先来看一个例子：

```java
String s2 = String.format("%1$tY-%1$tm-%1$te", new Date());  
System.out.println(s2);  
```

查看``JDK``文档得知,``String.format``方法的第一个参数是有个公式可以套的

```java
%[argument_index$][flags][width][.precision]conversion
```

这里我们只要牢记这个公式就可以,下面说下每个参数所代表的含义：

- ``argument_index``：可选,是一个十进制整数，用于表明参数在参数列表中的位置。第一个参数由``1$``引用，第二个参数由``2$``引用，依此类推。
- ``flags``：可选,用来控制输出格式
- ``width``：可选,是一个正整数，表示输出的最小长度
- ``precision``：可选,用来限定输出字符数
- ``conversion``：必须,用来表示如何格式化参数的字符

先看一个简单的列子:
```java
System.out.println(String.format("我的名字叫%s", "小明")); 
// 打印:我的名字叫小明  
```
这里我们只用了``%s``这个简单的表达式,对比上面的公式,我们发现``[argument_index$][flags][width][.precision]``这些部分全部都省略掉了，只留下一个必须的``conversion``，在这里``conversion``就是``s``,百分号``%``是固定不变的

``[argument_index$]``省略之后它会自动把"小明"这个值填入到``%s``中去

我再稍微改下例子：

```java
String.format("我叫%s,她叫%s", "小明","小方"); // 我叫小明,她叫小方  `
```
这里会按顺序分别把小明,小方填入到对应的%s中. 如果我们要把小方填在前面,小明填在后面,那该怎么做呢,``[argument_index$]``就派上用场了
```java
String.format("我叫%2$s,她叫%1$s", "小明","小方"); // 我叫小方,她叫小明  `
```
依然是百分号``%``开头,中间多了个``2$，1$``

``conversion``可以填``s``,那还有什么其它字母可以填呢,当然有的：

- ``o``:结果被格式化为八进制整数
- ``x``:结果被格式化为十六进制
- ``d``:结果被格式化为十进制整数


```java
System.out.println(String.format("%o", 8)); // 10  
System.out.println(String.format("%x", 16)); // 10 
```
更多的``conversion``类别可以参考``JDK``文档``java.util.Formatter``类

至此,我们已经了解了``argument_index$``和``conversion``的用处,接下来我们了解``flag``和``width``的用法

- ``flag``是用来控制输出格式的,比如左对齐,金额用逗号隔开等
- ``width``表示最小宽度
先看个列子:
```java
String.format("%1$,d", 12302562); // 12,302,562  
```
这里多出一个逗号``","``,它就是``flag``,用于金额千分位隔开,当然写成``"%,d"``也是可以的

再一个列子:

```java
String.format("%1$08d", 123456);// 00123456
```

这里``0``就是``flag``,表示结果将用零来填充，``8``就是``width``，表示最少要``8``位，``d``是``conversion``

至于其它的``flag``可以查阅``JDK``文档。

接下来说下``[.precision]``

这个单词翻译下是精度的意思,我们发现了前面有个小数点.，因此不难联想到这个是关于浮点数类型的。
只有当传入的数据是浮点数时这个才有用,整数或者日期类型的数据都不能用。

比如我想要四舍五入保留两位小数,那么我可以这么写：

```java
String.format("%1$.2f", 12.12555);  //12.13  
```

这里``f``表示传入的数字是浮点型,如果传入的是整数,或者把``f``改成``d``都会抛出异常,``JDK``文档中有明确说明

对于浮点转换``'e'``、``'E'``和``'f'``，精度是小数点分隔符后的位数。如果转换是``'g'``或``'G'``，那么精度是舍入计算后所得数值的所有位数。如果转换是``'a'``或``'A'``，则不必指定精度。

对于字符、整数和日期/时间参数类型转换，以及百分比和行分隔符转换，精度是不适用的；如果提供精度，则会抛出异常。

到现在为止这套表达式公式已经基本讲完了,这套公式是针对于基本数据类型,和字符串的,如果是正对于时间类型的数据该怎么做呢,比如格式化日期

其实文档中已经给出说明了:

用来表示日期和时间类型的格式说明符的语法如下：

```java
%[argument_index$][flags][width]conversion
```

可选的 ``argument_index``、``flags`` 和 ``width`` 的定义同上。

所需的 ``conversion`` 是一个由两字符组成的序列。第一个字符是``'t'``或``'T'``。第二个字符表明所使用的格式。这些字符类似于但不完全等同于那些由 ``GNU date`` 和 ``POSIX strftime(3c)`` 定义的字符。

注意的是``conversion`` 是一个由两字符组成的序列。第一个字符是 ``t`` 或 ``T``。

也就是说用``conversion``的时候首先必要写一个``t``,然后再写其它``conversion``

时间类型有它自己的一套``conversion``,我们简单的选择几个来说:

conversion |	说明
----|---
Y	| 年份，被格式化为必要时带前导零的四位数（至少），例如，0092 等于格里高利历的92CE
m	| 月份，被格式化为必要时带前导零的两位数，即 01 - 13。
d |	日，一个月中的天数，被格式化为必要时带前导零两位数，即01 - 31。

上面三个分别表示年月日

如果我要显示年份,我就可以``%tY``,显示月份我就可以写``%tm``,记得一定要带上``t``

那么本篇一开始提到的那串复杂的表达式现在看来是不是很简单呢:

```java
String s2 = String.format("%1$tY-%1$tm-%1$te", new Date()); 
System.out.println(s2);    
```
``String.format()``方法差不多讲完了,仔细看``JDK``文档也会慢慢了解的.

需要批量进行格式化时,考虑下``DateFormat``, ``MessageFormat``, ``NumberFormat`` 把他们封装成一个静态工具类或许更好

毕竟调用``String.format()``方法是会``new``一个``Formatter``对象,虽然有``GC``帮忙,但是平时编程的时候还是要考虑这些因素的

尽量少的创建对象,节省资源。

## 常规类型的格式化

``String``类的``format()``方法用于创建格式化的字符串以及连接多个字符串对象。熟悉``C``语言的同学应该记得``C``语言的``sprintf()``方法，两者有类似之处。``format()``方法有两种重载形式。

``format(String format, Object... args)`` 新字符串使用本地语言环境，制定字符串格式和参数生成格式化的新字符串。

``format(Locale locale, String format, Object... args)`` 使用指定的语言环境，制定字符串格式和参数生成格式化的字符串。

显示不同转换符实现不同数据类型到字符串的转换，如图所示。

转换符 |	说明 |	示例
----|---|---
%s |	字符串类型 |	"mingrisoft"
%c |	字符类型 |	'm'
%b |	布尔类型 |	true
%d |	整数类型（十进制） |	99
%x |	整数类型（十六进制） |	FF
%o |	整数类型（八进制）|	77
%f |	浮点类型 |	99.99
%a |	十六进制浮点类型 |	FF.35AE
%e |	指数类型 |	9.38e+5
%g |	通用浮点类型 （f和e类型中较短的）	
%h | 散列码	
%% |	百分比类型 |	％
%n |	换行符	
%tx	| 日期与时间类型（x代表不同的日期与时间转换符	

测试用例：

```java
public static void main(String[] args) {
    String str=null;
    str=String.format("Hi,%s", "王力");  
    System.out.println(str);  
    str=String.format("Hi,%s:%s.%s", "王南","王力","王张");
    System.out.println(str);
    System.out.printf("字母a的大写是：%c %n", 'A');
    System.out.printf("3>7的结果是：%b %n", 3>7);
    System.out.printf("100的一半是：%d %n", 100/2);
    System.out.printf("100的16进制数是：%x %n", 100);
    System.out.printf("100的8进制数是：%o %n", 100);
    System.out.printf("50元的书打8.5折扣是：%f 元%n", 50*0.85);
    System.out.printf("上面价格的16进制数是：%a %n", 50*0.85);
    System.out.printf("上面价格的指数表示：%e %n", 50*0.85);
    System.out.printf("上面价格的指数和浮点数结果的长度较短的是：%g %n", 50*0.85);
    System.out.printf("上面的折扣是%d%% %n", 85);
    System.out.printf("字母A的散列码是：%h %n", 'A');
}
```

输出结果

```java
Hi,王力
Hi,王南:王力.王张
字母a的大写是：A
3>7的结果是：false
100的一半是：50
100的16进制数是：64
100的8进制数是：144
50元的书打8.5折扣是：42.500000 元
上面价格的16进制数是：0x1.54p5
上面价格的指数表示：4.250000e+01
上面价格的指数和浮点数结果的长度较短的是：42.5000
上面的折扣是85%
字母A的散列码是：41
```

搭配转换符的标志，如图所示。

标志 |	说明 |	示例 |	结果
----|---|---|---
``+`` |	为正数或者负数添加符号 |	("%+d",15) |	+15
− |	左对齐 |	("%-5d",15) |	15
0 |	数字前面补0 |	("%04d", 99) |	0099
空格 |	在整数之前添加指定数量的空格 |	("% 4d", 99) |	99
, |	以“,”对数字分组 |	("%,f", 9999.99) |	9,999.990000
( |	使用括号包含负数 |	("%(f", -99.99) |	(99.990000)
``#`` |	如果是浮点数则包含小数点，如果是16进制或8进制则添加0x或0 |	("%#x", 99)("%#o", 99) |	0x63 0143
< |	格式化前一个转换符所描述的参数 | 	("%f和%<3.2f", 99.45) |	99.450000和99.45
$  |	 被格式化的参数索引 |	``("%1$d,%2$s", 99,"abc")`` |	99,abc

测试用例

```java
public static void main(String[] args) {
    String str=null;
    //$使用
    str=String.format("格式参数$的使用：%1$d,%2$s", 99,"abc");
    System.out.println(str);
    //+使用
    System.out.printf("显示正负数的符号：%+d与%d%n", 99,-99);
    //补O使用
    System.out.printf("最牛的编号是：%03d%n", 7);
    //空格使用
    System.out.printf("Tab键的效果是：% 8d%n", 7);
    //.使用
    System.out.printf("整数分组的效果是：%,d%n", 9989997);
    //空格和小数点后面个数
    System.out.printf("一本书的价格是：% 50.5f元%n", 49.8);
}
```
结果输出

```java
格式参数$的使用：99,abc
显示正负数的符号：+99与-99
最牛的编号是：007
Tab键的效果是：       7
整数分组的效果是：9,989,997
一本书的价格是： 49.80000元
```




## 日期和事件字符串格式化

在程序界面中经常需要显示时间和日期，但是其显示的 格式经常不尽人意，需要编写大量的代码经过各种算法才得到理想的日期与时间格式。字符串格式中还有``%tx``转换符没有详细介绍，它是专门用来格式化日期和时 间的。``%tx``转换符中的``x``代表另外的处理日期和时间格式的转换符，它们的组合能够将日期和时间格式化成多种格式。

常见日期和时间组合的格式，如图所示。

转换符 |	说明 |	示例
----|---|---
c	| 包括全部日期和时间信息 |	星期六 十月 27 14:21:20 CST 2007
F	 |“年-月-日”格式 |	2007-10-27
D	 |“月/日/年”格式 |	10/27/07
r	 |“HH:MM:SS PM”格式（12时制）|	02:25:51 下午
T	 |“HH:MM:SS”格式（24时制） |	14:28:16
R	 |“HH:MM”格式（24时制） |	14:28

测试用例

```java
public static void main(String[] args) {
    Date date=new Date();
    //c的使用
    System.out.printf("全部日期和时间信息：%tc%n",date);
    //f的使用
    System.out.printf("年-月-日格式：%tF%n",date);
    //d的使用
    System.out.printf("月/日/年格式：%tD%n",date);
    //r的使用
    System.out.printf("HH:MM:SS PM格式（12时制）：%tr%n",date);
    //t的使用
    System.out.printf("HH:MM:SS格式（24时制）：%tT%n",date);
    //R的使用
    System.out.printf("HH:MM格式（24时制）：%tR",date);
}
```
输出结果 :

```java
全部日期和时间信息：星期一 九月 10 10:43:36 CST 2012
年-月-日格式：2012-09-10
月/日/年格式：09/10/12
HH:MM:SS PM格式（12时制）：10:43:36 上午
HH:MM:SS格式（24时制）：10:43:36
HH:MM格式（24时制）：10:43
```

定义日期格式的转换符可以使日期通过指定的转换符生成新字符串。这些日期转换符如图所示。

```java
public static void main(String[] args) {
    Date date=new Date();
    //b的使用，月份简称
    String str=String.format(Locale.US,"英文月份简称：%tb",date);
    System.out.println(str);
    System.out.printf("本地月份简称：%tb%n",date);
    //B的使用，月份全称
    str=String.format(Locale.US,"英文月份全称：%tB",date);
    System.out.println(str);
    System.out.printf("本地月份全称：%tB%n",date);
    //a的使用，星期简称
    str=String.format(Locale.US,"英文星期的简称：%ta",date);
    System.out.println(str);
    //A的使用，星期全称
    System.out.printf("本地星期的简称：%tA%n",date);
    //C的使用，年前两位
    System.out.printf("年的前两位数字（不足两位前面补0）：%tC%n",date);
    //y的使用，年后两位
    System.out.printf("年的后两位数字（不足两位前面补0）：%ty%n",date);
    //j的使用，一年的天数
    System.out.printf("一年中的天数（即年的第几天）：%tj%n",date);
    //m的使用，月份
    System.out.printf("两位数字的月份（不足两位前面补0）：%tm%n",date);
    //d的使用，日（二位，不够补零）
    System.out.printf("两位数字的日（不足两位前面补0）：%td%n",date);
    //e的使用，日（一位不补零）
    System.out.printf("月份的日（前面不补0）：%te",date);
}
```

输出结果

```java
英文月份简称：Sep
本地月份简称：九月
英文月份全称：September
本地月份全称：九月
英文星期的简称：Mon
本地星期的简称：星期一
年的前两位数字（不足两位前面补0）：20
年的后两位数字（不足两位前面补0）：12
一年中的天数（即年的第几天）：254
两位数字的月份（不足两位前面补0）：09
两位数字的日（不足两位前面补0）：10
月份的日（前面不补0）：10
```
和日期格式转换符相比，时间格式的转换符要更多、更精确。它可以将时间格式化成时、分、秒甚至时毫秒等单位。格式化时间字符串的转换符如图所示。

转换符 |	说明 |	示例
----|---|---
H |	2位数字24时制的小时（不足2位前面补0）|	15
I |	2位数字12时制的小时（不足2位前面补0） |	03
k |	2位数字24时制的小时（前面不补0）|	15
l |	2位数字12时制的小时（前面不补0） |	3
M |	2位数字的分钟（不足2位前面补0） |	03
S |	2位数字的秒（不足2位前面补0） |	09
L |	3位数字的毫秒（不足3位前面补0）|	015
N |	9位数字的毫秒数（不足9位前面补0） |	562000000
p |	小写字母的上午或下午标记 |	中：下午英：pm
z |	相对于GMT的RFC822时区的偏移量 |	+0800
Z |	时区缩写字符串 |	CST
s |	1970-1-1 00:00:00 到现在所经过的秒数 |	1193468128
Q  | 1970-1-1 00:00:00 到现在所经过的毫秒数 |	1193468128984


测试代码

```java
public static void main(String[] args) {
    Date date = new Date();
    //H的使用
    System.out.printf("2位数字24时制的小时（不足2位前面补0）:%tH%n", date);
    //I的使用
    System.out.printf("2位数字12时制的小时（不足2位前面补0）:%tI%n", date);
    //k的使用
    System.out.printf("2位数字24时制的小时（前面不补0）:%tk%n", date);
    //l的使用
    System.out.printf("2位数字12时制的小时（前面不补0）:%tl%n", date);
    //M的使用
    System.out.printf("2位数字的分钟（不足2位前面补0）:%tM%n", date);
    //S的使用
    System.out.printf("2位数字的秒（不足2位前面补0）:%tS%n", date);
    //L的使用
    System.out.printf("3位数字的毫秒（不足3位前面补0）:%tL%n", date);
    //N的使用
    System.out.printf("9位数字的毫秒数（不足9位前面补0）:%tN%n", date);
    //p的使用
    String str = String.format(Locale.US, "小写字母的上午或下午标记(英)：%tp", date);
    System.out.println(str);
    System.out.printf("小写字母的上午或下午标记（中）：%tp%n", date);
    //z的使用
    System.out.printf("相对于GMT的RFC822时区的偏移量:%tz%n", date);
    //Z的使用
    System.out.printf("时区缩写字符串:%tZ%n", date);
    //s的使用
    System.out.printf("1970-1-1 00:00:00 到现在所经过的秒数：%ts%n", date);
    //Q的使用
    System.out.printf("1970-1-1 00:00:00 到现在所经过的毫秒数：%tQ%n", date);
}
```

输出结果

```java
2位数字24时制的小时（不足2位前面补0）:11
2位数字12时制的小时（不足2位前面补0）:11
2位数字24时制的小时（前面不补0）:11
2位数字12时制的小时（前面不补0）:11
2位数字的分钟（不足2位前面补0）:03
2位数字的秒（不足2位前面补0）:52
3位数字的毫秒（不足3位前面补0）:773
9位数字的毫秒数（不足9位前面补0）:773000000
小写字母的上午或下午标记(英)：am
小写字母的上午或下午标记（中）：上午
相对于GMT的RFC822时区的偏移量:+0800
时区缩写字符串:CST
1970-1-1 00:00:00 到现在所经过的秒数：1347246232
1970-1-1 00:00:00 到现在所经过的毫秒数：1347246232773
```






