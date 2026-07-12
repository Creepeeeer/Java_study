```java
javac ***.java #编译
java *** #运行
```



**final**

声明一个变量为常量



**布尔类型**

boolean



# 面向对象

## 类与对象

#### 类的定义与对象创建

定义一个类

public class 类名{}

创建一个对象

```c++
类名 对象名=new 类名();
```



#### 对象的使用

此时创建的对象就相当于生成的一个引用，假如

```c++
Person b=a;
```

a和b其实指向的是一块内存空间



#### 方法创建与使用

就是函数

但是比如一个对象当参数传入，其实传的是引用

**在函数里面要使用成员变量可以 this.成员变量名**



#### **方法的重载**

必须保证参数的类型或者变量改变，如果只是返回值不同是不能重载的



#### 构造方法

和构造函数一样，要声明为public



#### 静态变量和静态方法

此时变量和方法是类的属性，既可以通过对象名也可以通过类名访问

静态函数方法，无法获取成员变量的值和this关键字

在使用一个类的东西之前不会加载这个类，静态的东西在类被加载的时候分配

**静态代码块**：

```c++
static {}//常用于给静态变量初始化
```



## 包和访问控制

### 包的声明和导入

**包的格式**

```c++
名字1.名字2.名字3.....名字n
```

本质上就是建了多级文件夹

每一个文件都可以放在包下面

此时要在前面加上

```c++
package 包名;
```

**引用其他软件包下的类**

```c++
import 包名.类名
```

只有类不在同一个包下才需要导入，可以用包名.*表示导入这个包里的全部类



### 访问权限控制

可以给类，成员变量和成员方法指定访问权限

|           | 当前类             | 同一个包下的类     | 不同包下的子类     | 不同包下的类       |
| --------- | ------------------ | :----------------- | ------------------ | ------------------ |
| public    | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| protected | :white_check_mark: | :white_check_mark: | :white_check_mark: | :x:                |
| 默认      | :white_check_mark: | :white_check_mark: | :x:                | :x:                |
| private   | :white_check_mark: | :x:                | :x:                | :x:                |

创建的普通类不能是protected和private的，不然这个类不能被外部使用

**静态导入：**假如一个类有静态成员或者静态方法

可以

```java
import static 包名.类名.函数名/成员名
```

那么 在类里可以直接用这个函数/成员





## 封装，继承和多态

### 类的继承

```java
class 子类 extends 父类
```

一个子类只能由一个父类继承而来，不存在多继承

如果一个类前面加了final关键字，表示这个类不能被继承，就不能有子类

假如要在子类的构造函数里调用父类的构造函数

```java
在子类构造函数用super() 代指父类的构造函数，并且必须写在子类构造函数的第一行
```

可以用父类对象指向new出来的子类对象,但此时父类对象也只能使用父类的函数和变量，假如此时父类和子类由同名函数，那么调用的是子类的函数

```java
Person p=new Student();//person是student的父类
```

也可以使用强制类型转换，将一个被当作父类的子类对象转回子类

```java
Student s=(Student)p;
```

**判断变量所引用的对象是什么类，返回值为boolean**

```java
变量名 instanceof 类名
```

一个子类对象判断它为父类和子类都为true

父类和子类可以由相同名字的变量和函数，在子类里直接变量名访问的是子类的，要访问父类要加super.



### object类

所有类都是object类的子类，这是默认的

那么所有对象都有的函数

```java
public boolean equals(Object obj){
    return (this ==obj);
}//判断两个对象是否指向同一块内存
public String toString(){
    retrun getClass().getName()+"@"+Integer.toHexString(hashCode());
}//返回包名.类名@地址转的hash
protected native Object clone()//克隆出一个一摸一样的对象
```



### 方法的重写

覆盖原有的方法实现，就相当于Object类原有的函数，此时这个类把这个同名函数的功能重写了

