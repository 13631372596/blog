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

4. 不允许使用Executors去创建线程池，而是通过ThreadPoolExecutor的方式，threadPoolExecutor可以灵活定义很多参数。
    **Executors常用的四种线程池，可查看参考链接《Executor线程池的简单使用》**
    * FixedThreadPool：定长线程池，达到规模不再创建。允许请求队列长度为Integer.MAX_VALUE，容易OOM
    * SingleThreadPool：单线程线程池，保证顺序执行。允许请求队列长度为Integer.MAX_VALUE，容易OOM
    * CachedThreadPool：可缓存线程池，若无可回收线程则新建。允许创建线程数量为Integer.MAX_VALUE，容易OOM
    * ScheduledThreadPool：定长线程池，支持定时及周期性任务执行。

5. SimpleDateFormat线程不安全，一般不定义为static，定义为static时必须加锁。JDK8用Instant代替Date，LocalDateTime代替Calendar，DateTimeFormatter代替SimpleDateFormat。可查看参考链接《JDK8这样处理日期》

6. ThreadLocal必须回收
    ```
    objectThreadLocal.set(userInfo);
    try {
        // TODO
    } finally {
        objectThreadLocal.remove();
    }
    ```
7. 能不用锁就不用锁，能锁区块就不锁整个方法体，能用对象锁就不用类锁

8. 多线程加锁顺序需一致，否则可能死锁

9. 使用阻塞等待必须在try之外获取锁，在finally中解锁
    ```
    Lock lock = new XxxLock();
    // ...
    lock.lock();
    try {
        // TODO
    } finally {
        lock.unlock();
    }
    ```
    * 加锁方法与try之间没有可能抛出异常的方法调用
    * 防止unlock对未加锁对象解锁

10.  使用尝试机制获取锁时，必须判断当前线程是否持有锁
    ```
    Lock lock = new XxxLock();
    // ...
    boolean isLocked = lock.tryLock();
    if (isLocked) {
        try {
            // TODO
        } finally {
            lock.unlock();
        }
    }   
    ```
11. 并发修改同一记录时，加锁避免更新丢失。
    * 要么在应用层加锁，要么在缓存加锁，要么在数据库使用乐观锁version
    * 访问冲突率小于20%推荐用乐观锁，否则用悲观所。乐观锁重试次数不小于3次。可查看参考链接《SpringBoot Mybatis 乐观锁重试实现》

12. 多线程处理定时任务时，Timer运行的多个TimeTask之一没捕获异常时，其他任务自动终止。使用ScheduledExecutorService没有这个问题

13. 乐观锁获得锁时已完成更新操作，若处理不当容易造成系统压力或数据异常，所以资金相关的金融敏感信息使用悲观锁

14. 使用CountDownLatch进行异步操作时，线程退出前必须调用countDown，注意异常确保countDown能执行，避免主线程无法执行await直到超时才返回结果。try-catch捕获不到子线程抛出的异常。

15. 避免Random实例被多线程使用，虽为线程安全，但竞争同一seed(用于构造随机数生成器的参数)性能会下降。JDK7后直接使用ThreadLocalRandom，JDK7前需确保每个线程有一个Random实例

16. 并发场景下，通过双重检查锁实现延迟初始化。可查看参考链接《Java中的双重检查锁》

17. volatile可解决多线程内存不可见问题。一写多读可以解决变量同步问题，多写无法解决线程安全问题。可查看参考链接《volatile你必须了解一下》

18. HashMap在容量不够进行resize时由于高并发可能出现死链，可使用其他数据结构或加锁规避该风险。可查看参考链接《HashMap导致的死链以及数据丢失问题》

19. ThreadLocal对象使用static修饰。ThreadLocal解决共享变量问题，而无法解决共享对象问题。可查看参考链接《ThreadLocal原理解析与注意事项》

## 控制语句
1. switch方法中，每个case要么通过continue/break/return终止，要么注释说明到哪个case为止。必须包含default且放最后，即使没有代码。break为退出switch语句，return退出方法体。

2. 当switch参数类型为String时，必须判断null

3. if/else/for/while/do必须用大括号，避免单行方式。

4. 高并发场景，使用大于或小于来判断，避免等于判断被击穿

5. 表达异常时少用if-else，可改成：
    ```
    if (condition) {
        return obj;
    }
    // TODO
    ```
    if-else请勿超过3层，超过3层可用卫语句（有限考虑失败或异常）、策略模式、状态模式来实现

6. 除getXxx/isXxx等常用方法外，不要在条件判断中执行复杂语句。复杂逻辑结果赋值给一个有意义的布尔变量

7. 不要在条件等表达式中插入赋值语句

8. 循环体要考虑性能，定义对象、变量、获取数据库、不必要的try-catch尽量移至循环体外

9. 避免采用取反逻辑运算符

10. 接口入参保护，如批量操作时限定数量、查询时限定时间或条数

