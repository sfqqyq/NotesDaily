### JDK8新特性

	- Lambda表达式
	- 集合之Stream流式操作
	- 接口的增强
	- 并行数组的排序
	- Optional中避免Null检查
	- 新时间和日期API
	- 可重复注解

### Lambda表达式

	使用匿名内部类存在的问题
	public Thread(Runnable target)
	匿名内部类做了哪些事情
	1.定义了一个没有名字的类
	2.这个类实现了Runnable接口
	3.创建了这个类的对象
	使用匿名内部类语法是很冗余的
	其实我们最关注的是run方法和里面要执行的代码
	Lambda表达式体现的是函数式编程思想，只需要将要执行的代码放到函数中(函数就是类中的方法)
	Lambda就是一个匿名函数，我们只需要将要执行的代码放到Lambda表达式中即可
	new Thread(new Runnable(){
	
		@Override
		public void run(){
			Sout("新线程执行代码啦");
		}
	
		}).start();
	体验Lamdba表达式
	new Thread(() -> {
		sout("Lambda表达式执行啦！");
		}).start();
	Lambda表达式的好处：可以简化匿名内部类，让代码更加精简
#### Lambda的标准格式

Lambda省去面向对象的条条框框，Lambda的标准格式格式由3个部分组成：

~~~txt
(参数类型 参数名称) -> {
	代码体;
}
~~~

**格式说明：**

- (参数类型 参数名称)：参数列表
- {代码体}：方法体
- `->` ：箭头，分隔参数列表和方法体

#### Lambda省略格式

在Lambda标准格式的基础上，使用省略写法的规则为：

1.小括号内参数的类型可以省略

2.如果小括号内**有且仅有一个参数**，则小括号可以省略

3.如果大括号内**有且仅有一个语句**，可以同时省略大括号、return关键字及语句分号

~~~java
(int a)->{
	return new Persion();
}
~~~

省略后：

~~~java
a -> new Persion();
~~~



#### Lambda的前提条件

1.方法的参数或局部变量类型必须为接口才能使用Lambda

2.接口中有且仅有一个抽象方法

只有一个抽象方法的接口称为函数式接口，我们就能使用Lambda

jdk1.8提供了**@FunctionalInterface**注解检测这个接口是不是只有一个抽象方法，出现两个抽象方法注解会报错

~~~java
@FunctionalInterface
interface Flyable{
	public abstract void eat();
}
~~~



#### Lamdba和匿名内部类对比

了解Lambda和匿名内部类在使用上的区别

1.所需的类型不一样

- 匿名内部类，需要的类型可以是类，抽象类，接口
- Lambda表达式，需要的类型必须是接口

2.抽象方法的数量不一样

- 匿名内部类所需的接口中抽象方法的数量随意
- Lambda表达式所需的接口只能有一个抽象方法

3.实现原理不一样

- 匿名内部类是在编译后形成class

- Lambda表达式是在程序运行的时候动态生成class

**总结**

当接口中只有一个抽象方法时，建议使用Lambda表达式，其他情况还是需要使用匿名内部类



### JDK8接口增强

JDK8以前的接口：

~~~java
interface 接口名{
	静态常量;
	抽象方法;
}
~~~

JDK8对接口的增强，接口还可以有**默认方法**和**静态方法**

JDK8的接口：

~~~java
interface 接口名{
    静态常量;
	抽象方法;
    默认方法;
    静态方法;
}
~~~

#### 接口默认方法

接口默认方法实现类不必重写，可以直接使用，实现类也可以根据需要重写。这样就方便接口的扩展。

##### 接口默认方法的定义格式

~~~java
interface 接口名{
	修饰符 default 放回值类型 方法名(){
		代码：
	}
}
~~~

##### 接口默认方法的使用

1.方式一：实现类直接调用接口默认方法

2.方式二：实现类重写接口默认方法

#### 接口静态方法

##### 接口静态方法定义格式

~~~java
interface 接口名{
	修饰符 static 返回值类型 方法名(){
		代码：
	}
}
~~~

##### 接口静态方法的使用

直接使用接口调用即可；接口名.静态方法名();

~~~java
interface AAA{
	public static void test01(){
		sout("AAA")
	}
}

Class BBB implements AAA{
	//接口静态方法不能重写
}
~~~

#### 接口默认方法和静态方法放的区别

1.默认方法通过实例调用，静态方法通过接口名调用。