```java
@override
+原有的函数名（所有参数都要和原来是一样的）
//
public class Person{
   String name;
   int age;
   String sex;
   @Override
   public boolean equals(Object obj) {
      if(obj==null||!(obj instanceof Person))return false;//首先类型不同直接false
      Person person=(Person)obj;//此时要强制类型转换因为obj此时还是Object类的
      return this.name.equals(person.name)&& //java只有内置的数据类型可以用==判断是否相等，其他的用==比的是是不是同一块内存
              this.age==person.age&&
              this.sex.equals(person.sex);
   }
}
```

如果我们不希望子类重写某个方法，那么在这个方法前面加final关键字

变量前面由final，只能在构造函数里赋值，如果有初始值，那么构造函数里面也不可以赋值

子类在重写父类函数的时候，不能降低父类函数的可见性，详细来说就是改写的函数的权限必须要比原来父类函数的权限要高，父类函数是protected的那么子类重写函数的权限只能是protected或者public的



### 抽象类

和cpp抽象类是一样的

此时要在类名前面加abstract 虚函数前面也要加abstract，抽象类的子类必须重写父类的虚函数

抽象类不可以实例化，就不可以new出来

抽象方法不能为private



### 接口

很类似抽象类的概念，抽象类里某些函数父类没有实现功能定义为虚函数留给子类实现，接口就相当于把父类里的虚函数拿出来单独做一个模块让子类来实现

**定义** 

```java
interface 接口名{}
```

**类实现接口**

```java
class 某个类 implements 接口名
```

接口里面只能有抽象方法和静态方法，所有函数的前面必须为 public abstract（默认就是这个）

和抽象类一样，如果一个类添加了某个接口，那么这个类里必须实现接口里的抽象函数

可以定义接口变量 ，new的时候只能new有这个接口的类，和抽象类一样不能new 接口对象

一个类可以加多个接口，用逗号将接口名隔开

接口也支持向下转型

接口中可以存在方法的默认实现，要在函数前面加default

```java
public interface Study{
    default void test(){
    }
}
```

**如果类里重写接口的方法并且要使用接口里的默认实现**，要

```java
接口名.super.方法名()
```

接口里可以有静态变量和静态函数

定义的变量默认是public static final 的

**Object类提供的克隆方法**

正常的=相当于是浅拷贝，clone方法是深拷贝

```java
public class Person  implements Cloneable{//要加入Cloneable的接口
   @Override
   public Object clone() throws CloneNotSupportedException {//要设置为public
      return super.clone();
   }
}
```



接口可以继承，接口的继承可以多继承

假如父类实现了接口，子类可以不用实现接口了

Object里的函数在接口里面不能有默认实现



## 枚举类

语法

```java
public enum Status {
    RUNNING,WORKING,SLEEPING;//枚举的基本类型，本质上其实是实例化的对象
    //本质上其实是Status RUNNING,WORKING,SLEEPING;
}
要使用这些基本类型： Status.RUNNING
```

枚举是类，也可以有成员变量

假如此时是有参构造函数，那么前面的实例化对象也要相应的加上参数

```java
public enum Status {
    RUNNING("跑步"),WORKING("工作"),SLEEPING("睡觉");
    private String s;
    Status(String ss){
        s=ss;
    }
}
```



# 面向对象高级篇

## 基本类型包装类

Java提供的基本类型包装类，将基本数据类型也封装成类

Byte,Short,Integer,Float,Double,Character,Boolean

```java
Integer a=new Integer(10);//创建变量
Integer a=1;//自动装箱 等价于Integer.valueOf(1);
int b=a;//自动拆箱 等价于a.intValue();
```

通过包装类将字符串转成整数

```java
Integer a=new Integer("666");
```

将整数转成其他进制的数

```java
System.out.print(Integer.toHexString(10));//将十进制数转成16进制数
```

## 特殊包装类

**BigInteger**

```java
需要import java.math.BigInteger;
 BigInteger a=BigInteger.valueOf(111);//用字面量或者long类型来初始化
不能直接用+-*/符号，要使用类的函数add等等，并且返回的是一个新的对象
 a=a.add(BigInteger.valueOf(Long.MAX_VALUE));
```

**BigDecimal**高精度浮点数