11. 下列情形的方法做参数校验：
    * 调用频次低
    * 执行时间开销大（参数校验时间可忽略不计）
    * 需要极高稳定性和可用性
    * 对外提供的开放接口
    * 敏感权限入口

12. 下列情形的方法不需要参数校验：
    * 极有可能被循环调用（方法必须注明外部参数检查要求）
    * 底层调用频度搞（参数错误不太可能到底层才暴露，如DAO可忽略参数校验）
    * 被声明为private，只会被自己代码所调用的方法（如调用方传入参数已做校验）

## 注释规约
1. 类、类属性、类方法的注释，必须用Javadoc规范"/**内容*/"

2. 抽象方法（包括接口方法）必须用Javadoc注释。说明返回值、参数、异常和方法实现的功能，还有子类实现的要求

3. 类必须添加创建者和创建日期

4. 方法内单行注释用"//"，多行注释用"/* */"，与代码对齐

5. 枚举类型必须有注释说明用途

6. "半吊子"英文不如中文，专有名词和关键字保持英文

7. 代码修改，注释也要修改，特别是参数、返回值、异常和核心逻辑等

8. 后续会恢复的代码注释时请详细说明，无用则直接删除

9. 注释要求，一是反映设计思想和代码逻辑，二是说明业务含义

10. 好的命名和代码结构是自解析的，如put(elephant, fridge)不需额外注释。注释力求精简准确，避免过多过滥 

11. 特殊注释标记，著名标记人、时间和预计处理时间
    * TODO：目前还未实现，需要实现。只能用于类、接口和方法（因为是Javadoc标签）
    * FIXME：错的代码，需要及时纠正

## 其它
1. 利用正则表达式的预编译功能，加快匹配速度。不要在方法体内定义Patter.compile(规则)

2. velocity（模板引擎）调用POJO属性时直接用属性名取值，自动调用getXxx（boolean基础数据类型为isXxx）

3. 后台输送页面变量必须加&!{var}，${var}会直接显示在页面

4. Math.random()返回的是double，获取整数类型随机数使用nextInt或nextLong

5. 获取毫秒用System.currentTimeMillis()，纳秒用System.nanoTime()，JDK8推荐用Instant

6. 日期格式化时，年份用yyyy，月份用大写M，分钟用小写m，24小时制用大写H，12小时制用小写h

7. 根据MVC理论，视图职责是显示，不要加入复杂逻辑

8. 任何数据结构的构造或初始化应指定大小，避免无限增长

9. 及时清理不用的代码或配置信息。在后续可能恢复使用的代码上方使用三个斜杠"///"说明注释理由

# 异常日志
## 异常处理
1. 可通过预检查规避的RuntimeException不应通过catch方式来处理，不如NPE、IndexOutOfBoundsException等

2. 不用异常来做流程、条件控制

3. catch请分清稳定（无论如何不回处理）和不稳定代码（尽可能区分异常类型），避免对大段代码try-catch使程序无法根据异常做处理和定位问题

4. 捕获异常是为了处理，如果不处理，请将异常抛给调用者。最外层业务必须处理异常，转为为用户可理解内容

5. try放到事务中时，catch异常后，如要回滚事务请手动回滚

6. finally必须对资源、流对象进行关闭，有异常也要做try-catch
    **JDK7及以上可用try-catch-resources，如InputStream和BufferedReader都实现AutoCloseable接口，自动关闭资源**
    ```
    try (InputStream is = new FileInputStream("fileName");
             BufferedReader bf = new BufferedReader(new InputStreamReader(is))) {
        // TODO
    }
    ```

7. 不要在finally中使用return，否则try中的return并不马上返回且会被丢弃。

8. 捕获异常与抛异常必须完全匹配，或者是抛异常的父类

9. 调用RPC、二方包、或动态生成类的相关方法时，捕获异常使用Throwable，因为可能存在编译期正确但运行时抛出NoSuchMethodError的情况
    ![Java异常简单关系图](/img/Java异常简单关系图.png)

10. 方法返回值可为null，但必须注释说明什么情况会返回null。防止NPE是调用者的责任

11. 防止NPE，注意：
    * 返回类型为基础数据类型，但return包装数据类型
    * 数据库查询结果
    * 即使isNotEmpty集合的元素
    * 远程调用返回对象
    * Session中获取的数据
    * 级联调用obj.getA().getB()。使用JDK8的Optional防止NPE，如
    ```
    return user.map(u -> u.getUsername())
           .map(name -> name.toUpperCase())
           .orElse(null);
    ```
    避免
    ```
    User user = .....
    if (user != null) {
        String name = user.getUsername();
        if(name != null) {
            return name.toUpperCase();
        } else {
            return null;
        }
    } else {
        return null;
    } 
    ```

12. 区分unchecked和checked异常，避免抛出RuntimeException，更不允许抛出Exception或Throwable。应使用有业务含义的自定义异常，如业界定义的DAOException、ServiceException

