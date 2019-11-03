---
title: 阿里云认证 - 编码规范（Java）
date: 2019-11-03 13:04:31
description: 遵守一个好的编码规范，不仅有助于团队协作，也有利于个人养成良好编码习惯。该文章为阿里官网《Java开发手册》v1.5.0版的简单解读，使大家更好理解并通过认证。
tags:
---
# 前言
遵守一个好的编码规范，不仅有助于团队协作，也有利于个人养成良好编码习惯。该文章为阿里官网《Java开发手册》v1.5.0版的简单解读，使大家更好理解并通过认证。

[阿里巴巴编码规范（Java）认证地址](https://edu.aliyun.com/certification/cldt02)

# 编程规范
## 命名风格
1. 代码中的命名不能以"_"、"$"开始或结束

2. 代码中的命名需使用英文

3. 类名使用UpperCamelCase，POJO后缀除外

    **个人理解：**
    
    * POJO：附录写到"本手册POJO专指只有setter/getter/toString的简单类，包括DO/DTO/BO/VO"。
    * PO：持久对象，与数据库中表字段一一映射。
    * BO：业务对象，把数据封装为一个业务逻辑对象，可能含有多个PO。
    * VO：视图对象，封装业务数据给前端展示。
    * DTO：数据传输对象，封装服务间或前后端的传输数据。
    * DO：数据对象，一般跟PO相同，但某些情况：如乐观锁version字段不存在于DO，反之DO有某些字段在内存中而不需要持久化的
    ![POJO示例图](/img/POJO示例图.png)

4. 方法名、参数名、成员变量、局部变量使用lowerCamelCase

5. 常量命名全部大写，单词间用"_"隔开

6. 抽象类命名以Abstract或Base开头；异常类以Exception结尾；测试类以测试类名开始，Test结尾

7. 类型与中括号紧挨表示数组

    **数组初始化：**
    ```
    // 静态
    String[] strArr = new String[1];
    strArr[0] = "s1";
    // 动态
    String[] strArr2 = {"s1", "s2"};
    ```
8. POJO类中布尔类型变量不要加is前缀，否则RPC等框架解析时调用isXxx()方法获取不到Xxx属性。Mysql建表字段需命名为is_xxx，使用myBatis时在\<resultMap\>中定义is_xxx到xxx的映射

9. 包名统一使用小写及单数形式，点分隔符之间有且仅有一个自然语义的英文单词。类名如有复数含义则类名使用复数形式。

10. 避免子父类的成员变量间、同一方法体不同代码块的局部变量间采用相同命名。

11. 杜绝完全不规范缩写，如AbstractClass缩写为AbsClass

12. 使用尽量完整单词组合命名

13. 常量与变量命名时，表示类型的名词放词尾，如nameList

14. 命名时需体现具体模式，如OrderFactory

15. 接口类中的方法和属性不要加修饰符，需加上Javadoc注释。如要在接口定义变量，必须跟接口相关且为整个应用的基础常量。

    **接口缺省修饰符为：** `public abstract `

    **JDK8接口默认实现：**
    ```
    public interface Named {
        default String getName() {
            return "nathan, by default";
        }
    }

    public class NamedNobody implements Named {
    }

    public class Main {
        public static void main(String[] args) {
            Named named = new NamedNobody();
            System.out.println(named.getName()); // nathan, by default
        }
    }
    ```

16. 接口和实现类的命名有两套规则

    **对于Service和DAO，基于SOA概念：**
    XxxServiceImpl实现XxxService接口

    **形容能力的接口，通常为Xxxable：**
    XxxTranslator实现TranSlatable接口

17. 枚举类名带Enum后缀，成员名称需全大写，单词间"_"隔开

18. 各层命名规范

    **Service/DAO层：**
    * 获取单个对象：get作前缀
    * 获取多个对象方法：list作前缀
    * 获取统计：count作前缀
    * 插入：save/insrt作前缀
    * 删除：remove/delete作前缀
    * 修改：update作前缀

    **领域模型：**
    xxxDO、xxxDTO、xxxVO，禁止xxxPOJO

## 常量定义
1. 不允许魔法值

2. long或Long赋值时，数值后用大写L

3. 对常量类按功能归类，如缓存CacheConsts、系统配置ConfigConsts

4. 常量的复用层次有五层
    
    **跨应用共享** 
    放在二方库中，通常是client.jar的constant目录下

    **应用内共享**
    放在一方库中，通常是子模块的constant目录下

    **子工程内部共享** 
    当前子工程的constant目录下

    **包内共享**
    当前包单独的constant目录下

    **类内共享**
    直接在类定义private static final

5. 如果变量值仅在固定范围内变化用enum定义

## 代码格式
1. 若大括号内为空，写成"{}"即可;若非空
    * 左大括号前不换行，后换行
    * 右大括号前换行，后有代码不换行，没有则必须换行

2. 左右小括号和字符之间不需空格，左大括号前需空格

3. if/for/while/switch/do等保留字与括号间须加空格

4. 任何二目、三目运算符的左右都需加空格（不确定单目如++是否除外）

5. 采用4个空格缩进，禁止使用tab。如使用tab必须设置1个tab为4空格，IDEA设置为Use tab character；而在eclipse中insert spaces for tabs

6. 注释的双斜线与内容间有且仅有一个空格

7. 类强制转换时，右括号与转换值之间不需空格

8. 单行字符数不超过120个，超出需换行，换行时遵循
    * 第二行相对第一行缩进4空格，从第三行开始不再继续缩进
    * 运算符、点符号与下文一起换行
    * 多参数时在逗号后换行，括号前不换行
    ```
    method(args1, args2, ... ,
        argsX);
    ```

9. 多个参数逗号后需加空格

10. IDE的text file encoding使用UTF-8，文件的换行符使用Unix

11. 单个方法总行数不超80行，注释除外

12. 不要增加空格强行对齐上一行的等号

13. 不同逻辑的代码间用一个空行分割

## OOP规约
1. 直接用类名访问其静态变量或静态方法

2. 覆写方法必须加@Override

3. 不使用Object作为可变参数类型，可变参数必须放参数最后

4. 外部调用或二方库依赖的接口，不允许修改方法签名。接口过时需加@Deprecated，并说明新接口

5. 不使用过时的类或方法

6. 使用常量或确定有值的对象调用equals

7. 使用equals比较整型包装类对象的值

8. 浮点数之间的等值判断：
    **指定误差范围，若浮点数的差值在范围内，视为相等**
    ```
    float a = 1.0f - 0.9f
    float b = 0.9f - 0.8f
    float diff = 1e - 6f;
    if (Matb.abs(a - b) < diff) {
        System.out.println(true);
    }
    ```
    **用BigDecimal定义值，再进行浮点数的运算**
    ```
    BigDecimal a = new BigDecimal("1.0");
    BigDecimal b = new BigDecimal("0.9");
    BigDecimal c = new BigDecimal("0.8");
    BigDecimal x = a.substract(b);
    BigDecimal y = b.substract(c);
    if (x.equals(y)) {
        System.out.println(true);
    }
    ```
9. 定义数据对象DO时，属性类型与数据库字段类型匹配
    
    **常用类型**
    MySQL|Java
    :-:|:-:
    BIGINT|Long
    BLOB|byte[]
    DATE|Date
    DOUBLE|Double
    INTEGER|Integer
    TEXT|String
    VARCHAR|String

10. BigDecimal(double)精度丢失，须使用BigDecimal(String)或BigDecimal.valueOf(double)

11. POJO、RPC方法的返回值和参数必须使用包装类型，NPE问题由使用者保证，包装数据类型的null值能表示额外信息。局部变量使用基本数据类型。

12. POJO不要设定默认值

13. 序列化类新增属性时不要修改serialVersionUID，完全不兼容升级时请修改serialVersionUID。

14. 构造方法禁止加入业务逻辑，逻辑请放在init方法

15. POJO必须写toString

16. POJO禁止同时存在isXxx()和getXxx()

17. 使用String.split时，需检查最后分隔符后有无内容

18. 一个类有多个构造方法或多个同名方法时，应顺序放在一起

19. 类方法顺序依次为：公有或保护方法 > 私有 > getter/setter

20. setter方法中，参数名与类成员变量名一致，this.成员名 = 参数名。getter/setter中不要加入业务逻辑

21. 循环体的字符串拼接使用StringBuilder的append

22. 使用final声明
    * 不允许被继承的类，如String类
    * 不允许修改引用的域对象。引用地址不变，对象的值可变
    * 不允许被覆盖，POJO的setter
    * 不允许运行过程中重新赋值的局部变量
    * 避免上下文重复使用一个变量

23. Object的clone默认为浅拷贝，实现深拷贝需腹泻clone方法。
    **浅拷贝**
    对基本数据类型进行值传递，对引用数据类型进行引用传递
    
    **深拷贝** 
    对基本数据类型进行值传递，对引用数据类型创建新对象并复制内容

24. 类成员与方法访问控制从严，过宽访问范围不利于模块解耦
    * 访问控制修饰符
        修饰符 | 当前类 | 同一包 | 子孙类（同一包）| 子孙类（不同包）| 其他包
        :-|:-|:-|:-|:-|:-|
        public | Y | Y | Y | Y | Y
        protected | Y | Y | Y | Y/N | N
        default（缺省） | Y | Y | Y | N | N 
        private | Y | N | N | N | N 
  
        子类与基类不在同一包中时，子类实例可以访问其从基类继承而来的protected方法，而不能访问基类实例的protected方法

    * 工具类不允许有public或default构造方法
    * static成员变量考虑是否为final

## 集合处理
1. 覆写equals就必须覆写hashCode。Set存储的对象、Map键的自定义对象，必须覆写这两个方法。
    * 若两个对象执行equals()相等，那执行hashCode()也必须相等
    * 若两个对象执行equals()不等，那执行hashCode()可相等也可不等，因为hash码是int类型，溢出范围时可能会发生相等

2. ArrayList.subList(int fromIndex, int toIndex)返回的是内部类SubList，是ArrayList的一个视图，不能强转ArrayList，对子列表操作会反映到原列表。

3. 使用Map的keySet()/values()/entrySet()返回集合对象时，实则为所包含键或值的试图，映射的变化反映在集合，因此不可以添加元素。

4. Collections类返回的对象，如emptyList()、singletonList()等都为immutable list，不可以添加或删除元素。

5. 使用subList时注意原集合元素的增删，均会导致子列表出现ConcurrentModificationException异常

6. 集合转数组时，必须传入类型一致、长度为0的空数组。如list.toArray(new String[0])
    * 小于集合size时，重新创建size相同数组
    * 大于集合size时，存在NPE隐患
    * 等于集合size时，高并发时前两种情况均可能发生

7. 使用Collection.addAll()时，需进行NPE判断

8. 使用Arrays.asList时不能使用集合相关方法，因为用的适配器模式，后台数据仍是数组。
    **数组转集合并进行操作时个人建议使用**
    List&#60;Person&#62; levelList = new ArrayList&#60;Person&#62;(Arrays.asList("a", "b", "c"))

9. 泛型通配符&#60;? extend T&#62;不能用add方法，&#60;? super T&#62;不能用get方法
    * &#60;? extend T&#62;：参数未T或T的子类，用于频繁往外读取内容
    * &#60;? super T&#62;：参数为T类型父类，用于频繁插入内容

10. 无泛型限制集合赋值泛型限制集合，需进行instancof判断

11. foreach里不能使用remove/add，remove请使用Iterator。并发操作时需对Iterator对象加锁
    ```
    List<String> list = new ArrayList<>();
    list.add("1");
    list.add("2");
    Iterator<String> iterator = list.iterator();
    while (iterator.hasNext()) {
        String item = iterator.next();
        if (删除元素的条件) {
          iterator.remove();
        }
    }
    ```
    foreach底层为迭代器，但list.remove为不是迭代器的remove

12. JDK7及以上，Compartor实现类要满足3个条件
    * x，y 的比较结果和 y，x 的比较结果相反
    * x&#62;y，y&#62;z，则 x&#62;z
    * x=y，则 x，z 比较结果和 y，z 比较结果相同

13. JDK7及以上，使用diamond语法"&#60;&#62;"或全省略

14. 集合初始化时，指定初始值大小，无法确定时设置为16。resize需重建hash表，影响性能

15. 使用entrySet遍历Map，而不是keySet。JDK8使用Map.forEach

16. Map类K/V存储null值情况
 
    集合类 | Key | Value | 说明
    :-|:-:|:-:|:-
    Hashtable| N | N | 线程安全
    ConcurrentHashMap | N | N | 锁分段技术 (JDK8:CAS)
    TreeMap | N | Y | 线程不安全
    HashMap | Y | Y | 线程不安全

    JDK8:CAS请查看参考文献中《java中的CAS和原子类的实现》

17. 有序性（sort）指遍历结果按某种比较规则依次排序，稳定性（order）指集合每次遍历元素次序一定。
    集合类 | order | sort
    :-|:-:|:-:
    ArrayList | Y | N
    HashMap | N | N
    TreeSet | Y | Y

18. 使用Set唯一的特性进行去重，避免List.contains

## 并发处理
1. 获取单例对象需要保证线程安全，其中的方法也要保证线程安全。如资源驱动类、工具类、单例工厂

2. 线程或线程池需指定有意义的线程名称

3. 必须通过线程池获取线程资源

4. 不允许使用Executors去创建线程池，而是通过ThreadPoolExecutor的方式


# 异常日志

# 单元测试

# 安全规约

# MySQL数据库

# 工程结构

# 设计规约

# 代码检测

# 参考文献
1. [POJO等Java中的概念分别指什么 - 知乎话题](https://www.zhihu.com/question/39651928/answers/updated)
2. [JDK8接口新特性：默认方法](https://www.jianshu.com/p/42c329cbe9b7)
3. [mysql对应java中常用的字段](https://www.cnblogs.com/wenwenzuiniucha/p/11155110.html)
4. [细说 Java 的深拷贝和浅拷贝](https://www.cnblogs.com/plokmju/p/7357205.html)
5. [关于equals和hashCode](https://blog.csdn.net/CringKong/article/details/89429269)
6. [java中的CAS和原子类的实现（JDK1.8）](https://www.jianshu.com/p/a533cbb740c6)