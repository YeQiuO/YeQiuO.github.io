---
title: Java
date: 2024-04-01 12:00:00
categories: 
- Java开发
tags:
- Java基础
---

# 基础知识
数据类型

- 除八个基本类型，Java 的所有数据类型都是引用，Java 中没有指针的概念，所有数据传输都是传值（引用可以看作是地址值，所有引用值都占四个字节）
- 基本数据类型包装类 Byte Short Integer Long Character 有常量池
- Java 被编译为字节码格式（.class），通过解释器对这些字节码进行解释执行

特性（封装、继承、多态）

- 支持类之间的单继承，但支持接口之间的多继承。
- 类可以实现多个接口，抽象类不能实例化但可以有构造方法，内部类只能通过外部类创建。
- 全面支持动态绑定，动态绑定是实现多态（一个接口，多种实现）的基础。 
   - 多态方式调用方法时，首先检查父类中是否有该方法，如果没有，则编译错误；如果有，再去调用子类的同名方法（A a=new B()，A是父类，B是子类）
   - 静态绑定：对象属性和 static private final 修饰的对象方法（构造函数），哪里声明，哪里使用
   - 动态绑定：运行时 JVM 实现绑定。若子类实现了这个方法，则调用子类的内存地址，若没有则调用当前对象（父类）的方法。只能调用父类的属性，虽然实际实现的是子类，如果父类没有这个属性，那就无法调用。如果要获取子类属性，就要重写子类方法获取该属性，前提是这个方法在父类中同样存在。
- Object 是超类，是所有类的父类（无父类的子类默认继承 Object。jdk6之前是编译器处理，jdk7之后是虚拟机处理）
- Java8 之后接口可以有默认方法，允许在接口中声明静态方法。

编译运行

- Java 编译器（Javac）是用 Java 实现的，用于将 .java 文件编译成字节码 .class
- Java 的运行环境（JVM）如HotSpot VM，实现了其跨平台的特性
# 特性
## 自动装箱/拆箱
> jdk1.5 i = Integer.valueOf(3) -128-127

自动装箱通常也不会通过显式的 valueOf 方法调用实现。编译器生成的字节码可能直接使用内部指令或方法来处理装箱和拆箱，而不是通过Java方法调用机制。因此，在 valueOf 方法上设置断点通常不会捕获到自动装箱的过程。
new String("abc") 和 String.valueOf("abc") 的区别：前者在堆中创建了新的对象，后者返回字符串常量池对象的引用
拆箱：xxxValue()；装箱：valueOf()
NPE 问题

- 数据库查询返回结果为 Null，无法拆箱
- 三目运算符可能出现问题：flag ? 0 : i（应使用 flag ? new Integer(0) : i）

```java
public void zhuangXiang() {
   Integer a1 = 128;
   Integer a2 = 128;
   Integer a3 = 256;
   Integer a4 = a1 + a2;
   System.out.println(a1 == a2); // false
   System.out.println(a4 == a3); // false
   System.out.println(a1 + a2 == a3); // true 发生拆箱
}
```

## 动态代理
静态代理：编译时完成
动态代理：运行时动态生成类字节码，并加载到 JVM 中

- JDK 动态代理：通过生成一个实现被代理类接口的对象来拦截被代理类的方法调用
   - Proxy.newProxyInstance()
   - InvocationHandler 接口 invoke 方法
- CGLIB 动态代理：通过生成一个被代理类的子类来拦截被代理类的方法调用
   - Enhancer 类
   - MethodInterceptor 接口 intercept 方法

为什么 JDK动态代理 要求被代理对象必须实现一个接口？

- 因为 JDK动态代理类已经继承了Proxy这个类，所以只能通过接口来与被代理类建立联系（两个类建立起联系，一是继承的关系【jdk已经不能通过这个方式了，因为java仅支持单继承】，另一种就是实现同一个接口【JDK动态代理选这种】），所以必须要求被代理类也得实现一个接口
## 动态绑定
> 动态绑定实现了 Java 的"多态"

