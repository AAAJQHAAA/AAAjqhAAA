---
title: java中的位运算
date: 2023-11-09 15:03:28
category: 后端
tags:
  - java.util类
  - java国际化
---
# java中的位运算
- 原码：最高位为符号位，0-正数；1-负数；
- 反码：正数反码是本身，负数反码除符号全部按位取反；
- 补码：正数补码是本身，负数补码是反码+1；

- 按位运算：
    - &（与）：全1是1
    - |（或）：有1是1
    - ^（异或）：相异为1
    - ~（按位取反）

- java中变量都是以补码的形式保存的
- cpu计算器只有加法没有减法， 减法用正数和负数相加得到
- java中移位运算（int是4个字节32位）（移>32，移位n%32位）
```
<<(左移)：低位补0,相当于乘以2；（符号位不变）
>>(带符号右移)：正数高位补0，负数高位补1；
>>>（无符号右移）：高位补0，相当于除以2；（负数是补码移动）
```

计算机硬件有两种储存数据的方式：大端字节序和小端字节序
- 大端字节序：高位字节在前，低位字节在后
- 小端字节序：低位字节在前，高位字节在后
- 计算机处理字节序的时候，不知道什么是高位字节，什么是低位字节。它只知道按顺序读取字节，先读第一个字节，再读第二个字节。


# java集合
## Map
- `HashMap`(线程不安全)
    - 哈希映射
    - 数组+链表
        - 根据key的hash值%数组容量(capacity)，找到数组中的一个位置（一个位置可能有多个元素）
        - 每个位置存放链表
    - 无序，k/v可为null，key不可以重复
    - 链表（jdk1.8）
        - 链化：当链表长度小于等于6时，红黑树将转化为链表，（此时链表查询速度比红黑树快）
        - 树化：当链表长度大于8时，数组容量大于等于64时，将链表转化为红黑树（平衡二叉树），（此时红黑树查询速度比链表快）
    - 数组扩容：当元素size个数超过临界值时（临界值=容量capacity*负载因子loadFactor），扩容后的容量capacity是之前两倍，resize()会将元素重新put进新的数组中
        - 扩容会消耗性能，所以预设元素的个数能够有效的提高hashmap的性能
    - 常用方法：
        - 查询方法：`size()`、`isEmpty()`、`get(Object)`、`containsKey(Object)`
        - 修改方法：`put(k,v)`、`remove(Object)`
        - 批量操作：`keySet()`、`clear()`
    - `LinkedHashMap`(线程不安全)
        - 链式哈希映射
        - 和hashMap的区别：
            - 每个元素使用双向链表（记录上一个节点和下一个节点）
            - 记录头节点和尾节点
            - 有序
            - 构造方法可以设置是添加顺序（默认）、还是访问顺序
                - 添加顺序（默认）：添加谁，尾节点就指向谁
                - 访问顺序（构造方法设置）：获取谁，尾节点就指向谁，头节点指向它下一个
- ConcurrentHashMap(线程安全)
    - key、value都不能为null
- TreeMap(线程不安全)
    - 二叉树映射（红黑树）
        - 红黑树条件：每个节点非黑即红、根节点是黑色、叶节点是黑色、每个红节点下的子节点是黑色、任意节点到叶节点的每条路径上的黑色节点树相同
    - 无序
    - k不能为null（null不能比较）
    - key唯一
    - 常用方法：
        - 查询操作：`size()`、`isEmpty()`、`get(object)`、`containsKey(Object)`
        - 修改操作：`put(k,v)`、`remove(Object)`
        - 批量操作：`keySet()`、`clear()`、`descendingMap()`
## Collection
- 方法分类
    - 修改操作：`boolean add(E)`、`boolean remove(Object)`
    - 查询操作：`size()`、`isEmpty()`、`contains(Object)`、`iterator()`、`toArray()`、`toArray(T[])`
    - 批量操作：`containAll(Collection<?>)`、`addAll(Collection<?>)`、`boolean removeAll(Collection<?>)`、`boolean removeIf(Predicate<?>)`
        - `boolean retainAll(Collection<?>)`、`clear()`、`spliterator()`、`stream()`、`parallelStream()`
