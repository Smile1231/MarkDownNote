# 关于``Exception``的处理细节

## 背景1

**万事先讲背景,在写需求时,需要有一个场景也是第一次遇到. 调用某第三方接口,除了返回正常值时,都是会报出异常,然后需要将异常在当前层处理一部分,然后再抛到外层处理剩余部分,可能描述有点抽象,给出一点代码**

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            divideNum(0);
        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("此处是除零异常(外部)");
        }
    }

    public static void divideNum(Integer i){
        try {
            Integer res  =  10 / i;
        } catch (Exception e) {
            if (e instanceof ArithmeticException) System.out.println("此处是除零异常(内部)");
            throw e;
        }
    }
}
```

结果截图:

![图 1](../images/10b0a866db6b648c4c7eea9ecbc9bc0dc0be86912d8c9eaab6db983f2426ea40.png)  

可以看出使用了 ``throw`` 依然可以将本层的异常抛出,的确也是平时很少会接触到的

## 背景2

> 关于异常处理,然后又遇到了另一个场景,一个方法报了某个异常,那么他是否还会继续往下走呢?

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            divideNum(0);
            System.out.println("这边是异常之后的方法11...");
        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("此处是异常(外部)");
        }

        System.out.println("这边是异常之后的方法22...");
    }
    public static void divideNum(Integer i){
        try {
            Integer res  =  10 / i;
        } catch (Exception e) {
            if (e instanceof ArithmeticException) System.out.println("此处是除零异常(内部)");
            throw e;
        }
    }
}
```
![图 2](../images/e148db92f80c910f6054064ba41029843a2b90323795e56a9582c1d65c392794.png)  

可观察到,```try``内部的代码将不会往下执行,但是外部的代码以及方法会继续执行,

> 那么,新的问题来了,如何也停止外部的方法执行呢,其实这个很简单

![图 3](../images/82e99f5cc4aba26c8b11bd1a065c1c13f488e59a8780e7c86e2a7a3a276c51a2.png)  

![图 4](../images/8a01c7417ba21a0a138f22d7612a19f8f250ea9ebfd8341fe84e5a573c277197.png)  

可以看到加了``return``关键字时,就直接将整个方法返回出去了


### 常用应用

> 在这次开发需求中,其实也发现了一些新的世界,分享一下

> 在异常处理时,我是在一个``for``循环内部进行异常的捕捉并处理

```java
public static void main(String[] args) {
        try {
            divideNumNoParam();
            System.out.println("这边是异常之后的方法11...");
        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("此处是异常(外部)");
            return;
        }
        System.out.println("这边是异常之后的方法22...");
}

public static void divideNumNoParam(){
        try {
            for (int i = -2; i < 2; i++) {
                System.out.println(10 / i);
            }
        } catch (Exception e) {
            System.out.println("除0错误");
        }
}
```

运行截图:

![图 5](../images/60a3f7f2d3dd1631c20336b21171c930d7a91baa5834ecb24e608473ac88ecb6.png)  

**可以看到,报异常之后,将会推出``for``循环体,但是跳出子程序之后,程序依然会往下走**

> 但是我们如果再内部中将异常丢出

![图 6](../images/3dc5b26284f2ac2035bd2c0a79623e441ed5bb6c2e06cef33aeff398b7ab45b0.png)  

结果截图:

![图 7](../images/ccb5740c38885c0fcc7a18c7bf8a8b8453a0a47b477ebf1b9af246d8393ca2d0.png)  

**外部捕捉到的异常之后将不会再继续运行下去**

#### 小结论
我们可以看出在``Java``程序中,如果当前程序报错之后,将会直接跳出,跳出当前子集程序,如果没有父类了,将会抛给虚拟机.


> 那问题来了,那我们在``for``循环过程中,如何继续运行``for``循环体呢?

#### 1. ``try``内部使用``for``

```java
public static void main(String[] args) {
        try {
            testException();
        } catch (Exception e) {
            System.out.println("外部错误");
            e.printStackTrace();
    }
}
public static void testException() {
    for (int i = -2; i < 2; i++) {
        try {
            System.out.println(10 / i);
        } catch (Exception e) {
            System.out.println("除0错误");
            //continue  可加可不加
        }
    }
}
```

结果截图:

![图 8](../images/122b75e09d43dddfd72c92130bcefb1e5e9def635cdadb58d68f40560e606c3e.png)  


如果将``try``放在了``for``循环内部,则捕捉之后将可以继续运行

#### 2. 使用``Stream``流

```java
public static void userStream(){
        ArrayList<Integer> list = new ArrayList<>();
        list.add(-2);
        list.add(-1);
        list.add(-0);
        list.add(1);
        AtomicInteger i = new AtomicInteger();
        list.stream()
                .forEach(m -> {
                    try {
                        System.out.println(10/m);
                    } catch (Exception e) {
                        System.out.println("报错了");
                        e.printStackTrace();
                    }
            });
}
```

结果截图:

![图 9](../images/b5b8ca4fb40e0935846002fc72b67e9a4c04808114d1f203a8228f74d53124dc.png)  


可以知道``Stream``流报了异常之后还会回继续运行循环体