静态绑定：编译器可以确定调用哪个方法（private、static、final或者构造器）
动态绑定：虚拟机将调用对象实际类型所限定的方法，前提是向上转型的类有同名同参数方法被覆盖了。属性调用的是向上转型的类的属性（即继承的类或实现的接口）
# 数据结构
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1688615730100-bda3f6f2-27ff-4c05-a20f-1a934075202a.png#averageHue=%23f0e7bf&clientId=u31335a52-0270-4&from=paste&height=331&id=mdLvB&originHeight=842&originWidth=953&originalType=binary&ratio=1&rotation=0&showTitle=false&size=166185&status=done&style=none&taskId=u16423f35-6268-483a-8e70-412f90a547a&title=&width=374.9927978515625)
数组
列表：ArrayList、LinkedList
集合：HashSet、TreeSet
映射：HashMap、TreeMap、LinkedHashMap、HashTable
队列：ArrayDeque、LinkedList
栈 ：LinkedList
堆：PriorityQueue（查找集合中的最值）
## 常用数据结构
StringBuilder（速度快）StringBuffer（线程安全）

- append(string)、insert(offset, string)
- delete(start, end)、deleteCharAt(index)
- setCharAt(index, char)、replace(start, end, str)
- charAt(index)

LinkedList

- 继承 AbstractSequentialList 类：“顺序访问”数据存储（如链表）
- 实现 List 接口
   - get、set
- 实现 Collection 接口
   - 判断是否存在：contains
- 实现 Queue 接口（通过 Deque 接口）
   - 报错：add、remove、element
   - 不报错：offer、poll、peek
- 实现 Deque 接口
   - addFirst [push]、addLast [add]：无返回，容量超出限制会报错
   - offerFirst、offerLast [offer]：返回 Boolean 表示是否加入成功
   - removeFirst [remove\pop]、removeLast：队列为空报错
   - pollFirst [poll]、pollLast：队列为空返回 null
   - getFirst [element]、getLast：队列为空报错
   - peekFirst [peek]、peekLast：队列为空返回 null
      - 【注意】队列头部为 null 和队列为空时，都会返回 null
   - removeFirstOccurrence [remove]、removeLastOccurrence
   - pop、push
- 实现 Cloneable 接口
- 实现 Serializable 接口

LinkedHashMap、LinkedHashSet

- LinkedHashMap 在 HashMap 的基础上，采用双向链表将所有 entry 连接起来，可以按照插入顺序遍历 entry（accessOrder=false），也可以按照访问顺序遍历 entry（accessOrder=true）

TreeSet、TreeMap

- 采用红黑树存储 Key，可以有序迭代 Key 集合

HashSet、HashMap

- HashSet 在 HashMap 基础之上做了一层封装

