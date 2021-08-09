# Apache Commons Lang 3.10使用简介

## 1. 诞生背景 
由于标准的``Java``库无法提供用于操纵其核心类的足够方法。``Apache Commons Lang``提供了这些额外的方法工具。

``Lang``为``java.lang API``提供了许多帮助程序实用程序，特别是字符串操作方法，基本数值方法，对象反射，并发，创建和序列化以及系统属性。此外，它还包含对``java.util.Date``的基本增强，以及一系列专用于构建方法的实用程序，例如``hashCode``，``toString``和``equals``

##  2. 引入方案，``maven``引入方式

```xml
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.10</version>
</dependency>
```
## 3. 简单介绍

```properties
org.apache.commons.lang3 #提供高度可重用的静态实用程序方法，主要用于为java.lang类增加值。
org.apache.commons.lang3.arch #提供类以使用os.arch系统属性的值。
org.apache.commons.lang3.builder #帮助创建一致的equals（Object）、toString（）、hashCode（）和compareTo（Object）方法。
org.apache.commons.lang3.compare #提供类以使用可比较接口和比较接口。
org.apache.commons.lang3.concurrent #为多线程编程提供支持类。
org.apache.commons.lang3.event #提供一些有用的基于事件的实用程序。
org.apache.commons.lang3.exception #提供异常功能。
org.apache.commons.lang3.math #为商业数学类扩展java.math。
org.apache.commons.lang3.mutable #为基元值和对象提供类型化的可变包装器。
org.apache.commons.lang3.reflect #提供反射java.lang.reflect API的常见高级用法。
org.apache.commons.lang3.text #提供用于处理和操作文本的类，部分用作java.text的扩展。
org.apache.commons.lang3.text.translate #用于从一组较小的构造块创建文本转换例程的API。
org.apache.commons.lang3.time #提供处理日期和持续时间的类和方法。
org.apache.commons.lang3.tuple #元组类，从版本3.0中的对类开始。
```
## 4. 常用详情介绍
### **常用字符串（``StringUtils``）**

