# ``Java8``内置函数式接口


## 函数式接口有什么特点?
函数式接口只有一个方法，可以用注解``@FunctionalInterface``表示，当加上这个注解之后就给这个接口加上了条件，一旦接口中出现多个方法就会出现问题。

## 核心内置函数有哪些？
函数式接口 |	参数类型 |	返回类型 |	用途
----|---|---|---
Consumer  |	T |	void |	对类型T参数操作，无返回结果，包含方法 void accept(T t)
Supplier |	无 |	T |	返回T类型参数，方法时 T get()
Function |	T |	R |	对类型T参数操作，返回R类型参数，包含方法 R apply（T t）
Predicate |	T |	boolean |	断言型接口，对类型T进行条件筛选操作，返回boolean，包含方法 boolean test（T t）


## 四大核心函数式接口

### 1. ``Cousumer<T> ``消费型接口

> ``void accept(T t)``;

```java
//Consumer<T> 消费型接口
public void shooping(double money, Consumer<Double> consumer){
    consumer.accept(money);
}
@Test
public void test(){
    shooping(100, money -> System.out.println("消费金额￥:"+money));
}
```

### 2. ``Supplier<T>`` 供给型接口

> ``T get()``;
```java
//Supplier<T> 供给型接口
public Integer random(Supplier<Integer> supplier){
    return supplier.get();
}

@Test
public void test1(){
    int number = random(() -> (int) (Math.random() * 100));
    System.out.println(number);
}
```

### 3. ``Function<T, R>`` 函数式接口

> ``R apply(T t)``;

```java
//Function<T, R> 函数式接口
public String str(String str, Function<String, String> function){
    return function.apply(str);
}
@Test
public void test2(){
    str("Hello World!", (str) -> str.toUpperCase());
}
```

### 4. ``Predicate<T>`` 断言型接口

> ``booolean test(T t)``;

```java
//Predicate<T> 断言型接口
public List<Integer> filter(List<Integer> list, Predicate<Integer> predicate) {
    List<Integer> list1 = new ArrayList<>();
    for (Integer number : list) {
        list1.add(number);
    }
    return list1;
}

@Test
public void test3() {
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6);
    List<Integer> filterList = filter(list, item -> item > 3);
    for (Integer number: filterList) {
        System.out.println(number);
    }
}
```








