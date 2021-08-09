# ``equals``与 ``==``区别

``Java``中数据类型分两种：

1. 基本类型：``long,int,byte,float,double``
2. 对象类型：``Long,Integer,Byte,Float,Double``其它一切``java``提供的，或者你自己创建的类。

其中``Long``叫 ``long``的包装类。``Integer、Byte``和``Float``也类似，一般包装类的名字首写是数值名的大写开头。

> 什么是包装类？

在``java``中有时候的运算必须是两个类对象之间进行的，不允许对象与数字之间进行运算。所以需要有一个对象，这个对象把数字进行了一下包装，这样这个对象就可以和另一个对象进行运算了。

比如我们可以定义一个类：
```java
public class Long {  
    int i=0;  
    public Long (int i){  
        this.i=i;  
    }  
}  
```
这个``Long`` 就是一个包装类，它包装了一个整数值，然后可以在里面写一些运算符重载的方法使它支持某些运算。这个时候可以赋值：

``Long l = new Long(10);``

现在变量 ``l`` 就是一个对象，不是一个数字。 

``long``是原始数据类型,没有属性方法,只能进行数学运算，``Long``是``long``相对应的引用数据类型，它有方法和属性，一个没方法属性，一个有方法属性,这就是它们的区别。

## ``==``解读

对于基本类型和引用类型 ``==`` 的作用效果是不同的，如下所示：

- 基本类型：比较的是值是否相同；
- 引用类型：比较的是引用是否相同；

代码示例：
```java
String x = "string";
String y = "string";
String z = new String("string");
System.out.println(x==y); // true
System.out.println(x==z); // false
System.out.println(x.equals(y)); // true
System.out.println(x.equals(z)); // true
```

代码解读：因为 ``x`` 和 ``y`` 指向的是同一个引用，所以 ``== ``也是 ``true``，而 ``new String()``方法则重写开辟了内存空间，所以 ``== ``结果为 ``false``，而 ``equals`` 比较的一直是值，所以结果都为 ``true``。

## ``equals`` 解读

``equals`` 本质上就是 ``==``，只不过`` String`` 和 ``Integer`` 等重写了 ``equals`` 方法，把它变成了值比较。看下面的代码就明白了。

首先来看默认情况下 ``equals`` 比较一个有相同值的对象，代码如下：
```java
class Cat {
    public Cat(String name) {
        this.name = name;
    }

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

Cat c1 = new Cat("王磊");
Cat c2 = new Cat("王磊");
System.out.println(c1.equals(c2)); // false
```

输出结果出乎我们的意料，竟然是 ``false？``这是怎么回事，看了 ``equals ``源码就知道了，源码如下：
```java
public boolean equals(Object obj) {
		return (this == obj);
}
```

原来 ``equals`` 本质上就是 ==。
那问题来了，两个相同值的`` String ``对象，为什么返回的是`` true？``代码如下：
```java
String s1 = new String("老王");
String s2 = new String("老王");
System.out.println(s1.equals(s2)); // true
```
同样的，当我们进入 ``String`` 的 ``equals`` 方法，找到了答案，代码如下：

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```
原来是 ``String`` 重写了 ``Object`` 的 ``equals`` 方法，把引用比较改成了值比较。

## 总结

总体来说，== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而 ``equals`` 默认情况下是引用比较，只是很多类重写了 ``equals`` 方法，比如 ``String、Integer`` 等把它变成了值比较，所以一般情况下 ``equals`` 比较的是值是否相等。