```java
//缩短到某长度,用...结尾.其实就是(substring(str, 0, max-3) + "...")
//public static String abbreviate(String str,int maxWidth)
StringUtils.abbreviate("abcdefg", 6);// ---"abc..."

//字符串结尾的后缀是否与你要结尾的后缀匹配，若不匹配则添加后缀
StringUtils.appendIfMissing("abc","xyz");//---"abcxyz"
StringUtils.appendIfMissingIgnoreCase("abcXYZ","xyz");//---"abcXYZ"

//首字母大小写转换
StringUtils.capitalize("cat");//---"Cat"
StringUtils.uncapitalize("Cat");//---"cat"

//字符串扩充至指定大小且居中（若扩充大小少于原字符大小则返回原字符，若扩充大小为 负数则为0计算 ）
StringUtils.center("abcd", 2);//--- "abcd"
StringUtils.center("ab", -1);//--- "ab"
StringUtils.center("ab", 4);//---" ab "
StringUtils.center("a", 4, "yz");//---"yayz"
StringUtils.center("abc", 7, "");//---"  abc  "

//去除字符串中的"\n", "\r", or "\r\n"
StringUtils.chomp("abc\r\n");//---"abc"

//判断一字符串是否包含另一字符串
StringUtils.contains("abc", "z");//---false
StringUtils.containsIgnoreCase("abc", "A");//---true

//统计一字符串在另一字符串中出现次数
StringUtils.countMatches("abba", "a");//---2

//删除字符串中的梭有空格
StringUtils.deleteWhitespace("   ab  c  ");//---"abc"

//比较两字符串，返回不同之处。确切的说是返回第二个参数中与第一个参数所不同的字符串
StringUtils.difference("abcde", "abxyz");//---"xyz"

//检查字符串结尾后缀是否匹配
StringUtils.endsWith("abcdef", "def");//---true
StringUtils.endsWithIgnoreCase("ABCDEF", "def");//---true
StringUtils.endsWithAny("abcxyz", new String[] {null, "xyz", "abc"});//---true

//检查起始字符串是否匹配
StringUtils.startsWith("abcdef", "abc");//---true
StringUtils.startsWithIgnoreCase("ABCDEF", "abc");//---true
StringUtils.startsWithAny("abcxyz", new String[] {null, "xyz", "abc"});//---true

//判断两字符串是否相同
StringUtils.equals("abc", "abc");//---true
StringUtils.equalsIgnoreCase("abc", "ABC");//---true

//比较字符串数组内的所有元素的字符序列，起始一致则返回一致的字符串，若无则返回""
StringUtils.getCommonPrefix(new String[] {"abcde", "abxyz"});//---"ab"

//正向查找字符在字符串中第一次出现的位置
StringUtils.indexOf("aabaabaa", "b");//---2
StringUtils.indexOf("aabaabaa", "b", 3);//---5(从角标3后查找)
StringUtils.ordinalIndexOf("aabaabaa", "a", 3);//---1(查找第n次出现的位置)

//反向查找字符串第一次出现的位置
StringUtils.lastIndexOf("aabaabaa", ‘b‘);//---5
StringUtils.lastIndexOf("aabaabaa", ‘b‘, 4);//---2
StringUtils.lastOrdinalIndexOf("aabaabaa", "ab", 2);//---1

//判断字符串大写、小写
StringUtils.isAllUpperCase("ABC");//---true
StringUtils.isAllLowerCase("abC");//---false

//判断是否为空(注：isBlank与isEmpty 区别)
StringUtils.isBlank(null);StringUtils.isBlank("");StringUtils.isBlank(" ");//---true
StringUtils.isNoneBlank(" ", "bar");//---false

StringUtils.isEmpty(null);StringUtils.isEmpty("");//---true
StringUtils.isEmpty(" ");//---false
StringUtils.isNoneEmpty(" ", "bar");//---true

//判断字符串数字
StringUtils.isNumeric("123");//---false
StringUtils.isNumeric("12 3");//---false (不识别运算符号、小数点、空格……)
StringUtils.isNumericSpace("12 3");//---true

//数组中加入分隔符号
//StringUtils.join([1, 2, 3], ‘;‘);//---"1;2;3"

//大小写转换
StringUtils.upperCase("aBc");//---"ABC"
StringUtils.lowerCase("aBc");//---"abc"
StringUtils.swapCase("The dog has a BONE");//---"tHE DOG HAS A bone"

//替换字符串内容……（replacePattern、replceOnce）
StringUtils.replace("aba", "a", "z");//---"zbz"
StringUtils.overlay("abcdef", "zz", 2, 4);//---"abzzef"(指定区域)
StringUtils.replaceEach("abcde", new String[]{"ab", "d"},
        new String[]{"w", "t"});//---"wcte"(多组指定替换ab->w，d->t)

//重复字符
StringUtils.repeat(‘e‘, 3);//---"eee"

//反转字符串
StringUtils.reverse("bat");//---"tab"

//删除某字符
StringUtils.remove("queued", ‘u‘);//---"qeed"

//分割字符串
StringUtils.split("a..b.c", ‘.‘);//---["a", "b", "c"]
StringUtils.split("ab:cd:ef", ":", 2);//---["ab", "cd:ef"]
StringUtils.splitByWholeSeparator("ab-!-cd-!-ef", "-!-", 2);//---["ab", "cd-!-ef"]
StringUtils.splitByWholeSeparatorPreserveAllTokens("ab::cd:ef", ":");//-["ab"," ","cd","ef"]

//去除首尾空格，类似trim……（stripStart、stripEnd、stripAll、stripAccents）
StringUtils.strip(" ab c ");//---"ab c"
StringUtils.stripToNull(null);//---null
StringUtils.stripToEmpty(null);//---""

//截取字符串
StringUtils.substring("abcd", 2);//---"cd"
StringUtils.substring("abcdef", 2, 4);//---"cd"

//left、right从左(右)开始截取n位字符
StringUtils.left("abc", 2);//---"ab"
StringUtils.right("abc", 2);//---"bc"
//从第n位开始截取m位字符       n  m
StringUtils.mid("abcdefg", 2, 4);//---"cdef"

StringUtils.substringBefore("abcba", "b");//---"a"
StringUtils.substringBeforeLast("abcba", "b");//---"abc"
StringUtils.substringAfter("abcba", "b");//---"cba"
StringUtils.substringAfterLast("abcba", "b");//---"a"

StringUtils.substringBetween("tagabctag", "tag");//---"abc"
StringUtils.substringBetween("yabczyabcz", "y", "z");//---"abc"
```
### **随机数生成类（``RandomStringUtils``）**