13. 对公司外的http/api开放接口必须用"错误码"；应用内部推荐异常抛出；跨应用优先使用Result（封装isSuccess方法、错误码、错误简短信息）
    **RPC方法使用Result理由**
    * 抛异常返回，调用方没捕获则产生运行时错误
    * 加栈信息返回，频繁出错时数据序列化和传输性能损耗

14. DRY原则，避免出现重复代码，必要时抽取共性方法、抽象公共类甚至组件化。如一个类中有多个public方法进行数行相同参数校验

## 日志规约
1. 不直接使用日志系统（Log4j、Logback）中的API，应用日志框架SLF4J，利于日志处理方式统一

2. 日志文件至少保存15天，因为有异常以"周"为频次发生。网络运行状态、安全相关信息、系统检测、管理后台操作、用户敏感操作留存日志不少于6个月

3. 应用中扩展日志命名方式为：appName_logType_logName.log。logType为日志类型，如stats/monitor/access；logName为日志描述。推荐日志分类，如错误日志和业务日志分开存放。

4. 日志输出，字符床拼接使用占位符，占位符仅是替换动作，提升性能。如log.debug("id：{}", id)

5. trace/debug/info级别的日志，必须进行日志级别的开关判断。如debug(xxx参数)方法内第一行代码为"当isDisabled为真时直接返回true"，但xxx参数仍可能已进行字符串拼接。
    * 正例
    ```
    if (log.isDebugEnabled()) {
        log.debug("...");
    }
    ```
    * 日志级别排序：OFF level、FATAL、ERROR、WARN、INFO、DEBUG、ALL level，从小到大

6. 避免重复打印日志浪费磁盘空间，log4j.xml必须设置additivity=false。additivity表示子log是否继承父log的输出源，默认true，即子log会在父log的输出源中输出

7. 异常信息应包括两类信息：案发现场、异常堆栈，如log.error(参数或者对象toString() + "_" + e.getMessage(), e)，不处理时请通过throws往上抛

8. 生产环境禁止输出debug，有选择输出info，如使用warn记录刚上线的业务行为时需注意输出量并及时删除

9. 可以用warn记录用户输入错误参数的情况，如非必要不要用error（只记录系统异常或重要错误），避免频繁报警

10. 尽量用英文描述错误信息，如果英文描述不清楚用中文即可。国际化团队或海外由于字符集问题，必须使用全英

# 单元测试
1. 单元测试必须遵守AIR原则，具有Automatic（自动化）、Independent（独立性）、Repeatable（可重复）特点

2. 应为全自动执行，且非交互的。不准使用System.out来人工验证，必须使用assert

3. 单元测试用例间不能互相调用

4. 可重复执行。持续集成中，即有代码check in时单元测试会被执行。要求代码把SUT（被测系统System under test）依赖改成注入，如用spring的注入或Mock实现

5. 要求力度足够小，至多是类级别，一般为方法级别。单测不负责跨类或跨系统交互，那属于集成测试

6. 核心业务、核心应用、核心模块的增量代码确保单元测试通过

7. 单元测试代码必须写在如下工程目录src/test/java

8. 单测目标为：语句覆盖率达到70% ；核心模块的语句覆盖率和分支覆盖率达到100%

9. BCDE

# 安全规约

# MySQL数据库

# 工程结构
## 应用分层

## 二方库依赖

## 服务器

# 设计规约

# 代码检测

# 参考链接
1. [POJO等Java中的概念分别指什么 - 知乎话题](https://www.zhihu.com/question/39651928/answers/updated)
2. [JDK8接口新特性：默认方法](https://www.jianshu.com/p/42c329cbe9b7)
3. [mysql对应java中常用的字段](https://www.cnblogs.com/wenwenzuiniucha/p/11155110.html)
4. [细说 Java 的深拷贝和浅拷贝](https://www.cnblogs.com/plokmju/p/7357205.html)
5. [关于equals和hashCode](https://blog.csdn.net/CringKong/article/details/89429269)
6. [java中的CAS和原子类的实现（JDK1.8）](https://www.jianshu.com/p/a533cbb740c6)
7. [Executor线程池的简单使用](https://www.cnblogs.com/qlqwjy/p/9470414.html)
8. [JDK8这样处理日期](https://cloud.tencent.com/developer/article/1442102)
9. [SpringBoot Mybatis 乐观锁重试实现](https://blog.csdn.net/qq_32923745/article/details/88800060)
10. [Java中的双重检查锁](https://www.cnblogs.com/xz816111/p/8470048.html)
11. [volatile你必须了解一下](https://www.cnblogs.com/fengzheng/p/9070268.html)
12. [HashMap导致的死链以及数据丢失问题](https://www.jianshu.com/p/11c99bf29ad2)
13. [ThreadLocal原理解析与注意事项](https://www.jianshu.com/p/1268e47af4d1)