- List
    - `ArrayList`(线程不安全)
        - 数组存储数据
        - 可重复、可null
        - 查询快：索引查询
        - 增删慢：数组满了之后会扩容1.5倍，然后再拷贝数据到新数组
            - 无参构造初始容量10
        - 常用方法：
            - 查询方法：`get(int)`、`indexOf(Object)`、`iterator()`、`size()`、`isEmpty()`
            - 修改方法：`add(E)`、`set(int,E)`、`remove(int)`
            - 批量操作：`clear()`、`stream()`
    - CopyOnWriteArrayList(线程安全)
    - `LinkedList`(线程不安全)
        - 双向链表
        - 有序、可重复、可为null
        - 增删快：增加删除只需要调整前后节点
        - 查询慢：知道索引也只能从头部或者尾部遍历找到对应的目标
        - 常用方法：
            - 查询方法：`getFirst()`、`getLast()`、`size()`、`isEmpty()`
            - 修改操作：`add(E)`、`addFirst(E)`
            - 批量操作：`listIterator()`、`clear()`、`stream()`
- Set
    - `HashSet`(线程不安全)
        - 哈希集合（HashMap存储数据）
        - 无序、值唯一、值可为null
        - 常用方法：
            - 查询操作：`size()`、`isEmpty()`、`contains(Object)`
            - 修改操作：`add(E)`、`remove(Object)`
            - 批量操作：`iterator()`、`clear()`
        - `LinkedHashSet`(线程不安全)
            - 链式哈希集合（LinkedHashMap存储数据）
            - 和hashset区别：有序
    - `CopyOnWriteArraySet`(线程安全)
    - `TreeSet`(线程不安全)
        - 二叉树集合（TreeMap实现）
        - 无序、值不为null、值唯一
        - 常用方法：
            - 查询操作：`size()`、`isEmpty()`、`contains(Object)`
            - 修改操作：`add(E)`、`remove(Object)`
            - 批量操作：`iterator()`、`clear()`、`descendingIterator()`
# java面向对象编程
## 继承
- 使得子类对象（实例）具有父类的实例域和方法
- 关键字
    - `extends`，单一继承类，不支持多继承
    - `implements`，实现接口
    - `super`来实现对父类成员的访问
        - 不显式调用，父类无参构造方法会隐式调用
        - 父类有参构造方法需要super显式调用
    - `this`指向自己的引用
    - `final`用来修饰变量（包括类属性、对象属性、局部变量和形参）、方法（包括类方法和对象方法）和类
        - 修饰的类不能被继承
        - 修饰的方法不能被重写
- 重写(Override)：子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变
- 重载(Overload)：在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同
## 多态
- 多态就是同一个接口，使用不同的实例而执行不同操作
- 父类引用指向子类对象
- 实现方式
    - 继承+重写
    - 接口
## 封装
- 将抽象性函式接口的实现细节部分包装、隐藏起来的方法
- 例如：类中属性变量设置为`private`私有的，提供get/set方法访问和设置变量。
# java8新特性
## Lambda 表达式
- 使用条件：必须有接口（不能是抽象类），接口中有且仅有一个需要被重写的抽象方法
- 省略规则：
    - `()`中的`参数类型`可以省略
    - 如果`()`中只有`一个参数`，那么可以省略`()`
    - 如果`{}`中只有`一条语句`，那么可以省略`{}`、`return`、`;`
```text
(参数类型 参数名) -> {
    方法体;
    return 返回值;
}
```
## 方法引用`::`
## 新增了接口的默认方法`default`
## Stream
- Stream（流）是一个来自`数据源`的`元素队列`并支持`聚合操作`
    - 数据源`Collection`、`Arrays`