```java
//随机生成n位数数字
RandomStringUtils.randomNumeric(n);
//在指定字符串中生成长度为n的随机字符串
RandomStringUtils.random(n, "abcdefghijk");
//指定从字符或数字中生成随机字符串
System.out.println(RandomStringUtils.random(n, true, false));  
System.out.println(RandomStringUtils.random(n, false, true));
```
### **数字类``NumberUtils``**

```java
//从数组中选出最大值
NumberUtils.max(new int[] { 1, 2, 3, 4 });//---4
//判断字符串是否全是整数
NumberUtils.isDigits("153.4");//--false
//判断字符串是否是有效数字
NumberUtils.isNumber("0321.1");//---false
```

### **数组类 ``ArrayUtils``**

```java
//创建数组
String[] array = ArrayUtils.toArray("1", "2");
//判断两个数据是否相等，如果内容相同， 顺序相同 则返回 true
ArrayUtils.isEquals(arr1,arr2);
//判断数组中是否包含某一对象
ArrayUtils.contains(arr, "33");
//二维数组转换成MAP
Map map = ArrayUtils.toMap(new String[][] { 
        { "RED", "#FF0000" }, { "GREEN", "#00FF00" }, { "BLUE", "#0000FF" } });
```

### **日期类``DateUtils``**

