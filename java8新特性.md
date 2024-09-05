##### 一、Lambda 表达式的基础语法: 

Java8中引入了一个新的操作符"->”该操作符称为箭头操作符或Lambda 操作符箭头操作符将
                     Lambda表达式拆分成两部分:
                            左侧:Lambda表达式的参数列表
                            右侧:Lambda表达式中所需执行的功能,即Lambda体

​    语法格式一：无参数，无返回值
​         () -> System.out.println("Hello world");

​    语法格式二：有一个参数，无返回值
​         (e) -> System.out.println(e);
​         有一个参数：小括号可省略不写： e -> System.out.println(e);

​    语法格式三：有两个参数，有返回值,并且 里面 有多条语句
​         Comparator<Integer> comparator = (x,y) ->{
​            System.out.println("函数式接口");
​            return Integer.compare(x,y);
​        };

​    语法格式四:若Lambda 体中只有一条语句，return和大括号都可以省略不写
​        Comparator<Integer> com = (x,y) -> Integer.compare(x,y);

​    语法格式五:Lambda 表达式的参数列表的数据类型可以省略不写，因为JVN编译器通过上下文推断出，数据类型，即“类型推断”
​         (Integer x, Integer y) -> Integer.compare(x, y);
左右遇一括号省
左侧推断类型省

##### 二、Lambda表达式需要“函数式接口”的支持

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

**三、Stream API (cheer up!!!)**

①Stream自己不会存储元素。

②Stream不会改变源对象。相反，他们会返回一个持有结果的新Stream。

③Stream操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

步骤：

1。创建Stream
	一个数据源（如:集合、数组），获取一个流
2.中间操作
	一个中间操作链，对数据源的数据进行处理
3.终止操作(终端操作)
	一个终止操作，执行中间操作链，并产生结果

**筛选与切片**

```
filter——接收Lambda ,从流中排除某些元素。
limit——截断流,使其元素不超过给定数量。
skip(n)一跳过元素，返回一个扔掉了前n个元素的流。若流中元素不足n 个，则返回一个空液。与limit(n)互补
distinct—筛选，通过流所生成元素的hashCode()和equals()去除重复元素
```

**映射**

```
map—接收Lambda ,将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
flatMap—接收一个函数作为参数，将值中的每个值都换成另一个流，然后把所有流连接成一个流
```

**排序**

```
sorted()—自然排序 (Comparable)
sorted(Comparator com)—定制排序 (Comparator)
```

**查找与匹配**

```
allMatch—检查是否匹配所有元素
anyMatch——检查是否至少匹配一个元素
noneMatch——检查是否没有匹配所有元素
findFirst——返回第一个元素
findAny——返回当前流中的任意元素
count——返回流中元素的总个数
max——返回流中最大值
min——返回流中最小值
```

**归约**

```
reduce(T identity，BinaryOperator) / reduce(BinaryOperator)—可以将流中元素反复结合起来，得到一个值。
```

**收集**

```
collect—将流转换为其他形式。接收一个Collector接口的实观，用于给Stream中元素做汇总的方法
```





