- 常用方法
    - 中间操作(返回流)
        - `limit()`获取指定数量元素的流
        - `sorted()`返回排序后的流
        - `filter()`过滤流，返回符合条件的流
        - `distinct()`去除重复元素
        - `map()`返回处理后的流
    - 终止操作
        - `forEach()`遍历流中元素
        - `count()`返回流中元素个数
        - `findFirst()`返回流中第一个元素
        - `toArray()`将流中的元素转换为数组
        - `collect(Collector<? super T, A, R> collector)`将流中的元素收集到另一个目标中
            - `List<String> result = stream.collect(Collections.tolist());`收集到List
            - `Set<String> result = stream.collect(Collections.toSet());`收集到Set
            - `TreeSet<String> result = stream.collect(Collectors.toCollection(Treeset::new));`收集到Treeset
            - `String result = stream.collect(Collectors.joining());`收集一个字符串
## Optional类
- 创建
    - `Optional.of(T)`不为空的Optional
    - `Optional.ofNullable(T)`可以为空的Optional
    - `Optional.empty()`值为null的Optional
- 方法
    - `isPresent()`对象持有的值是否不为null
    - `get()`获取内部持有的值，内部值为null时会抛出异常
    - `ifPresent(Consumer<? super T>)`在值存在的情况下,以值为参数,调用Consumer
    - `filter(Predicate<? super T>)`值不存在则直接返回this(empty)；值存在判断值是否满足Predicate，满足返回this，不满足返回empty
    - `map(Function<? super T, ? extends U>)`在内部值不为null的情况下, 通过传入的Function, 将内部值处理成新的值, 并构造成新的Optional对象返回.
    - `orElse(T)`如果值为null, 则返回该方法的参数值.
    - `orElseGet(Supplier<? extends T>)`如果内部值为空, 就返回supplier提供的值
    - `orElseThrow(Supplier<? extends X>)`如果为空就抛出supplier提供的异常
# 单例模式
- 懒汉式与饿汉式的根本区别在与是否在类内方法外创建自己的对象。

# 题目
## 1、如下代码1和2两处运行分别时什么结果？
```java
public class test1{
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");
        list.add("ddd");
        /*
         * remove(int index)
         * 每删除一个元素，数组都会发生一次复制，将后面元素复制到前面，
         * 通过索引删除，删除不全
         * */
        for (int i = 0; i < list.size(); i++) {
            list.remove(i);
        }
        
        for (String s : list) {
            System.out.println("====" + s);  // 1
        }
        /*
         * remove(Object o)
         * 遍历过程中修改，会抛异常
         * */
        for (String s : list) {
            list.remove(s);
        }

        for (String s : list) {
            System.out.println("====" + s);  // 2
        }

    }
}
```
## 2、设计模式中的设计原则是什么？
```text
1、开闭原则（Open Close Principle）
开闭原则的意思是：对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。
2、里氏代换原则（Liskov Substitution Principle）
里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。
3、依赖倒转原则（Dependence Inversion Principle）
这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。
4、接口隔离原则（Interface Segregation Principle）
这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。
5、迪米特法则，又称最少知道原则（Demeter Principle）
最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。
6、合成复用原则（Composite Reuse Principle）
合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。
```
## 3、数组的增和删一定比链表的效率低吗？原因是？
因为CPU缓存会读入一段连续的内存，顺序存储符合连续的内存，所以顺序存储可以被缓存处理，而链接存储并不是连续的，分散在堆中，所以只能内存去处理。
所以数组查询比链表要快。
而数组大小固定，插入和删除都需要移动元素，链表可以动态扩充，插入删除不需要移动元素，只需要更改元素中的指针。所以链表的插入删除比数组效率高。
## 4、hashMap、hashTable、concurrentHashMap的异同点？
hashMap：key/value都可为null、线程不安全
hashTable：key/value都不可为null、线程安全（锁整个数组）
concurrentHashMap：key/value都可为null、线程安全（分段锁）


# 基础知识
### Spring cloud 配置文件优先级
- `classpath:/bootstrap.yml`
- `./bootstrap.yml`
- `./config/bootstrap.yml`
- `classpath:/application.yml`
- `./application.yml`
- `./config/application.yml`
- `springconfig中心/config/application.yml`
- `springconfig中心/config/spring.application.name.yml`
- `springconfig中心/config/spring.application.name-dev.yml`