```java
BigDecimal a=BigDecimal.valueOf(10);
a=a.divide(BigDecimal.valueOf(3),100, RoundingMode.CEILING);//表示10/3 保留小数后100位，精确到最后一位的时候向上取整
```



## 数组

本质上也是一个类创建的时候要new

```java
int[] a=new int [数组大小];
int []a={1,2,3,4,5};//默认长度为5
求数组长度a.length
```

除了clone 其他方法都没有被重写，都和Object类里是一样的

支持这么些for(int x:a)

final修饰数组表示的是arr指向的对象不能变，但是数组里面的值可以改

二维数组

```java
int[][]a=new int [3][4];
```



## 可变长参数

给函数传入参数的时候可以这么写

```java
数据类型...变量名
```

此时变量名本质上是一个数组，可以传任意数量的该数据类型变量

如果有可变长参数的变量以及有多个变量，可变长参数变量要放在最后，只能有一个可变长参数



## 字符串

String类不可更改，不能直接下标访问，要str.charAt();

要修改字符串可以用StringBuilder类，最后再toString()转成String

### 正则表达式

判断一个字符串是否符合规则

```java
s.matches("")//里面加正则表达式规则
```

- `*` 匹配前面子表达式0次或者多次，”zo*“能匹配“z"和"zoo"，等价于{0，}
- `+`匹配前面的子表达式一次或者多次，"zo+"不能匹配”z“ 等价于{1，}
- `?`匹配0次或者1次
- {n} 确定匹配n次
- {n,} 至少匹配n次
- {n,m} 匹配[n,m]次

要匹配一个范围内的字符，可以用方括号

- [abc] 匹配abc中任意一个字符
- [^abc]匹配除了abc的任意一个字符
- [a-z]可以表示范围
- .匹配除了和换行符(\n,\r)之外的任何字符，等效[\^\n\r]
- \w 匹配字母，数字下划线 等价[A-Za-z0-9_]



## 内部类

### 成员内部类

一个类里面可以再声明一个类，成员内部类和成员方法和成员变量是一样的，是对象所有的

**创建成员内部类**

```java
public class Test {
    public class Inner{

    }
}
Test a=new Test();
Test.Inner in=a.new Inner();
```

成员内部类可以访问外部的变量

```java
public class Test {
    String s;
    public class Inner{
        void func(){
            System.out.print(s);
        }
    }
}
```

但是外部不能访问内部的变量/函数，除非是静态的，但是JDK16才能再内部类创建静态变量和函数

假如外部和内部变量/函数同名，那么

```java
public class Test {
    String s="out";
    public class Inner{
        String s="in";
        void func(String s){
            System.out.print(s);//函数参数的s
            System.out.print(this.s);//内部类的s
            System.out.print(Test.this.s);//外部的s
        }
    }
}
//包括对super关键字的使用也是一样的
this.toString();//内部
super.toString();//内部的父类
Test.this.toString();//外部
Test.super.toString();//外部的父类
```

### 静态内部类

静态内部类和静态方法/变量一样，是属于类的，此时可以直接创建

```java
public class Test {
    public static class Inner{
    }
}
Test.Inner in=new Test.Inner();
```

静态内部类只能访问外部类的静态方法/变量，外部类也只能访问静态内部类的静态方法/变量

### 局部内部类

将一个类定义在一个函数里面，那么这个类的生命周期就是在这个函数里面

### 匿名内部类

在抽象类和接口中都需要有子类才能实例化对象，此时可以创建一个临时类作为它的子类对象，抽象类和接口里的抽象方法都必须在这个临时类里实现

```java
public abstract class Test{
    public abstract void func();
}
    Test test=new Test(){
    public void func(){

    }
    };
//接口是一样的，把class换成interface就行
```



### lambda表达式

**如果一个接口中有且仅有一个待实现的抽象方法，那么可以把匿名内部类简写为lambda表达式**

```java
public interface Test {
    void func();
}
    Test test=(参数列表)->{
        //func函数的函数体
    };
```

接口内部类必须有且仅有一个未实现的抽象方法，可以有多个方法，但有且仅有一个方法没有默认实现

