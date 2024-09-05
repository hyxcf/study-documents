# Stream流的用法

### 1.获取Stream流（四种类型）

##### 注意：Stream.of方法使用基本数据类型的话，获取的是地址值（具体看下面代码）

```java
    // 1，单列集合获取 Stream 流
    @Test
    public void test() {
        ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list, "a", "b", "c");
        // 获取到一条流水线，并把集合中的数据放到流水线上
        list.stream().forEach(System.out::println);
    }

    @Test
    // 2.双列集合无法直接使用stream流，需要转化
    public void test2() {
        HashMap<String, Integer> hashMap = new HashMap<>();
        hashMap.put("aaa", 111);
        hashMap.put("bbb", 222);
        hashMap.put("ccc", 333);
        hashMap.put("ddd", 444);
        // 获取stream流
        hashMap.entrySet().stream().forEach(System.out::println);
    }

    @Test
    // 3.数组使用stream流
    public void testArr() {
        int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        Arrays.stream(arr).forEach(System.out::println);
    }

    @Test
    // 4.一堆零散数据（数据类型一致）
    public void testData(){
        // 注意：
        // Stream 接口中静态方法 of 的细节
        // 方法的形参是一个可变参数，可以传递一堆零散的数据，也可以传递数组
        // 但是数组必须是引用数据类型的，如果传递基本数据类型，是会把整个数组当做一个元素，放到 Stream 当中
        Stream.of(1,2,3,4,5).forEach(System.out::println);
        int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        Stream.of(arr).forEach(System.out::println); // [I@4450d156
    }
```

### 2.Stream流的中间方法

##### 2.1 filter：过滤

##### 2.2 limit：获取前几个元素

##### 2.3 skip：跳过前几个元素

##### 2.4 distinct：元素去重，依赖（hashCode和equals方法）

##### 2.5 concat：合并a和b两个流为一个流

##### 2.6 map：转换流中的数据类型

**注意1:中间方法，返回新的Stream流，原来的Stream流只能使用一次，建议使用链式编程**

**注意2:修改Stream流中的数据，不会影响原来集合或者数组中的数据**



### 3.Stream流的终结方法

##### 3.1  forEach：遍历

##### 3.2  count：统计

##### 3.3 toArray：收集流中的数据，放到数组中

##### 3.4 collect ：收集流中的数据，放到集合中



##### Lambda表达式需要“函数式接口”的支持

函数式接口:接口中只有一个抽象方法的接口，称为函数式接口。可以使用注解 @FunctionalInterface修饰
可以检查是否是函数式接口

Java8内置的四大核心函数式接口

- Consumer<T> :消费型接口

  void accept(T t);

- Supplier<T>:供给型接口

  T get();

- Function<T,R> :函数型接口

  apply(T t);

- Predicate<T>:断言型接口

  boolean test(T t);