HashTable：历史遗留类。继承自Dictionary，支持同步
ArrayList、LinkedList（[为什么 ArrayList 中 elementData 为什么被 transient 修饰？](https://blog.csdn.net/zero__007/article/details/52166306)）

- LinkedList 每次增加的时候，会 new 一个对象；ArrayList 容量超出限制时，会扩容。数据量少时 LinkedList 增删快，数据量大时 ArrayList 增删快
## 原生工具类
Collections 集合操作类

- sort(list)
- reverse(list)
- binarySearch(list, target)

Arrays 数组操作类

- max(list)
- sort(array)
- copyOf(array, Length)
- binarySearch(array, value)
- fill(array, value)

比较器 集合排序工具

- Comparator 类级别
   - 针对不同的比较，单独定义一个比较类
   - int compare(T o1, T o2);  
      - 返回值为正数，交换 o1 o2 的顺序
      - 返回值为负数或零，不需要调整
      - 返回升序 o1-o2，降序 o2-o1
- Comparable 方法级别
   - 若一个类实现了Comparable接口，就意味着“该类支持排序”，可以用Arrays.sort()排序
   - public int compareTo(T o);
## 底层实现

- HashMap 中的红黑树 Node 相比 TreeMap 中的 Node 多了一个 prev 属性，可以顺序遍历红黑树中的树节点
- PriorityQueue（优先级队列，可以实现小根堆和大根堆）：底层通过数组来实现元素的存储；采用树形结构来描述元素的存储，思想类似于完全二叉树
- Object.clone() 是浅拷贝，不会复制属性的引用值![image.png](https://cdn.nlark.com/yuque/0/2024/png/35963383/1710914971159-15a5b84e-a6a6-40d8-b416-de53a479cc54.png#averageHue=%2366b2a6&clientId=u68ef795e-43e0-4&from=paste&height=167&id=YLdNH&originHeight=377&originWidth=1303&originalType=binary&ratio=1.2999999523162842&rotation=0&showTitle=false&size=85210&status=done&style=none&taskId=u5cdd571e-494b-445d-a805-dada0445771&title=&width=576.298095703125)

# 基础语法
多态、继承、封装、抽象、类、对象、实例、方法、重载
常量和静态变量：常量也是与类相关的，但它是用 final 关键字修饰的变量，一旦被赋值就不能再修改。与静态变量不同的是，常量在编译时就已经确定了它的值，而静态变量的值可以在运行时改变

- TODO 同步机制、原子类或 volatile 关键字

![image.png](https://cdn.nlark.com/yuque/0/2024/png/35963383/1707223814751-5acf6444-218e-4119-8959-cfb01ebdd1b9.png#averageHue=%23e4c49c&clientId=u3cace4ab-545a-4&from=paste&height=244&id=jzyOM&originHeight=397&originWidth=1222&originalType=binary&ratio=1.625&rotation=0&showTitle=false&size=129464&status=done&style=none&taskId=u905bd199-28f4-47b1-ac7f-25862d8460b&title=&width=752)

- Interface [接口特性](https://blog.csdn.net/sun_shine56/article/details/86621481)
   - 接口中的所有成员变量都默认是由public static final修饰的
   - 接口中的所有方法都默认是由public abstract修饰的
   - 接口没有构造方法
   - 实现接口的类中必须提供接口中所有方法的具体实现内容
   - 多个无关的类可以实现同一个接口
   - 一个类可以实现多个无关的接口
   - 与继承关系类似，接口与实现类之间存在多态性
   - 接口可以继承多个接口，使用extends关键字
   - Java8 后，接口可以拥有普通方法，实现类不需要重写，可以被实现类继承

接口和抽象类的关系

- 共同点
   - 都不能被实例化
   - 都可以包含抽象方法
   - 都可以有[默认实现](https://liushiming.cn/article/java-default-methods.html)的方法
- 区别
   - 接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是从属关系。
   - 一个类只能继承一个类，但是可以实现多个接口。
   - 接口中的成员变量只能是 public static final 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。
## 关键字
| 关键字 | 说明 |
| --- | --- |
| final | 常量 |
| static | 静态变量，无论一个类实例化多少对象，它的静态变量只有一份拷贝；静态方法，不能使用类的非静态变量，类名直接调用，不需要进行实例化对象。 |
| native | 本地、原生方法（非 Java 实现） |
| strictfp | 严格浮点、精准浮点 |
| synchronized | 线程、同步 |
| transient | 修饰的成员属性变量不被序列化，仅存于调用者的内存中而不会写到磁盘里持久化，防止敏感信息泄露（与Serilizable接口一同使用）当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复，直接设置为默认值。static 不属于任何对象，因此永远不会被序列化。 |
| volatile | 变量在 CPU 的寄存器中是不确定的，必须从主存中读取。保证多线程环境下变量的可见性；禁止指令重排序。 |
| instanceof | 实例 |

访问权限：

|  | public | protected | default | private |
| --- | --- | --- | --- | --- |
| 同一类中 | ✔ | ✔ | ✔ | ✔ |
| 同一包中（子类与无关类） | ✔ | ✔ | ✔ |  |
| 不同包的子类 | ✔ | ✔ |  |  |
| 不同包中的无关类 | ✔ |  |  |  |

## 运算符
&（按位与）如果相对应位都是1，则结果为1，否则为0
|（按位或）如果相对应位都是 0，则结果为 0，否则为 1
^（异或）如果相对应位值相同，则结果为0，否则为1
<<（左移运算符）
>>（右移运算符）
~（取反运算符）
>>>（无符号右移运算符 || 按位右移补零操作符）
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35963383/1712375811158-33314084-a734-44f5-9cd3-707e11b7e50c.png#averageHue=%23fcfbfb&clientId=u53ef871b-1033-4&from=paste&height=676&id=ue2b604e4&originHeight=886&originWidth=1336&originalType=binary&ratio=1.309999942779541&rotation=0&showTitle=false&size=72729&status=done&style=none&taskId=ueac817d4-f475-497b-af74-f0a1d7906d0&title=&width=1019.8473727909426)
## 序列化
> 跨平台存储、网络传输

- 这个类实现了Serilizable接口，这个类的所有属性和方法都会自动序列化
- 不需要序列化的属性前添加关键字transient，序列化对象的时候，这个属性就不会序列化到指定的目的地中
   - 目的：保存在内存中而不持久化，防止敏感信息泄露
# 编程技巧
## 常识知识
值计算

- java 两个整数之间的除法总是向零截断，直接保留整数位

数据类型默认值

- int \ double：0
- boolean：false

常量缓存池
> 提高性能以及减少内存开销

- Integer：-128-127
- String：运行时常量池

初始化

- 成员变量（定义在类里方法外的变量）一定要进行初始化的，如果不显式的进行初始化，那么虚拟机会进行默认的初始化
   - 基本数据类型一般是给予默认值
   - 如果是引用类型，会初始化成 null
- 如果是局部变量，就不会有默认值了，所以在声明局部变量时要注意，可以不在声明时初始化，但在使用之前一定要进行初始化，否则会报编译错误
## 初始化赋值
```java
int[] array = {1,2,3};
int[] array = new int[]{1,2,3};
int[][] array = {{1,2},{2,3}};
```
```java
List<> list = Arrays.asList(1,2,3); // 返回的是Arrays的内部类无法增删
List<> list = new ArrayList<>(){{
    add(1);
    add(2);
    add(3);
}};
```
## 初始化小根堆
```java
PriorityQueue<int[]> queue = new PriorityQueue<>((pair1,pair2)->pair1[1]-pair2[1]); // 数组
PriorityQueue<ListNode> queue = new PriorityQueue<>((o1, o2) -> o1.val-o2.val); // 节点

queue.offer(data) // 节点入堆
data = queue.poll() // 头节点出堆
```
## List 转数组
```java
int[] array = list.stream().mapToInt(Integer::valueOf).toArray();
```
```java
// 流式
String[] array = list.stream().toArray(String[]::new);
// 串行
String[] array = list.toArray(new String[list.size()]); // 规定好数组大小，速度更快
String[] array = list.toArray(new String[0]);
```
## 数组转 List
```java
int[] arr = { 1, 2, 3, 4, 5 };
List<Integer> list = Arrays.stream(arr).boxed().collect(Collectors.toList());
```
```java
Integer[] integers = new Integer[]{3,8,20,7,11,25};
List<Integer> list = Arrays.asList(integers); 
// Collections.addAll(list, a);
```
## 原地交换
```java
ch[a] ^= ch[b];
ch[b] ^= ch[a];
ch[a] ^= ch[b];
```
## 泛型&占位符
泛型：T E K V
> 泛型的初衷就是为了能在编译器检查出类型安全问题，并通过编译错误提示程序员

- 泛型必须是引用类型，不能是基本类型
- 泛型通过类型擦除实现

占位符：?

TODO lambda stream 流式计算 详解
## 判断奇偶
```java
num & 1 == 0 则为奇数
```
## 十进制数转 char
```java
char t = Character.forDigit(i, 10);
```
## Collection 转 List的方法
```
Collection<T> collection = map.values();
List<T> list = new ArrayList<T>(collection);
```