如果只有一行代码那么花括号可以省略

```java
 Test test=()->System.out.print("hello");
```

如果方法体只有一个返回语句，那么可以省略花括号和return关键字

```java
int test();
Test test=()->1+1;
```



### 方法引用

方法引用就是将某个类里已实现的方法，直接作为接口中抽象方法的实现（前提是函数的定义要完全一样）

- 静态方法，可以直接用类名来使用

  ```java
  public interface Test {
     int sum(int a,int b);
  }
  Test test=Integer::sum;
  ```

- 成员方法,需要创建对象来调用

```java
public static void main(){
    Main main = null;
    Test test=main::sum;
}
public int sum(int a,int b){
    return 1;
}
```

- 也可以引用一个类的构造函数

  ```java
  String func();
  Test test=String::new;
  ```

  



## 异常机制

### 异常的类型

- 运行时异常

编译时无法感知代码是否会出现问题，只有运行的时候才会知道会不会出错，所有运行时异常都继承自RuntimeException

- 编译时异常

编译时异常明确指出可能会出现的异常，继承自Exception



### 抛出异常

抛出异常时要创建一个异常对象

```java
throw new 异常类("提示信息")
```

如果一个方法中抛出了一个编译时异常，那么必须告知函数我们会抛出一个异常，**在函数体前面加上throws 编译时异常类型**

```java
public static void main() throws CloneNotSupportedException {
        Test test =new Test();
        test.clone();
}
```

如果不同分支会出现不同的编译时异常，那么所有可能会抛出的异常都要在函数体前面注明

对于调用这个会抛出编译时异常的函数的函数，要么接着throws给它的父亲函数，要么直接处理这个异常（try catch)



### 异常的处理

```java
try{
    可能会出现异常的地方
}catch(异常类型 异常对象){
    处理出现的异常
}//catch可以有多个，但是是按顺序执行的
```

此时程序会接着运行下去

catch能捕获的只有Throwable类和它的子类

如果需要一个catch代码块同时处理多个异常，可以使用`|`将这几个异常连接在一起

**finally**代码块，无论程序出没出现异常，最终finally代码块里的内容都会执行



### 断言表达式

assert 接bool表达式，如果返回false直接抛出AssertionError

```java
assert false:"打印的提示信息";
```



## 常用工具类

### 数学类 Math

类里提供了很多静态函数可以使用

```java
Math.PI 表示Π
Math.log(Math.E) e为底数的指数函数 Math.E 表示e
```



### 随机数Random类

**需要import java.util**

```java
 Random random=new Random();
 random.nextInt(100);//生成0~100的随机数
```



### 数组工具类 Arrays类

**需要import java.util**

```java
Arrays.sort(数组名)
int[]target= Arrays.copyOf(a,2);//拷贝一个数组，第二维为长度
int[]target= Arrays.copyOfRange(a,2,3);//拷贝一个数组，左闭右开
Arrays.equals(数组a，数组b)//两个数组内容是不是一样的
```



# 泛型程序设计

## 泛型

### 泛型类

```java
class Test<T>{  
}
Test<int> =new Test<int>();
```

不能直接构造待定类型的对象

如果要存放基本数据类型的值，我们只能使用对应的包装类

如果要让某个变量能够引用任意类型的泛型，那么可以使用?通配符

```java
A<?> a=new A<String>();
a=new A<Integer>();
```



### 泛型与多态

假如一个接口是一个泛型接口,使用这个接口的类要么直接类型或者让这个类也是抽象类

```java
public interface Test<T> {
}
public class A<T> implements Test<T>{
}//还是抽象类
public class A implements Test<String>{
}//指明了类型
```



### 泛型方法

```java
public static <T> void func(T a){
}
```

Arrays的sort自定义比较规则方法，就是通过创建泛型接口的匿名内部类来实现接口里的泛型函数

```java
Arrays.sort(a,new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                return a-b;//升序
            }
 });
```



### 泛型的界限

#### 上界

如果要指定泛型只能是某个类或者是某个类的子类，**那么可以在类型后面加上extends 父类**