```java
//日期加n天
DateUtils.addDays(new Date(), n);
//判断是否同一天
DateUtils.isSameDay(date1, date2);
//字符串时间转换为Date
DateUtils.parseDate(str, parsePatterns);

/**
* String转换成Date
* arg0 : 日期字符串 String
* arg1 : 特定的地理，政治和文化地区.可以传null
* arg3 : 日期格式.与arg0格式一致 String
* 该方法对日期和时间的解释是宽松的
* 宽松的解释日期（如 1996 年 2 月 42 日）将被视为等同于 1996 年 2 月 1 日后的第 41 天
* 如果是严格的解释，此类日期就会引发异常
*/
Date date1 = DateUtils.parseDate("20171012 14:30:12", Locale.TRADITIONAL_CHINESE, "yyyyMMdd hh:mm:ss");
Date date2 = DateUtils.parseDate("20171012 14:30:12", Locale.TRADITIONAL_CHINESE, "yyyyMMdd hh:mm:ss");

/**
* String转换成Date 严格的
* arg0 : 日期字符串 String
* arg1 : 特定的地理，政治和文化地区.可以传null
* arg3 : 日期格式.与arg0格式一致 String
* 该方法对日期和时间的解释是严格的
*/
Date date3 = DateUtils.parseDateStrictly("20171012", Locale.TRADITIONAL_CHINESE, "yyyyMMdd");
Date date4 = DateUtils.parseDateStrictly("20171012", Locale.TRADITIONAL_CHINESE, "yyyyMMdd");

/**
* 判断两个日期是否是同一天
* arg0 arg1 数据类型 : Date Calendar
* 比较arg0 arg1的
* ERA = 0 年代
* YEAR = 1 年
* DAY_OF_YEAR = 6 年中的第几天
*/
DateUtils.isSameDay(date3, date4);
System.out.println("isSameDay = " + DateUtils.isSameDay(date3, date4));

/**
* 判断两个日期是不是同一毫秒
* arg0 arg1 数据类型 : Date Calendar
* 自1970年1月1日00:00:00 GMT 的毫秒数是否相等
*/
DateUtils.isSameInstant(date1, date2);
System.out.println("isSameInstant = " + DateUtils.isSameInstant(date1, date2));

/**
* 判断是否是同一个本地时间
* arg0 arg1 数据类型 : Calendar
* 比较arg0 arg1的
* 数据类型
* ERA = 0 年代
* YEAR = 1 年
* DAY_OF_YEAR = 6 年中的第几天
* HOUR_OF_DAY = 11 天中的第几个小时
* MINUTE = 12 分钟
* SECOND = 13 秒
* MILLISECOND = 14 毫秒
*/
Calendar cal1 = Calendar.getInstance();
cal1.setTime(date1);
Calendar cal2 = Calendar.getInstance();
cal2.setTime(date2);
DateUtils.isSameLocalTime(cal1, cal2);
System.out.println("isSameLocalTime = " + DateUtils.isSameLocalTime(cal1, cal2));

/**
* 获取指定日期前后arg1年
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addYears(date1, 4);
System.out.println("addYears = " + sdf.format(date));

/**
* 获取指定日期前后arg1月
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addMonths(date1, 4);
System.out.println("addMonths = " + sdf.format(date));

/**
* 获取指定日期前后arg1周
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addWeeks(date1, 4);
System.out.println("addWeeks = " + sdf.format(date));

/**
* 获取指定日期前后arg1天
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addDays(date1, 4);
System.out.println("addDays = " + sdf.format(date));

/**
* 获取指定日期前后arg1小时
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addHours(date1, 4);
System.out.println("addHours = " + sdf.format(date));

/**
* 获取指定日期前后arg1分钟
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addMinutes(date1, 4);
System.out.println("addMinutes = " + sdf.format(date));

/**
* 获取指定日期前后arg1秒
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addSeconds(date1, 4);
System.out.println("addSeconds = " + sdf.format(date));

/**
* 获取指定日期前后arg1毫秒
* arg0 : 指定日期 Date类型
* arg1 : int型,正数向后天数,0当天,负数向前天数
*/
date = DateUtils.addMilliseconds(date1, 4);
System.out.println("addMilliseconds = " + sdf.format(date));

/**
* 指定日期年的值
* arg0 : 日期 Date类型
* arg1 : int型
*/
date = DateUtils.setYears(date1, 2008);
System.out.println("setYears = " + sdf.format(date));

/**
* 指定日期月的值
* arg0 : 日期 Date类型
* arg1 : int型 范围在 1-12
*/
date = DateUtils.setMonths(date1, 1);
System.out.println("setMonths = " + sdf.format(date));

/**
* 指定日期天的值
* arg0 : 日期 Date类型
* arg1 : int型 范围在 1-31(不同月份值略有不同)
*/
date = DateUtils.setDays(date1, 24);
System.out.println("setDays = " + sdf.format(date));

/**
* 指定日期小时的值
* arg0 : 日期 Date类型
* arg1 : int型 范围在1-23
*/
date = DateUtils.setHours(date1, 23);
System.out.println("setHours = " + sdf.format(date));

/**
* 指定日期分钟的值
* arg0 : 日期 Date类型
* arg1 : int型 范围在1-59
*/
date = DateUtils.setMinutes(date1, 56);
System.out.println("setMinutes = " + sdf.format(date));

/**
* 指定日期秒的值
* arg0 : 日期 Date类型
* arg1 : int型 范围在1-59
*/
date = DateUtils.setSeconds(date1, 14);
System.out.println("setMinutes = " + sdf.format(date));

/**
* 指定日期毫秒的值
* arg0 : 日期 Date类型
* arg1 : int型
*/
date = DateUtils.setMilliseconds(date1, 100);
System.out.println("setMinutes = " + sdf.format(date));

/**
* 相当于
* Calendar cal3 = Calendar.getInstance();
* cal3.setTime(date);
* 得到的cal
*/
Calendar cal3 = DateUtils.toCalendar(date1);

/**
* 获取时区
* timeZone 系统默认
* timeZone1 系统默认时区
* timeZone2 设置时区
*/
Calendar calendar = new GregorianCalendar();
TimeZone timeZone = calendar.getTimeZone();
TimeZone timeZone1 = TimeZone.getDefault();
TimeZone timeZone2 = TimeZone.getTimeZone("Europe/Copenhagen");

/**
* Date 转换成 Calendar 带时区
* arg0 : 日期 Date类型
* arg1 : 时区
*/
Calendar cal4 = DateUtils.toCalendar(date1, timeZone2);

long fragment = 0;

/**
* 获取指定日期中从指定位置起的毫秒数
* arg0 : 指定的日期 Date类型 或 Calendar类型
* arg1 : 指定从什么位置开始 int类型:建议使用 Calendar.YEAR Calendar.MONTH 等常量
*/
fragment = DateUtils.getFragmentInMilliseconds(date1, Calendar.MONDAY);
System.out.println("getFragmentInMilliseconds = " + fragment);

/**
* 获取指定日期中从指定位置起的秒数
* arg0 : 指定的日期 Date类型 或 Calendar类型
* arg1 : 指定从什么位置开始 int类型:建议使用 Calendar.YEAR Calendar.MONTH 等常量
*/
fragment = DateUtils.getFragmentInSeconds(date1, Calendar.MONDAY);
System.out.println("getFragmentInSeconds = " + fragment);

/**
* 获取指定日期中从指定位置起的分钟数
* arg0 : 指定的日期 Date类型 或 Calendar类型
* arg1 : 指定从什么位置开始 int类型:建议使用 Calendar.YEAR Calendar.MONTH 等常量
*/
fragment = DateUtils.getFragmentInMinutes(date1, Calendar.MONDAY);
System.out.println("getFragmentInMinutes = " + fragment);

/**
* 获取指定日期中从指定位置起的小时数
* arg0 : 指定的日期 Date类型 或 Calendar类型
* arg1 : 指定从什么位置开始 int类型:建议使用 Calendar.YEAR Calendar.MONTH 等常量
*/
fragment = DateUtils.getFragmentInHours(date1, Calendar.MONDAY);
System.out.println("getFragmentInHours = " + fragment);

/**
* 获取指定日期中从指定位置起的天数
* arg0 : 指定的日期 Date类型 或 Calendar类型
* arg1 : 指定从什么位置开始 int类型:建议使用 Calendar.YEAR Calendar.MONTH 等常量
*/
fragment = DateUtils.getFragmentInDays(date1, Calendar.MONDAY);
System.out.println("getFragmentInDays = " + fragment);

boolean isEquals = false;

/**
* 判断两个时间在指定的位置之上是否相等
* arg0 : 时间1 Date类型 或 Calendar类型
* arg1 : 时间2 Date类型 或 Calendar类型
* arg2 : 指定在位置上开始比较 int类型:建议使用 Calendar.YEAR Calendar.MONTH 等常量
*/
isEquals = DateUtils.truncatedEquals(date1, date2, Calendar.MONDAY);
System.out.println("truncatedEquals = " + isEquals);

int truncatedCompare = -1;

/**
* 比较arg0与arg1两个时间在指定的位置上的时间差值
* arg0 : 时间1 Date类型 或 Calendar类型
* arg1 : 时间2 Date类型 或 Calendar类型
* arg2 : 指定在位置上开始比较 int类型:建议使用 Calendar.YEAR Calendar.MONTH 等常量
*/
truncatedCompare = DateUtils.truncatedCompareTo(date1, date2, Calendar.MONDAY);
System.out.println("truncatedCompareTo = " + truncatedCompare);
```