2.默认方法可以被继承，实现类可以直接使用接口默认方法，也可以重写接口默认方法。

3.静态方法不能被继承，实现类不能重写接口静态方法，只能使用接口名调用。

#### 小结

接口中新增的两种方法：

默认方法和静态方法

如何选择呢？如果这个方法需要被实现类继承或重写，使用默认方法，如果接口中的方法不需要被继承就使用静态方法



#### 常用内置函数式接口

基本都是放在`java.util.function`包下

##### Supplier接口

`java.util.function,Supplier<T>`接口，意味着“供给”，对应的Lambda表达式需要“对外提供”一个符合泛型类型的对象数据。

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

使用Lambda表达式返回数组元素最大值

使用Supplier接口作为方法参数类型，通过Lambda表达式求出int数组中最大值。提示：接口的泛型请使用`java.lang/Integer`类

 

```java
public static void main(String[] args) {
        getMaxValue(()->{
            int[] arr={2,1,5,4,6};
            Arrays.sort(arr);
            return arr[arr.length-1];
        });
    }

    /**
     * 获取数组中最大值
     * @param supplier
     */
    public static void getMaxValue(Supplier<Integer> supplier){
        Integer max = supplier.get();
        System.out.println("max is :"+max);
    }
```



##### Consumer接口

这是一个消费性接口，其数据类型由泛型参数决定

~~~java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
~~~

**默认方法：andThen**

如果一个方法的参数和返回值全都是`Consumer`类型，那么就可以实现效果：消费一个数据的时候，首先做一个操作,然后再做一个操作，实现组合，而这个方法就是`Consumer`接口中的default方法`andThen`

~~~java
public static void main(String[] args) {
        System.out.println("main");
        getUpString((String str)->{
            System.out.println(str.toUpperCase());
        },(String str)->{
            System.out.println(str.toLowerCase());
        });
    }

    public static void getUpString(Consumer<String> c1,Consumer<String> c2){
        System.out.println("123");
        c1.andThen(c2).accept("hello");
        System.out.println("321");
    }
~~~

#### Function接口

`public interface Function<T, R> `接口用来根据一个类型的数据得到另一个类型的数据，前者称为前置条件，后者称为后置条件。有参数有返回值。

~~~java
@FunctionalInterface
public interface Function<T, R> {
    
    R apply(T t);
    
    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

}
~~~



~~~java
  public static void main(String[] args) {
        StringToInt((String str)->{
            return Integer.parseInt(str);
        },(Integer ite)->{
            return ite*10;
        });
    }
    public static void StringToInt(Function<String,Integer> 			f1,Function<Integer,Integer> f2){
        //将字符串转为数字，再讲数字乘以10
        Integer apply = f1.andThen(f2).apply("10");
        System.out.println("apply is :"+apply);
    }
~~~

#### Predicate接口

有时候我们需要对某些类型的数据进行判断，从而得到一个boolean值结果，这时可以使用 接口

~~~java
@FunctionalInterface
public interface Predicate<T> {

    boolean test(T t);

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }
}
~~~



~~~java
public static void main(String[] args) {
        isOutLenth((String name)->{
            return name.contains("M");
        },(String name)->{
            return name.contains("H");
        });
    }
    public static void isOutLenth(Predicate<String> p1,Predicate<String> p2){
        String name="HODW";
        boolean test = p1.and(p2).test(name);
        System.out.println(test);

        boolean test2 = p1.or(p2).test(name);
        System.out.println(test2);
    }
~~~



### Stream流介绍

#### 集合的处理数据的弊端

当我们需要对集合中的元素进行操作的时候，除了必需的添加、删除、获取外，最典型的就是集合遍历。

~~~java
  /**
     * 1、开头是A
     * 2、是3位
     * 3、打印
     * @param args
     */
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        Collections.addAll(list,"Abb","Acc","Bnn","Cxx","An");
        list.stream().filter((str)->{//过滤器一层一层过滤
            return str.startsWith("A");
        }).filter((str)->{
            return str.length()==3;
        }).forEach((item)->{
            System.out.println(item);
        });
    }
~~~



#### Stream流的功能

Stream API能让我们快速完成许多复杂的操作，如筛选、切片、映射、查找、去除重复、统计、匹配、和归约。

#### 小结 

- 首先我们了解集合操作数据的弊端，每次都需要循环遍历，还要创建新的集合，很麻烦

- Stream是流式思想，相当于工厂的流水线，对集合的数据进行加工处理



