```java
public class A< T extends Number>{
}
```

在使用变量的时候泛型通配符也支持泛型的界限

```java
A<? extends Number> a=new A<String>();//此时会报错
```

#### 下界

**泛型类和泛型方法没有下界，只有泛型通配符有下界**

```java
A<?super Integer>a=new A<Number>(); 
```



如果定义了上界那么可以使用上界的成员函数因为一定是上界的子类



### 类型擦除

java实际上并不是真的有泛型，一个泛型类编译后，如果这个类有上界那么就是这个上届的类，否则就是Object类



### 函数式接口

JDK1.8提供了许多可以用lambda表达式的泛型接口

- Supplier供给型函数接口（import java.util.function.Supplier;）

它提供了一个方法get用于获取需要的对象

```java
public interface Supplier<T>{
    T get();
} 
Supplier<A>s=()->{
     return new A();
};
也可以替换成方法引用
 Supplier<A>s=A::new;
```

- Consumer 消费型函数接口（import java.util.function.Consumer;）

```java
public interface Consumer<T> {
    void accept(T t);//通过accept传入一个T类型变量来消费这个变量
    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }//可以理解成将两个Consumer捆绑在一起，先执行this后执行after
}
//
Consumer<Integer>A=(Integer a)->System.out.println("first");
Consumer<Integer>B=(Integer a)->System.out.println("second");
A=A.andThen(B);
A.accept(1);
```

- Function函数型接口

```java
public interface Function<T, R> {
    R apply(T t);//将传入的类型T转成类型R
    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }
    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }
    static <T> Function<T, T> identity() {
        return t -> t;
    }
}
//compose把一个类型先转成另外一个类型和T相同
Function<Integer,String>fc=Object::toString;
String s=fc.compose(String::length).apply("111");
```

将一个对象转换成另外一个对象

- Predicate 断言型函数接口

接收一个参数，然后进行自定义判断并返回一个Boolean

```java
public interface Predicate<T> {
    boolean test(T t);
}
Predicate<Integer>a=(Integer x)->{
    return x>=60;
};
a.test(100);
```



### 判空包装类

java提供了判空包装类optional能有效处理空指针问题

初始化（通常不会初始化一个对象而会直接链式的调用它）

```java
String s=null;
Optional<String> a=Optional.ofNullable(s);
```

判断一个字符串不为空后执行 ifPresent,传的是一个Consumer接口

```java
Optional.ofNullable(s).ifPresent(
    str->{
        if(!str.isEmpty()){
            System.out.print(str);
        }
    }
);
```

将一个Optional\<T>类转成对应的T类，.get();

假如字符串为空选择备选方案

```java
String s=null;
String s2=Optional.ofNullable(s).orElse("备选方案");
System.out.print(s2);
```

将Optional\<T\>转成 Optional\<T2\>

```java
Optional<Integer> a=Optional.ofNullable("111").map(String::length);//map接收的的是一个函数型接口
```

为空抛出异常

```java
Optional.ofNullable("111").orElseThrow(NullPointerException::new);
```





# 集合类与IO

## 集合类

### 集合根接口

Collection 集合根接口定义了集合类的一些基本操作

### List列表

List是集合类的一个子接口

通常创建list变量创建的都是接口变量

```java
List<Integer>a=new ArrayList<>();
```

ArrayList 是数组实现

Linklist是链表实现



### 迭代器

创建迭代器

```java
List<Integer>ls=new ArrayList<Integer>();
Iterator<Integer>it=ls.iterator();
```

使用迭代器来遍历

```java
while(it.hasNext()){
    System.out.println(it.next());
}
```

迭代器是一次性的，如果要再次进行便利操作，需要重新生成一个迭代器对象

**java8提供了一个支持lambda表达式的forEach方法，这个方法接受一个Consumer**

```java
ls.forEach(System.out::println);//forEach内部本质上用的也是迭代器
```

只要实现了Iterable接口的类也可以使用迭代器





### stream流

假如要对一个List\<String\>进行多重筛选和操作，比如

- 删除长度不大于3的字符串
- 删除首字母不大写的字符串
- 去掉重复的字符串

```java
list=list
.stream()//得到一个stream对象
.filter(str->str.length()>=3)//里面传的断言表达式 返回值为true的不会被过滤
.filter(str->str.charAt(0)>='A'&&str.charAt(0)<='Z')
.distinct()//去重
.collect(Collectors.toList());//重新写成list
//还有的操作
.map()//里面接受的是Function函数型接口
//e.g .map(str->str+"1")
```



### 集合判定对象相等

假如自己手写一个类再集合类里调用contains方法

- List接口：只需重写equals方法
- Map,Set：需要手写equals和hashcode方法





## Java I/O

### 文件字节输入流

```java
FileInputStream stream=null;
try{
    stream=new FileInputStream("1.txt");//这里接文件路径，相对路径和绝对路径都行
}catch(IOException e){
    e.printStackTrace();
}finally{//无论是否抛出异常都会执行关闭stream
    if(stream!=null){
        try{
            stream.close();
        }catch(IOException e){
            throw new RuntimeException(e);
        }
    }
}
//这个写法太繁琐了 JDK1.7新增了try-with-resource语法
try(FileInputStream stream=new FileInputStream("1.txt")){

}
catch(IOException e){

}//这个写法会自动close

int c=stream.read();//读取一个字符 如果到了结尾返回-1
stream.available();//返回流里有多少个字节，一个中文字符占3个字节
//每次读取三个字节
byte[]bt=new byte[3];
while(stream.read(bt)!=-1){
    System.out.println(new String(bt));
}
bt.skip(num);//跳过num字节的字符
```



### 文件字节输出流

```java
try(FileOutputStream out =new FileOutputStream("1.txt",true)){//这里默认是false表示覆盖，true则为追加
    out.write('A');
    out.write("111".getBytes());//将一个String转成byte数组并写入文件
    out.flush();
}
catch(IOException e){
    e.printStackTrace();
}
```



### 文件字符流

字符流不同于字节，字符流是以一个具体的字符进进行读取，因此只适合读纯文本文件

FileReader,FileWriter



### 文件对象

```java
File file=new File("1.txt");//这个路径可以是文件也可以是文件夹
file.createNewFile();//创建一个文件
file.exists();//是否已经存在
file.mkdir();//创建目录
file.mkdirs();//创建多级目录
file.delete();//删除文件
```



### 缓冲流

从外部IO设备直接读取的速度一般非常慢，buffer就相当于提前将部分内容存入缓冲区

BufferedInputStream，BufferedOutputStream，BufferedWriter，BufferedReader这几个类就相当于把前面学的文件字节流和文件字符流添加了一个缓冲区数组

初始化

```java
try(BufferedInputStream in=new BufferedInputStream(new FileInputStream("1.txt"))){

}
catch(IOException e){
    e.printStackTrace();
}
```

I/O操作一般不能重复读取内容，就文件指针不能回退，但是缓冲流可以

```java
in.mark(limit);//在当前文件流指针的地方打一个mark
in.reset();//回溯到前面mark的地方，limit的意思是假如limit>=缓冲区的大小并且从mark之后读取的字节数>=limit会报错
```



### 打印流

之前使用的System.out 其实就是一个PrintStream 对象

用PrintStream 直接往文件里写字符串更加方便

```java
PrintStream p=new PrintStream(new FileOutputStream("1.txt"));
p.println();
```



### 数据流和对象流

#### **数据流**

支持基本数据类型的直接读取，同样output也支持

```java
DataInputStream input=new DataInputStream(new FileInputStream("1.txt"));
```

#### **对象流**

ObjectOutputStream不仅支持基本数据类型，而且通过对象的序列化操作，支持对对象的IO

自己手写的类如果要对象流可以操作的话要加上implements Serializable接口 

假如一个类有一定的更改，原来保存的数据只适用于原来的版本，那么可以用版本号来区分不同的版本

```java
 private static final long serialVersionUID=123456;//在序列化的时候，会被自动添加这个属性，它表示当前类的版本，我们也可以手动指定版本
```

当发生版本不匹配的时候将无法反序列化为对象

如果我们不希望某些属性参与到序列化里面可以加transient关键字



# 多线程和反射

## 多线程

### 线程的创建和启动

```java
Thread t1=new Thread(()->{
},"线程名字");//创建一个线程对象
t1.start();//启动新线程
t1.run();//在当前线程执行
Thread a=Thread.currentThread();//获取当前线程的线程对象
t1.stop();//终止当前线程
```

### 线程的中断和休眠

```java
Thread.sleep(1000);//类的静态方法，表示让当前进程休眠多少毫秒
t1.interrupt();//给t1这个线程打终端标记
if(Thread.currentThread().isInterrupted()){}//如果当前进程打了终端标记则返回true
Thread.interrupted();//取消当前进程的中断标记
```

### 线程的优先级

线程的优先级一般分为三种

```java
t1.setPriority(Thread.MAX_PRIORITY);//高
t1.setPriority(Thread.MIN_PRIORITY);//低
t1.setPriority(Thread.NORM_PRIORITY);//常规
```

### 线程的礼让和加入

```java
t1.yield();//将当前CPU资源让位给其他同优先级线程
t1.join();//假如当前在t2进程，意思就是先让t1执行，等t1执行完了再接着执行t2
```

### 线程锁和线程同步

假如两个线程同时访问一块内存并进行修改操作，可能原来x=1两个线程都让x++，x应该是3，但是可能同时读取内存修改后同时放回去结果变成2

可以使用**synchronized**修饰代码块或者函数

```java
new Thread(()->{
    Main main=new Main();
   synchronized (锁){//这里的锁可以是类的对象也可以是类 main / Main.class();
   }
});//修饰代码块
//假如两个线程用的是同一个锁，那么不能同时执行代码块里的内容

public synchronized  void func(){
        
}//修饰函数，其实修饰函数和修饰代码块本质上是一样的，如果是成员函数锁就是对象，如果是静态函数锁就是类
//synchronized修饰一个类的静态方法作为锁和用这个类作为锁的代码块也不能同时执行代码块和函数里的内容
```

### 死锁

两个线程互相持有对方需要的锁，但是又迟迟不释放

```java
Object o1=new Object();
Object o2=new Object();
new Thread(()->{
    synchronized (o1){
        synchronized (o2){

        }
    }
});
new Thread(()->{
    synchronized (o2){
        synchronized (o1){

        }
    } 
});
```

### wait和notify方法

```java
  Object o1=new Object();
        Object o2=new Object();
        new Thread(()->{
           synchronized (o1){
               try{
                   System.out.println("线程开始");
                   Thread.sleep(1000);
                   System.out.println("开始等待");
                   o1.wait();//线程1处于等待状态，并且释放o1这把锁，其他锁为o1的代码块可以执行
                   //原来线程1的o1代码块没结束之前其他线程是不能执行锁为o1的代码块的
                   System.out.println("线程1结束");
               } catch (InterruptedException e) {
                   throw new RuntimeException(e);
               }
           }
        }).start();
        new Thread(()->{
            synchronized (o1){
                System.out.println("我拿到锁了");
                o1.notify();//唤醒处于等待状态的同一把锁的进程，唤醒的进程只能在这个进程结束后才能执行
                System.out.println("线程2结束");
            }
        }).start();
```



### ThreadLocal

使用ThreadLocal创建的变量，将变量创建到自己的工作内存，不同线程访问ThreadLocal对象的时候，只能获取当前线程所属变量

```java
new Thread(()->{
    ThreadLocal<String>local=new ThreadLocal<>();
    local.set("111");
    System.out.println(local.get());
}).start();
new Thread(()->{
    local.set("111");//报错了
    System.out.println(local.get());
}).start();
//如果是父线程创建的变量子线程能访问这个变量本身，但是是空的
ThreadLocal<String>local=new InheritableThreadLocal<>();
//如果申请的是InheritableThreadLocal,子线程将copy父线程的这个变量，但本质还是两块单独的内存
```

### 定时器

可以进行循环定时任务

```java
Timer timer=new Timer();
timer.schedule(new TimerTask() {
    @Override
    public void run() {

    }
},3000,1000);//第二个参数表示延迟多久后执行，第三个参数表示间隔多久后执行
timer.cancel();//使这个任务停止
```

### 守护线程

当所有非守护线程结束后，守护线程自动结束，所以守护线程不适合IO操作

```java
Thread t1=new Thread();
t1.setDaemon(true);//设置这个线程为守护线程
//在守护线程里创建的线程也是守护的
```







# 反射

反射就是把Java类中的各个成分映射成一个个Java对象，即在运行状态中，对于一个类，都能知道这个类的所有属性和方法，对于任意一个对象，都能调用它的任意一个方法和属性，这种动态获取信息及动态调用对象方法的功能叫Java的反射机制

## class类与多态

判断是否为子类或者接口/抽象类的实现

```java
Integer.class.asSubclass(Number.class);
```



## 获取class对象

```java
Class<String> clazz=String.class;
Class<?> clazz1 = Class.forName("java.lang.String");
Class<?> clazz2 = "".getClass();
```

ps:包装类的类对象和基本数据类型的类对象不是一个

```java
System.out.println(Integer.class == int.class); // false
```



## 获取构造方法

```java
Constructor[] constructors = clazz.getConstructors();//返回所有公共构造方法对象的数组
Constructor[] constructors = clazz.getDeclaredConstructors();//返回所有构造方法对象的数组
Constructor constructor = clazz.getConstructor();//返回单个公共构造方法对象
Constructor constructor = clazz.getDeclaredConstructor();//返回单个构造方法对象

Constructor constructor = clazz.getDeclaredConstructor(String.class);
constructor.setAccessible(true);
String s=(String) constructor.newInstance("111");
//使用构造方法生成对象
```



## 获取成员变量

```java
Field[]fields=clazz.getFields();//返回所有公共成员变量的数组
Field[]fields=clazz.getDeclaredFields();//返回所有成员变量的数组
Field field=clazz.getField("id");//返回单个公共成员变量对象
Field field=clazz.getDeclaredField("id");//返回单个成员变量对象


//获取成员变量的名字
int modifiers = field.getModifiers();

//获取成员变量数据类型
Class<?> type = field.getType();

//获取成员变量记录的值
Student s=new Student("111",1);
field.setAccessible(true);
String value =(String)field.get(s);

//修改对象里面记录的值
field.set(s,"222");
```



## 获取成员方法

```java
Method[] methods = clazz.getMethods();//返回所有公共成员方法对象的数组，包括继承的
Method[] methods = clazz.getDeclaredMethods();//返回所有成员方法对象的数组，不包括继承的
Method method = clazz.getMethod("func1");//返回单个公共成员方法对象
Method method = clazz.getDeclaredMethod("func1");//返回单个成员方法对象

//获取方法修饰符
int modifiers = method.getModifiers();

//获取方法名字
String name=method.getName();

//获取方法的形参
Parameter[] parameters = method.getParameters();

//获取方法抛出的异常
Class<?>[] exceptionTypes = method.getExceptionTypes();

Student s=new Student("111",11);
method.setAccessible(true);

//方法调用
method.invoke(s,1);
```





# 动态代理

代理可以无侵入的给对象增强其他功能

```java
package com.creep;

public interface Star {
    public String sing(String name);
    public void dance();
}
//Star
package com.creep;

public class BigStar implements Star{
    private String name;
    BigStar(String _name){
        this.name=_name;
    }
    @Override
    public String sing(String name) {
        System.out.println(this.name+"正常唱"+name);
        return "谢谢";
    }

    @Override
    public void dance() {
        System.out.println(this.name+"正在跳舞");
    }
}
//BigStar
package com.creep;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyUtil {

    public static Star createProxy(BigStar bigStar){
        Star s = (Star)Proxy.newProxyInstance(ProxyUtil.class.getClassLoader(), new Class[]{Star.class}, new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                return method.invoke(bigStar, args);
            }
        });
        return s;
    }
}
//ProxyUtil
```

