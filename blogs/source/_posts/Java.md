---
title: Java
date: 2024-06-15 08:50:20
auther: 小狼
summary: Java八股零散整理
categories: 后端
tags:
  - Java
---

# Java

## 基础概念

**Java特点**：面向对象（封装，继承，多态）；平台无关（基于JVM）；可靠安全（异常处理，自动内存管理，多重安全防护）；编译与解释并存->一次编译，到处运行；Java生态

Java SE 是 Java 的基础版本，Java EE 是 Java 的高级版本。Java SE 更适合开发桌面应用程序或简单的服务器应用程序，Java EE 更适合开发复杂的企业级应用程序或 Web 应用程序。

> 编译型：编译型语言会通过编译器将源代码一次性翻译成可被该平台执行的机器码。一般情况下，编译语言的执行速度比较快，开发效率比较低。常见的编译性语言有C、C++、Go、Rust等等。
> 解释型：释型语言会通过解释器一句一句的将代码解释（interpret）为机器代码后再执行。解释型语言开发效率比较快，执行速度比较慢。常见的解释性语言有 Python、JavaScript、PHP 等等。
> Java为了实现“一次编译，处处运行”的特性，把编译的过程分成两部分，首先它会先由javac编译成通用的中间形式——字节码，这些字节码可以在任何安装了Java虚拟机的平台上运行，由解释器逐条将字节码解释为机器码来执行。这种方式使得Java程序具有了跨平台性，同一份Java代码可以在各种操作系统和硬件平台上运行，而不需要针对不同平台进行重新编译。

在 Java 中，JVM 可以理解的代码就叫做字节码（即扩展名为 `.class` 的文件），它不面向任何特定的处理器，只面向虚拟机。Java 语言通过字节码的方式，**在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点**。

JVM引入 **JIT（Just in Time Compilation）** 编译器， 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。

**Java和C++：**

* Java **不提供指针**来直接访问内存，程序内存更加安全
* Java 的类是单继承的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。
* Java 有**自动内存管理垃圾回收机制(GC)**，不需要程序员手动释放无用内存。
* C ++同时支持方法重载和操作符重载，但是 Java **只支持方法重载（**操作符重载增加了复杂性，这与 Java 最初的设计思想不符）。

### 基本数据类型

<img src="Java\image-20240615094911702.png" alt="image-20240615094911702" style="zoom:80%;" />

###  基本类型和包装类型

* **用途**：基本类型用于定义一些**常量和局部变量**。方法参数/对象属性等多用包装类型。并且，包装类型可用于泛型，而基本类型不可以。
* **存储方式**：基本类型的**局部变量**存放在 Java 虚拟机**栈**中的局部变量表中，基本数据类型的**成员变量**（未被 `static` 修饰 ）存放在 Java 虚拟机的**堆**中。包装类型属于对象类型，存在于堆中。
* **占用空间**：相比于包装类型（对象类型）， **基本数据类型占用的空间往往非常小。**
* **默认值**：成员变量包装类型不赋值就是 `null`，而**成员变量基本类型有默认值**且不是 `null`。
* **比较方式**：对于基本数据类型来说，`==` 比较的是值。对于包装数据类型来说**，`==` 比较的是对象的内存地址。**所有整型包装类对象之间值的比较，全部使用 `equals()` 方法。

**包装类型缓存机制：**包装类型的大部分都用到了缓存机制来提升性能。`Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建了数值在 **[0,127]** 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。如果超出对应范围仍然会去创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。两种浮点数类型的包装类 `Float`,`Double` 并没有实现缓存机制。

### 成员变量与局部变量

* **语法形式**：从语法形式上看，成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 `public`,`private`,`static` 等修饰符所修饰，而局部变量不能被访问控制修饰符及 `static` 所修饰；但是，成员变量和局部变量都能被 `final` 所修饰。

* **存储方式**：从变量在内存中的存储方式来看，如果成员变量是使用 `static` 修饰的，那么这个成员变量是属于类的，如果没有使用 `static` 修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
* **生存时间**：从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。
* **默认值**：从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外:被 `final` 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。

### 重载和重写

<img src="Java\image-20240615115307076.png" alt="image-20240615115307076"  />

关于 **重写的返回值类型** 这里需要额外多说明一下，上面的表述不太清晰准确：`如果方法的返回类型是 void 和基本数据类型，则返回值重写时不可修改`。但是如果方法的返回值是引用类型，重写时是可以返回该引用类型的子类的。

## 面向对象

### 封装

封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。

### 继承

不同类型的对象，相互之间经常有一定数量的共同点。继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。

* 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，**只是拥有**。
* 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
* 子类可以用自己的方式实现父类的方法。

### 多态

表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。

* 对象类型和引用类型之间具有继承（类）/实现（接口）的关系；
* 引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定；
* 多态不能调用“只在子类存在但在父类不存在”的方法；
* 如果子类重写了父类的方法，真正执行的是子类重写的方法，如果子类没有重写父类的方法，执行的是父类的方法。

### 接口和抽象类

**抽象类**：包含抽象方法的类。通过abstract关键字来创建抽象类，以及定义抽象方法。抽象类的存在就是为了被继承，所以抽象类中的抽象方法不能被private、static、final修饰，否则无法被继承。抽象类虽然不能被实例化，但是它可以有构造方法，供子类创建对象时，初始化父类成员。

**接口**：接口是一种引用数据类型，可以看成是多个类的公共规范。定义接口需要借助interface关键字，定义方式与定义类的方式相似：

**共同点**：

- 都不能被实例化。
- 都可以包含抽象方法。**每个抽象方法前都隐藏着public abstract修饰。**
- **都可以有默认实现的方法**（Java 8 开始可以用 `default` 关键字在接口中定义默认方法）。

**区别**：

- **接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为**。抽象类主要用于代码复用，强调的是**所属关系**。
- 一个类只能继承一个类，但是可以实现多个接口。
- 接口中的**成员变量**只能是 `public static final` 类型的，不能被修改且必须有初始值，而**抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。**
- 接口中**不能有**静态代码块（可以有静态成员方法）、实例代码块以及构造方法；而**抽象类可以有构造方法**，供子类创建对象时，初始化父类成员。

### 拷贝

<img src="Java\shallow&deep-copy.png" alt="浅拷贝、深拷贝、引用拷贝示意图" style="zoom:80%;" />

## String

**线程安全性：**`String` 中的对象是不可变的，也就可以理解为常量，线程安全。`AbstractStringBuilder` 是 `StringBuilder` 与 `StringBuffer` 的公共父类，定义了一些字符串的基本操作，如 `expandCapacity`、`append`、`insert`、`indexOf` 等公共方法。`StringBuffer` 对方法加了**同步锁**或者对调用的方法加了同步锁，所以是线程安全的。`StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。

**性能：**每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。`StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

### String不可变

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char value[];
  //...
}
```

被 `final` 关键字修饰的类不能被继承，修饰的方法不能被重写，修饰的变量是基本数据类型则值不能改变，修饰的变量是引用类型则不能再指向其他对象。因此，`final` 关键字修饰的数组保存字符串并不是 `String` 不可变的根本原因，因为**这个数组保存的字符串是可变的**（`final` 修饰引用类型变量的情况）。

`String` 真正不可变有下面几点原因：

1. 保存字符串的数组被 `final` 修饰且为私有的，并且**`String` 类没有提供/暴露修改这个字符串的方法。**
2. `String` 类被 `final` 修饰导致其**不能被继承**，进而避免了子类破坏 `String` 不可变。

在 Java 9 之后，`String`、`StringBuilder` 与 `StringBuffer` 的实现改用 `byte` 数组存储字符串。

```java
public final class String implements java.io.Serializable,Comparable<String>, CharSequence {
    // @Stable 注解表示变量最多被修改一次，称为“稳定的”。
    @Stable
    private final byte[] value;
}

abstract class AbstractStringBuilder implements Appendable, CharSequence {
    byte[] value;

}
```

新版的 String 其实支持两个编码方案：Latin-1 和 UTF-16。如果字符串中包含的汉字没有超过 Latin-1 可表示范围内的字符，那就会使用 Latin-1 作为编码方案。Latin-1 编码方案下，`byte` 占一个字节(8 位)，`char` 占用 2 个字节（16），`byte` 相较 `char` 节省一半的内存空间。

## 异常

<img src="Java\types-of-exceptions-in-java.png" alt="Java 异常类层次结构图" style="zoom:80%;" />

**`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。

* **Checked Exception** 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译。

* **Unchecked Exception** 即 **不受检查异常** ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。

  `RuntimeException` 及其子类都统称为非受检查异常，常见的有

  * `NullPointerException`(空指针错误)
  * `IllegalArgumentException`(参数错误比如方法入参类型错误)
  * `NumberFormatException`（字符串转换为数字格式错误，`IllegalArgumentException`的子类）
  * `ArrayIndexOutOfBoundsException`（数组越界错误）
  * `ClassCastException`（类型转换错误）
  * `ArithmeticException`（算术错误）
  * `SecurityException` （安全错误比如权限不够）
  * `UnsupportedOperationException`(不支持的操作错误比如重复创建同一用户)

**`Error`**：`Error` 属于程序无法处理的错误 ，不建议通过`catch`捕获 。例如 Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。

## 泛型

泛型，即“参数化类型”。在方法定义时，将方法签名中的`形参的数据类型`也设置为参数（也可称之为类型参数），在调用该方法时再从外部传入一个具体的数据类型和变量。**泛型的本质是为了将类型参数化**， 也就是说在泛型使用过程中，数据类型被设置为一个参数，在使用时再从外部传入一个数据类型；而一旦传入了具体的数据类型后，传入变量（实参）的数据类型如果不匹配，编译器就会直接报错。这种参数化类型可以用在类、接口和方法中，分别被称为泛型类、泛型接口、泛型方法。

* 泛型提供了一种扩展能力，更符合面向对象开发的软件编程宗旨。
* 泛型提高了程序代码的可读性。在定义泛型阶段（类、接口、方法）或者对象实例化阶段，由于 < 类型参数 > 需要在代码中显式地编写，所以程序员能够快速猜测出代码所要操作的数据类型，提高了代码可读性。

### 泛型类

类型参数用于类的定义中，则该类被称为泛型类。通过泛型可以完成对一组类的操作对外开放相同的接口。最典型的就是各种容器类，如：List、Set、Map等。

```java
public class Generic<T> { 
    // key 这个成员变量的数据类型为 T, T 的类型由外部传入  
    private T key;

	// 泛型构造方法形参 key 的类型也为 T，T 的类型由外部传入
    public Generic(T key) { 
        this.key = key;
    }
    
	// 泛型方法 getKey 的返回值类型为 T，T 的类型由外部指定
    public T getKey(){ 
        return key;
    }
}
```

**泛型类中的静态方法和静态变量不可以使用泛型类所声明的类型参数**

- 泛型类中的**类型参数的确定是在创建泛型类对象**的时候（例如 ArrayList< Integer >）。
- 而静态变量和静态方法在类加载时已经初始化，直接使用类名调用；在泛型类的类型参数未确定时，静态成员有可能被调用，因此泛型类的类型参数是不能在静态成员中使用的。
- 静态泛型方法中可以使用自身的方法签名中**新定义的类型参数**（即泛型方法），而不能使用泛型类中定义的类型参数。

### 泛型接口

```java
public interface Inter<T> {
    public abstract void show(T t) ;
}
```

**泛型接口中的类型参数，在该接口被继承或者被实现时确定。**

### 泛型方法

当在一个方法签名中的返回值前面声明了一个 < T > 时，该方法就被声明为一个`泛型方法`。< T >表明该方法声明了一个类型参数 T，并且这个类型参数 T 只能在该方法中使用。当然，泛型方法中也可以使用`泛型类中定义的泛型参数`。

```java
public class Test<U> {
	// 该方法只是使用了泛型类定义的类型参数，不是泛型方法
	public void testMethod(U u){
		System.out.println(u);
	}
	
	// <T> 真正声明了下面的方法是一个泛型方法
	public <T> T testMethod1(T t){
		return t;
	}
}
```

泛型类中定义的类型参数和泛型方法中定义的类型参数是相互独立的，它们一点关系都没有。**也就是说，泛型方法始终以自己声明的类型参数为准。**

为了避免混淆，如果在一个泛型类中存在泛型方法，那么两者的类型参数最好不要同名。

**在静态成员中不能使用泛型类定义的类型参数，但我们可以将静态成员方法定义为一个泛型方法。**

### 类型擦除

泛型的本质是将`数据类型参数化`，它通过**擦除**的方式来实现，即编译器会在编译期间`擦除`代码中的所有泛型语法并相应的做出一些类型转换动作。

换而言之，**泛型信息只存在于代码编译阶段**，在代码编译结束后，与泛型相关的信息会被擦除掉，专业术语叫做`类型擦除`。也就是说，**成功编译过后的 class 文件中不包含任何泛型信息**，泛型信息不会进入到`运行时阶段`。

```java
public class Caculate<T> {
	private T num;
}
// 将这个泛型类反编译, 结果如下
public class Caculate {
	public Caculate() {}// 默认构造器，不用管
	private Object num;// T 被替换为 Object 类型
}
```

- 可以发现编译器`擦除`了 Caculate 类后面的泛型标识 < T >，并且将 num 的数据类型替换为 Object 类型，而替换了 T 的数据类型我们称之为`原始数据类型`。

**那么是不是所有的类型参数被擦除后都以 Object 类进行替换呢？**

- 答案是否定的，大部分情况下，类型参数 T 被擦除后都会以 Object 类进行替换；而有一种情况则不是，那就是使用到了 extends 和 super 语法的`有界类型参数`（即`泛型通配符`）。

在现实编码中，确实有这样的需求，希望泛型能够处理`某一类型范围内`的类型参数，比如某个泛型类和它的子类，为此 Java 引入了`泛型通配符`这个概念。

> 1. <?> ：被称作无限定的通配符。**代表了任何一种数据类型。**
> 2. <? extends T> ：被称作有上界的通配符。 **逻辑上表示类型参数的范围是 T 和 T 的子类。**
> 3. <? super T> ：被称作有下界的通配符。 **逻辑上表示类型参数的范围是 T 和 T 的超类。**

* Object 本身也算是一种数据类型，但却不能代表任何一种数据类型，所以 ArrayList< Object > 和 ArrayList<?>的含义是不同的，前者类型是 Object，也就是继承树的最高父类，而后者的类型完全是未知的；ArrayList<?> 是 ArrayList< Object > 逻辑上的父类。

* ArrayList< Integer > 和 ArrayList< Number > 之间不存在继承关系。而引入上界通配符的概念后，我们便可以在逻辑上将 ArrayList<? extends Number> 看做是 ArrayList< Integer > 的父类，**但实质上它们之间没有继承关系。**
* ArrayList<? super Integer> 在逻辑上表示为 Integer 类以及 Integer 类的所有父类，它可以代表 ArrayList< Integer>、ArrayList< Number >、 ArrayList< Object >中的某一个集合，但实质上它们之间没有继承关系。

```java
public class Caculate<T extends Number> {
	private T num;
}
public class Caculate {
	public Caculate() {}// 默认构造器，不用管
	private Number num;
}
```

* 使用到了 extends 语法的类型参数 T 被擦除后会替换为 Number 而不再是 Object。
* extends 和 super 是一个**限定类型参数边界的**语法，extends 限定 T 只能是 Number 或者是 Number 的**子类**。 也就是说，在创建 Caculate 类对象的时候，尖括号 <> 中只能传入 Number 类或者 Number 的子类的数据类型，所以在创建 Caculate 类对象时无论传入什么数据类型，Number 都是其父类，于是可以使用 Number 类作为 T 的原始数据类型，进行类型擦除并替换。

#### 原理

泛型信息被擦除了，如何保证我们在集合中只添加指定的数据类型的对象呢？

- 其实在创建一个泛型类的对象时， Java 编译器是先检查代码中传入 < T > 的**数据类型，并记录下来**，然后再对代码进行编译，`编译的同时进行类型擦除`；如果需要对被擦除了泛型信息的对象进行操作，**编译器会自动将对象进行类型转换。**

> 可以把泛型的类型安全检查机制和类型擦除想象成演唱会的验票机制：以 ArrayList< Integer> 泛型集合为例。
>
> 当我们在创建一个 ArrayList< Integer > 泛型集合的时候，ArrayList 可以看作是演唱会场馆，而< T >就是场馆的验票系统，Integer 是验票系统设置的门票类型；
> 当验票系统设置好为< Integer >后，只有持有 Integer 门票的人才可以通过验票系统，进入演唱会场馆（集合）中；若是未持有 Integer 门票的人想进场，则验票系统会发出警告（编译器报错）。
> 在通过验票系统时，门票会被收掉（类型擦除），但场馆后台（JVM）会记录下观众信息（泛型信息）。
> 进场后的观众变成了没有门票的普通人（原始数据类型）。但是，在需要查看观众的信息时（操作对象），场馆后台可以找到记录的观众信息（编译器会自动将对象进行类型转换）。

```java
public class GenericType {
    public static void main(String[] args) {  
        ArrayList<Integer> arrayInteger = new ArrayList<Integer>();// 设置验票系统   
        arrayInteger.add(111);// 观众进场，验票系统验票，门票会被收走（类型擦除）
        Integer n = arrayInteger.get(0);// 获取观众信息，编译器会进行强制类型转换
        System.out.println(n);
    }  
}
```

擦除 ArrayList< Integer > 的泛型信息后，get() 方法的返回值将返回 Object 类型，但编译器会自动插入 Integer 的强制类型转换。也就是说，编译器把 get() 方法调用翻译为两条字节码指令：

* 对原始方法 get() 的调用，返回的是 Object 类型；
* 将返回的 Object 类型强制转换为 Integer 类型；
  

**项目中哪里用到了泛型**

- **自定义接口通用返回结果** `CommonResult<T>` 通过参数 `T` 可根据具体的返回类型动态指定结果的数据类型
- 定义 `Excel` 处理类 `ExcelUtil<T>` 用于动态指定 `Excel` 导出的数据类型
- 构建集合工具类（参考 `Collections` 中的 `sort`, `binarySearch` 方法）。

## 反射

反射 (Reflection) 是 Java 的特征之一，它允许**运行中的 Java 程序获取自身的信息**，并且可以操作类或对象的内部属性。通过反射你可以获取任意一个类的所有属性和方法，你还可以调用这些反射可以让我们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利。

不过，反射让我们在运行时有了分析操作类的能力的同时，也增加了安全问题，比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。

**使用的前提条件：必须先得到代表的字节码的Class，Class类用于表示.class文件（字节码），一切反射的操作都是从类对象开始**

反射就是把java类中的各种成分映射成一个个的Java对象

> 例如：一个类有：成员变量、方法、构造方法、包等等信息，利用反射技术可以对一个类进行解剖，把个个组成部分映射成一个个对象。

<img src="Java\20170513133210763" alt="img" style="zoom:80%;" />

### 作用

* **运行时动态获取类的信息**：在编写代码时，对于类的信息是必须在编译时确定的，但在运行时，有时需要根据某些条件，动态获取某个类的信息，这时就可以使用Java中的反射机制。
* 动态生成对象：反射机制可以在**运行时生成对象**，这样就可以根据参数的不同，动态的创建不同的类的实例对象。
* 动态调用方法：通过反射机制可以调用类中的方法，不论这些方法是否是公共的，也不论这些方法的参数个数和类型是什么，反射机制都具有这样的能力。
* 动态修改属性：利用反射机制可以获取到类中的所有成员变量，并可以对其进行修改。
* 实现动态代理：利用反射机制可以实现代理模式，通过代理对象完成原对象对某些方法的调用，同时也可以在这些方法的调用前后做一些额外的处理。

Spring/Spring Boot、MyBatis 等等框架中都大量使用了反射机制。**这些框架中也大量使用了动态代理，而动态代理的实现也依赖反射。**

另外，像 Java 中的一大利器 **注解** 的实现也用到了反射。

为什么你使用 Spring 的时候 ，一个`@Component`注解就声明了一个类为 Spring Bean 呢？为什么你通过一个 `@Value`注解就读取到配置文件中的值呢？究竟是怎么起作用的呢？

这些都是因为你可以基于反射分析类，然后获取到类/属性/方法/方法的参数上的注解。你获取到注解之后，就可以做进一步的处理。

### 获取class对象

Class 类对象将一个类的方法、变量等信息告诉运行的程序。Java 提供了四种方式获取 Class 对象:

```java
//  知道具体类的情况下可以使用 -- 类名.class
Class alunbarClass = TargetObject.class;
//  通过 Class.forName()传入类的全路径获取
Class alunbarClass1 = Class.forName("cn.javaguide.TargetObject");
//  通过对象实例instance.getClass()获取
TargetObject o = new TargetObject();
Class alunbarClass2 = o.getClass();
//  通过类加载器xxxClassLoader.loadClass()传入类路径获取
ClassLoader.getSystemClassLoader().loadClass("cn.javaguide.TargetObject");

// 反射实例化 -- 对象.newInstance()
Class<?> targetClass = Class.forName("cn.javaguide.TargetObject");
TargetObject targetObject = (TargetObject) targetClass.newInstance();
```

- **获得类中属性相关的方法**

| 方法                          | 用途                   |
| :---------------------------- | :--------------------- |
| getField(String name)         | 获得某个公有的属性对象 |
| getFields()                   | 获得所有公有的属性对象 |
| getDeclaredField(String name) | 获得某个属性对象       |
| getDeclaredFields()           | 获得所有属性对象       |

- **获得类中注解相关的方法**

| 方法                                         | 用途                                   |
| :------------------------------------------- | :------------------------------------- |
| getAnnotation(Class annotationClass)         | 返回该类中与参数类型匹配的公有注解对象 |
| getAnnotations()                             | 返回该类所有的公有注解对象             |
| getDeclaredAnnotation(Class annotationClass) | 返回该类中与参数类型匹配的所有注解对象 |
| getDeclaredAnnotations()                     | 返回该类所有的注解对象                 |

- **获得类中构造器相关的方法**

| 方法                                               | 用途                                   |
| :------------------------------------------------- | :------------------------------------- |
| getConstructor(Class...<?> parameterTypes)         | 获得该类中与参数类型匹配的公有构造方法 |
| getConstructors()                                  | 获得该类的所有公有构造方法             |
| getDeclaredConstructor(Class...<?> parameterTypes) | 获得该类中与参数类型匹配的构造方法     |
| getDeclaredConstructors()                          | 获得该类所有构造方法                   |

- **获得类中方法相关的方法**

| 方法                                                       | 用途                   |
| :--------------------------------------------------------- | :--------------------- |
| getMethod(String name, Class...<?> parameterTypes)         | 获得该类某个公有的方法 |
| getMethods()                                               | 获得该类所有公有的方法 |
| getDeclaredMethod(String name, Class...<?> parameterTypes) | 获得该类某个方法       |
| getDeclaredMethods()                                       | 获得该类所有方法       |

## 序列化

- **序列化**：将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程

应用场景：

* 对象在进行网络传输（比如远程方法调用 RPC 的时候）之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
* 将对象存储到文件之前需要进行序列化，将对象从文件中读取出来需要进行反序列化；
* 将对象存储到数据库（如 Redis）之前需要用到序列化，将对象从缓存数据库中读取出来需要反序列化；
* 将对象存储到内存之前需要进行序列化，从内存中读取出来之后需要进行反序列化。

**序列化的主要目的是通过网络传输对象或者说是将对象存储到文件系统、数据库、内存中。**

### JDK序列化

JDK 自带的序列化，只需实现 `java.io.Serializable`接口即可。

```java
@AllArgsConstructor
@NoArgsConstructor
@Getter
@Builder
@ToString
public class RpcRequest implements Serializable {
    private static final long serialVersionUID = 1905122041950251207L;
    private String requestId;
    private String interfaceName;
    private String methodName;
    private Object[] parameters;
    private Class<?>[] paramTypes;
    private RpcMessageTypeEnum rpcMessageTypeEnum;
}
```

序列化号 `serialVersionUID` 属于版本控制的作用。反序列化时，会检查 `serialVersionUID` 是否和当前类的 `serialVersionUID` 一致。如果 `serialVersionUID` 不一致则会抛出 `InvalidClassException` 异常。强烈推荐每个序列化类都手动指定其 `serialVersionUID`，如果不手动指定，那么编译器会动态生成默认的 `serialVersionUID`。

> `static` 修饰的变量是静态变量，属于类而非类的实例，本身是不会被序列化的。然而，`serialVersionUID` 是一个特例，`serialVersionUID` 的序列化做了特殊处理。当一个对象被序列化时，`serialVersionUID` 会被写入到序列化的二进制流中；在反序列化时，也会解析它并做一致性判断，以此来验证序列化对象的版本一致性。

**如果有些字段不想进行序列化怎么办？**

对于不想进行序列化的变量，可以使用 `transient` 关键字修饰。

`transient` 关键字的作用是：**阻止实例中那些用此关键字修饰的的变量序列化**；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

关于 `transient` 还有几点注意：

- `transient` 只能修饰变量，不能修饰类和方法。
- `transient` 修饰的变量，**在反序列化后变量值将会被置成类型的默认值**。例如，如果是修饰 `int` 类型，那么反序列后结果就是 `0`。
- `static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化。

### Kryo

Kryo 是一个高性能的序列化/反序列化工具，由于其变长存储特性并使用了字节码生成机制，拥有较高的运行速度和较小的字节码体积。

```java
/**
 * Kryo serialization class, Kryo serialization efficiency is very high, but only compatible with Java language
 *
 * @author shuang.kou
 * @createTime 2020年05月13日 19:29:00
 */
@Slf4j
public class KryoSerializer implements Serializer {

    /**
     * Because Kryo is not thread safe. So, use ThreadLocal to store Kryo objects
     */
    private final ThreadLocal<Kryo> kryoThreadLocal = ThreadLocal.withInitial(() -> {
        Kryo kryo = new Kryo();
        kryo.register(RpcResponse.class);
        kryo.register(RpcRequest.class);
        return kryo;
    });

    @Override
    public byte[] serialize(Object obj) {
        try (ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
             Output output = new Output(byteArrayOutputStream)) {
            Kryo kryo = kryoThreadLocal.get();
            // Object->byte:将对象序列化为byte数组
            kryo.writeObject(output, obj);
            kryoThreadLocal.remove();
            return output.toBytes();
        } catch (Exception e) {
            throw new SerializeException("Serialization failed");
        }
    }

    @Override
    public <T> T deserialize(byte[] bytes, Class<T> clazz) {
        try (ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(bytes);
             Input input = new Input(byteArrayInputStream)) {
            Kryo kryo = kryoThreadLocal.get();
            // byte->Object:从byte数组中反序列化出对象
            Object o = kryo.readObject(input, clazz);
            kryoThreadLocal.remove();
            return clazz.cast(o);
        } catch (Exception e) {
            throw new SerializeException("Deserialization failed");
        }
    }

}
```

## I/O流

IO 即 `Input/Output`，输入和输出。数据输入到计算机内存的过程即输入，反之输出到外部存储（比如数据库，文件，远程主机）的过程即输出。数据传输过程类似于水流，因此称为 IO 流。IO 流在 Java 中分为输入流和输出流，而根据数据的处理方式又分为字节流和字符流。

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

- `InputStream`/`Reader`: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- `OutputStream`/`Writer`: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

### 字节流

#### InputStream

`FileInputStream` 是一个比较常用的字节输入流对象，可直接指定文件路径，可以直接读取单字节数据，也可以读取至字节数组中。

```java
try (InputStream fis = new FileInputStream("input.txt")) {
    System.out.println("Number of remaining bytes:"
            + fis.available());	// 返回输入流中可以读取的字节数。
    int content;
    long skip = fis.skip(2);	// 忽略输入流中的 n 个字节 ,返回实际忽略的字节数。
    System.out.println("The actual number of bytes skipped:" + skip);
    System.out.print("The content read from file:");
    while ((content = fis.read()) != -1) {	// 返回输入流中下一个字节的数据。返回的值介于 0 到 255 之间。
        									// 如果未读取任何字节，则代码返回 -1 ，表示文件结束
        System.out.print((char) content);	// 将ascii码转为读到的字符
    }
} catch (IOException e) {
    e.printStackTrace();
}
输出：
    Number of remaining bytes:11
	The actual number of bytes skipped:2
	The content read from file:JavaGuide

// 新建一个 BufferedInputStream 对象
BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("input.txt"));
// 读取文件的内容并复制到 String 对象中            读取输入流所有字节
String result = new String(bufferedInputStream.readAllBytes());
System.out.println(result);

FileInputStream fileInputStream = new FileInputStream("input.txt");
//必须将fileInputStream作为构造参数才能使用
DataInputStream dataInputStream = new DataInputStream(fileInputStream);
//可以读取任意具体的类型数据
dataInputStream.readBoolean();
dataInputStream.readInt();
dataInputStream.readUTF();

// ObjectInputStream 用于从输入流中读取 Java 对象（反序列化），ObjectOutputStream 用于将对象写入到输出流(序列化)。
ObjectInputStream input = new ObjectInputStream(new FileInputStream("object.data"));
MyClass object = (MyClass) input.readObject();
input.close();
```

#### OutputStream

```java
try (FileOutputStream output = new FileOutputStream("output.txt")) {
    byte[] array = "JavaGuide".getBytes();
    output.write(array);	// 将数组写入到输出流，等价于 write(b, 0, b.length)
} catch (IOException e) {
    e.printStackTrace();
}

// 字节缓冲输出流
FileOutputStream fileOutputStream = new FileOutputStream("output.txt");
BufferedOutputStream bos = new BufferedOutputStream(fileOutputStream)

// 输出流
FileOutputStream fileOutputStream = new FileOutputStream("out.txt");
DataOutputStream dataOutputStream = new DataOutputStream(fileOutputStream);
// 输出任意数据类型
dataOutputStream.writeBoolean(true);
dataOutputStream.writeByte(1);

// 序列化，将对象写入到输出流
ObjectOutputStream output = new ObjectOutputStream(new FileOutputStream("file.txt")
Person person = new Person("Guide哥", "JavaGuide作者");
output.writeObject(person);
```

### 字符流

**不管是文件读写还是网络发送接收，信息的最小存储单元都是字节，那为什么 I/O 流操作要分为字节流操作和字符流操作呢？**

- 字符流是由 Java 虚拟机将字节转换得到的，这个过程比较耗时；
- 如果我们不知道编码类型的话，使用字节流的过程中很容易出现乱码问题。
- 所以， I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。

例如，如果你想从`InputStream`中读取字符，你需要考虑字符的编码方式。如果字符使用UTF-8编码，一个字符可能由一个或多个字节组成。因此，直接使用`InputStream`的`read()`方法可能无法完整地读取一个字符，因为它一次只读取一个字节。

要正确地从`InputStream`中读取字符，你可以使用`Reader`类及其子类，如`InputStreamReader`。`Reader`是字符输入流，专门用于读取字符。`InputStreamReader`是一个桥接类，它可以将字节流转换为字符流，同时指定字符编码。

> 1，ASCII码：一个英文字母（不分大小写）占一个字节的空间，一个中文汉字占两个字度节的空间。
>
> 2，UTF-8编码：一个英文字符等于一个字节，**一个中文（含繁体）等于三个字节**。中文标点占三个字节，英文标点占一个字节
>
> 3，Unicode编码：**一个英文等于两个字节**，一个中文（含繁体）等于两个字节。中文标点占两个字节，英文标点占两个字节
>
> 4，GBK：英文占 1 字节，中文占 2 字节。

#### Reader

`Reader`用于从源头（通常是文件）读取数据（字符信息）到内存中，`java.io.Reader`抽象类是所有字符输入流的父类。

`Reader` 用于读取文本， `InputStream` 用于读取原始字节。

```java
// 字节流转换为字符流的桥梁
public class InputStreamReader extends Reader {
}
// 用于读取字符文件
public class FileReader extends InputStreamReader {
}

try (FileReader fileReader = new FileReader("input.txt");) {
    int content;
    long skip = fileReader.skip(3);
    System.out.println("The actual number of bytes skipped:" + skip);
    System.out.print("The content read from file:");
    while ((content = fileReader.read()) != -1) {
        System.out.print((char) content);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Writer

`Writer`用于将数据（字符信息）写入到目的地（通常是文件），`java.io.Writer`抽象类是所有字符输出流的父类。

```java
// 字符流转换为字节流的桥梁
public class OutputStreamWriter extends Writer {
}
// 用于写入字符到文件
public class FileWriter extends OutputStreamWriter {
}

try (Writer output = new FileWriter("output.txt")) {
    output.write("你好，我是Guide。");
} catch (IOException e) {
    e.printStackTrace();
}
```

### 字节缓冲流

IO 操作是很消耗性能的，**缓冲流将数据加载至缓冲区，一次性读取/写入多个字节，从而避免频繁的 IO 操作**，提高流的传输效率。

字节缓冲流这里采用了**装饰器模式**来增强 `InputStream` 和`OutputStream`子类对象的功能。

举个例子，我们可以通过 `BufferedInputStream`（字节缓冲输入流）来增强 `FileInputStream` 的功能。

```java
// 新建一个 BufferedInputStream 对象
BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("input.txt"));
```

字节流和字节缓冲流的性能差别主要体现在我们使用两者的时候都是调用 `write(int b)` 和 `read()` 这两个一次只读取一个字节的方法的时候。由于字节缓冲流内部有缓冲区（字节数组），因此，**字节缓冲流会先将读取到的字节存放在缓存区，大幅减少 IO 次数，提高读取效率。**

`BufferedInputStream` 内部维护了一个缓冲区，这个缓冲区实际就是一个字节数组

```java
public
class BufferedInputStream extends FilterInputStream {
    // 内部缓冲区数组
    protected volatile byte buf[];
    // 缓冲区的默认大小
    private static int DEFAULT_BUFFER_SIZE = 8192;
    // 使用默认的缓冲区大小
    public BufferedInputStream(InputStream in) {
        this(in, DEFAULT_BUFFER_SIZE);
    }
    // 自定义缓冲区大小
    public BufferedInputStream(InputStream in, int size) {
        super(in);
        if (size <= 0) {
            throw new IllegalArgumentException("Buffer size <= 0");
        }
        buf = new byte[size];
    }
}
```

字节缓冲输出流

```java
try (BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("output.txt"))) {
    byte[] array = "JavaGuide".getBytes();
    bos.write(array);
} catch (IOException e) {
    e.printStackTrace();
}
```

### 字符缓冲流

`BufferedReader` （字符缓冲输入流）和 `BufferedWriter`（字符缓冲输出流）类似于 `BufferedInputStream`（字节缓冲输入流）和`BufferedOutputStream`（字节缓冲输入流），内部都维护了一个字节数组作为缓冲区。不过，前者主要是用来操作字符信息。

### 打印流

`System.out` 实际是用于获取一个 `PrintStream` 对象，`print`方法实际调用的是 `PrintStream` 对象的 `write` 方法。

`PrintStream` 属于字节打印流，与之对应的是 `PrintWriter` （字符打印流）。`PrintStream` 是 `OutputStream` 的子类，`PrintWriter` 是 `Writer` 的子类。

### 随机访问流

这里要介绍的随机访问流指的是**支持随意跳转到文件的任意位置进行读写**的 `RandomAccessFile` 。

`RandomAccessFile` 的构造方法如下，我们可以指定 `mode`（读写模式）。

```java
// openAndDelete 参数默认为 false 表示打开文件并且这个文件不会被删除
public RandomAccessFile(File file, String mode)
    throws FileNotFoundException {
    this(file, mode, false);
}
// 私有方法
private RandomAccessFile(File file, String mode, boolean openAndDelete)  throws FileNotFoundException{
  // 省略大部分代码
}
```

`RandomAccessFile` 中有一个文件指针用来表示下一个将要被写入或者读取的字节所处的位置。我们可以通过 `RandomAccessFile` 的 `seek(long pos)` 方法来设置文件指针的偏移量（距文件开头 `pos` 个字节处）。如果想要获取文件指针当前的位置的话，可以使用 `getFilePointer()` 方法。

```java
RandomAccessFile randomAccessFile = new RandomAccessFile(new File("input.txt"), "rw");	// 内容ABCDEFG
System.out.println("读取之前的偏移量：" + randomAccessFile.getFilePointer() + ",当前读取到的字符" + (char) randomAccessFile.read() + "，读取之后的偏移量：" + randomAccessFile.getFilePointer());	// 读取之前的偏移量：0,当前读取到的字符A，读取之后的偏移量：1
// 指针当前偏移量为 6
randomAccessFile.seek(6);
System.out.println("读取之前的偏移量：" + randomAccessFile.getFilePointer() + ",当前读取到的字符" + (char) randomAccessFile.read() + "，读取之后的偏移量：" + randomAccessFile.getFilePointer());	// 读取之前的偏移量：6,当前读取到的字符G，读取之后的偏移量：7
// 从偏移量 7 的位置开始往后写入字节数据
randomAccessFile.write(new byte[]{'H', 'I', 'J', 'K'});		// 文件内容变为 ABCDEFGHIJK
// 指针当前偏移量为 0，回到起始位置
randomAccessFile.seek(0);
System.out.println("读取之前的偏移量：" + randomAccessFile.getFilePointer() + ",当前读取到的字符" + (char) randomAccessFile.read() + "，读取之后的偏移量：" + randomAccessFile.getFilePointer());
```

`RandomAccessFile` 比较常见的一个应用就是实现大文件的 **断点续传** 。何谓断点续传？简单来说就是上传文件中途暂停或失败（比如遇到网络问题）之后，不需要重新上传，只需要上传那些未成功上传的文件分片即可。分片（先将文件切分成多个文件分片）上传是断点续传的基础。

## IO模型

**我们的应用程序对操作系统的内核发起 IO 调用（系统调用），操作系统负责的内核执行具体的 IO 操作。也就是说，我们的应用程序实际上只是发起了 IO 操作的调用而已，具体 IO 的执行是由操作系统的内核来完成的。**

当应用程序发起 I/O 调用后，会经历两个步骤：

1. 内核等待 I/O 设备准备好数据
2. 内核将数据从内核空间拷贝到用户空间

### BIO (Blocking I/O)

**BIO 属于同步阻塞 IO 模型** 。

同步阻塞 IO 模型中，应用程序发起 read 调用后，会**一直阻塞，直到内核把数据拷贝到用户空间**。

在客户端连接数量不高的情况下，是没问题的。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。

NIO (Non-blocking/New I/O)
------

Java 中的 NIO 于 Java 1.4 中引入，对应 java.nio 包，提供了 Channel , Selector，Buffer 等抽象。NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它是支持面向缓冲的，基于通道的 I/O 操作方法。 **对于高负载、高并发的（网络）应用，应使用 NIO** 。Java 中的 NIO 可以看作是 I/O 多路复用模型。也有很多人认为，Java 中的 NIO 属于同步非阻塞 IO 模型。

同步非阻塞 IO 模型中，应用程序会一直发起 read 调用，等待数据从内核空间拷贝到用户空间的这段时间里，线程依然是阻塞的，直到在内核把数据拷贝到用户空间。相比于同步阻塞 IO 模型，同步非阻塞 IO 模型确实有了很大改进。通过轮询操作，避免了一直阻塞。

但是，这种 IO 模型同样存在问题：**应用程序不断进行 I/O 系统调用轮询数据是否已经准备好的过程是十分消耗 CPU 资源的。**

IO 多路复用模型中，线程首先发起 select 调用，询问内核数据是否准备就绪，等内核把数据准备好了，用户线程再发起 read 调用。read 调用的过程（数据从内核空间 -> 用户空间）还是阻塞的。**IO 多路复用模型，通过减少无效的系统调用，减少了对 CPU 资源的消耗。**

Java 中的 NIO ，有一个非常重要的**选择器 ( Selector )** 的概念，也可以被称为 **多路复用器**。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。

### AIO (Asynchronous I/O)

AIO 也就是 NIO 2。Java 7 中引入了 NIO 的改进版 NIO 2,它是异步 IO 模型。

异步 IO 是**基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。**

## 语法糖

**语法糖（Syntactic sugar）** 代指的是编程语言为了方便程序员开发程序而设计的一种特殊语法**，这种语法对编程语言的功能并没有影响**。实现相同的功能，基于语法糖写出来的代码往往更简单简洁且更易阅读。

不过，JVM 其实并不能识别语法糖，Java 语法糖要想被正确执行，需要**先通过编译器进行解糖**，也就是在程序**编译阶段将其转换成 JVM 认识的基本语法**。这也侧面说明，Java 中**真正支持语法糖的是 Java 编译器**而不是 JVM。如果你去看com.sun.tools.javac.main.JavaCompiler的源码，你会发现在compile()中有一个步骤就是调用desugar()，这个方法就是负责解语法糖的实现的。

Java 中最常用的语法糖主要有**泛型、自动拆装箱、变长参数、枚举、内部类、增强 for 循环、try-with-resources 语法、lambda 表达式**等。

增强for循环：

```java
for (Student stu : students) {
    if (stu.getId() == 2)
        students.remove(stu);
}
```

会抛出`ConcurrentModificationException`异常。

Iterator 是工作在一个独立的线程中，并且拥有一个 mutex 锁。 Iterator 被创建之后会建立一个指向原来对象的单链索引表，当原来的对象数量发生变化时，这个索引表的内容不会同步改变，所以当索引指针往后移动的时候就找不到要迭代的对象，所以按照 fail-fast 原则 Iterator 会马上抛出`java.util.ConcurrentModificationException`异常。

所以 `Iterator` 在工作的时候是不允许被迭代的对象被改变的。但你可以使用 `Iterator` 本身的方法`remove()`来删除对象，`Iterator.remove()` 方法会在删除当前迭代对象的同时维护索引的一致性。

## 集合

Java 集合，也叫作容器，主要是由两大接口派生而来：一个是 `Collection`接口，主要用于存放单一元素；另一个是 `Map` 接口，主要用于存放键值对。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 、 `Queue`。

<img src="Java\java-collection-hierarchy.png" alt="Java 集合框架概览" style="zoom:80%;" />

- `List`: 存储的元素是有序的、可重复的。
- `Set`: 存储的元素不可重复的。
- `Queue`: 按特定的排队规则来确定先后顺序，存储的元素是有序的、可重复的。
- `Map`: 使用键值对（key-value）存储，key 是无序的、不可重复的，value 是无序的、可重复的，每个键最多映射到一个值。

### List

#### ArrayList

`ArrayList` 的底层是数组队列，相当于**动态数组**。与 Java 中的数组相比，它的**容量能动态增长**。在添加大量元素前，应用程序可以使用`ensureCapacity`操作来增加 `ArrayList` 实例的容量。这可以**减少递增式再分配的数量**。

`ArrayList` 继承于 `AbstractList` ，实现了 `List`, `RandomAccess`, `Cloneable`, `java.io.Serializable` 这些接口。

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable{

  }
```

* `List` : 表明它是一个列表，支持**添加、删除、查找**等操作，并且可以**通过下标进行访问**。

* `RandomAccess` ：这是一个标志接口，表明实现这个接口的 `List` 集合是支持 **快速随机访问** 的。在 `ArrayList` 中，我们即可以通过元素的序号快速获取元素对象，这就是快速随机访问。

* `Cloneable` ：表明它具有拷贝能力，可以进行深拷贝或浅拷贝操作。

* `Serializable` : 表明它可以进行序列化操作，也就是可以将对象转换为字节流进行持久化存储或网络传输，非常方便。

**ArrayList 插入和删除元素的时间复杂度**

对于插入：

- 头部插入：由于需要将所有元素都依次向后移动一个位置，因此时间复杂度是 O(n)。
- 尾部插入：当 `ArrayList` 的容量未达到极限时，往列表末尾插入元素的时间复杂度是 O(1)，因为它只需要在数组末尾添加一个元素即可；当容量已达到极限并且需要**扩容时，则需要执行一次 O(n) 的操作**将原数组复制到新的更大的数组中，然后再执行 O(1) 的操作添加元素。
- 指定位置插入：需要将目标位置之后的所有元素都向后移动一个位置，然后再把新元素放入指定位置。这个过程需要移动平均 n/2 个元素，因此时间复杂度为 O(n)。

对于删除：

- 头部删除：由于需要将所有元素依次向前移动一个位置，因此时间复杂度是 O(n)。
- 尾部删除：当删除的元素位于列表末尾时，时间复杂度为 O(1)。
- 指定位置删除：需要将目标元素之后的所有元素向前移动一个位置以填补被删除的空白位置，因此需要移动平均 n/2 个元素，时间复杂度为 O(n)。

##### 核心机制

初始化时，默认无参构造函数给`elementData`（保存ArrayList数据的数组）赋值`DEFAULTCAPACITY_EMPTY_ELEMENTDATA={}`，也就是一个默认大小0的空实例。在第一次添加数据的时候才会真正分配容量`DEFAULT_CAPACITY = 10`。此后添加第2，3，，，一直到10个元素，`minCapacity - elementData.length > 0`都不成立，也就是现有的Object数组的长度都大于需要的最小数组长度，所以不会扩容。到第11个元素时，进入`grow`方法扩容，新的容量`newCapacity = oldCapacity + (oldCapacity >> 1);`也就是原始大小的1.5倍。

此外，外部方法` ensureCapacity`可以供调用者手动传入` minCapacity`，这个值会在`grow`方法中与`newCapacity`比较， 如果1.5倍的`old `仍然小于需要的`minCapacity`，则更新`newCapacity`为`minCapacity`。

如果新容量大于 `MAX_ARRAY_SIZE`,进入(执行) `hugeCapacity()` 方法来比较 `minCapacity` 和 `MAX_ARRAY_SIZE`，如果 `minCapacity` 大于最大容量，则新容量则为`Integer.MAX_VALUE`，否则，新容量大小则为 `MAX_ARRAY_SIZE` 即为 `Integer.MAX_VALUE - 8`。

#### LinkeadList

`LinkedList` 是一个基于双向链表实现的集合类，经常被拿来和 `ArrayList` 做比较。

<img src="Java\bidirectional-linkedlist.png" alt="双向链表" style="zoom:80%;" />

不过，我们在项目中一般是不会使用到 `LinkedList` 的，需要用到 `LinkedList` 的场景几乎都可以使用 `ArrayList` 来代替，并且，性能通常会更好。

**LinkedList 为什么不能实现 RandomAccess 接口？**

`RandomAccess` 是一个**标记**接口，用来表明**实现该接口的类支持随机访问**（即可以通过索引快速访问元素）。由于 `LinkedList` 底层数据结构是链表，**内存地址不连续，只能通过指针来定位，不支持随机快速访问**，所以不能实现 `RandomAccess` 接口。

`LinkedList` 的类定义如下：

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
  //...
}
```

`LinkedList` 继承了 `AbstractSequentialList` ，而 `AbstractSequentialList` 又继承于 `AbstractList` 。

阅读过 `ArrayList` 的源码我们就知道，`ArrayList` 同样继承了 `AbstractList` ， 所以 `LinkedList` 会有大部分方法和 `ArrayList` 相似。

`LinkedList` 实现了以下接口：

- `List` : 表明它是一个列表，支持添加、删除、查找等操作，并且可以通过下标进行访问。
- `Deque` ：继承自 `Queue` 接口，具有**双端队列**的特性，支持从两端插入和删除元素，方便实现栈和队列等数据结构。
- `Cloneable` ：表明它具有拷贝能力，可以进行深拷贝或浅拷贝操作。
- `Serializable` : 表明它可以进行序列化操作，也就是可以将对象转换为字节流进行持久化存储或网络传输，非常方便。

`LinkedList` 中的元素是通过 `Node` 定义的：

```java
private static class Node<E> {
    E item;// 节点值
    Node<E> next; // 指向的下一个节点（后继节点）
    Node<E> prev; // 指向的前一个节点（前驱结点）

    // 初始化参数顺序分别是：前驱结点、本身节点值、后继节点
    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

`LinkedList` 中有一个无参构造函数和一个有参构造函数。

```java
// 创建一个空的链表对象
public LinkedList() {
}

// 接收一个集合类型作为参数，会创建一个与传入集合相同元素的链表对象
public LinkedList(Collection<? extends E> c) {
    this();
    addAll(c);
}
```

##### 核心机制

在定位第idx个元素时，调用node(index)方法，该方法通过比较索引值与链表 size 的一半大小来确定从链表头还是尾开始遍历。如果索引值小于 size 的一半，就从链表头开始遍历，反之从链表尾开始遍历。这样可以在较短的时间内找到目标节点，充分利用了双向链表的特性来提高效率。

`unlink(x)` 方法的逻辑如下：

1. 首先获取待删除节点 x 的前驱和后继节点；
2. 判断待删除节点是否为头节点或尾节点： 
   - 如果 x 是头节点，则将 first 指向 x 的后继节点 next
   - 如果 x 是尾节点，则将 last 指向 x 的前驱节点 prev
   - 如果 x 不是头节点也不是尾节点，执行下一步操作
3. 将待删除节点 x 的前驱的后继指向待删除节点的后继 next，断开 x 和 x.prev 之间的链接；
4. 将待删除节点 x 的后继的前驱指向待删除节点的前驱 prev，断开 x 和 x.next 之间的链接；
5. 将待删除节点 x 的元素置空，修改链表长度。

#### ArrayList vs LinkedList

* **是否保证线程安全：** `ArrayList` 和 `LinkedList` 都是**不同步的**，也就是不保证线程安全；
  * 当多个线程同时对ArrayList进行修改操作时，可能会导致数据不一致或者出现异常。这是因为ArrayList的内部结构不是线程安全的，它**没有提供对并发修改的支持**。例如，当一个线程正在向ArrayList中添加元素，而另一个线程同时在删除元素，就有可能导致**索引越界或者元素丢失**的问题。
  * 推荐使用并发集合类（例如 `ConcurrentHashMap`、`CopyOnWriteArrayList` 等）或者手动实现线程安全的方法来提供安全的多线程操作支持。

* **底层数据结构：** `ArrayList` 底层使用的是 **`Object` 数组**；`LinkedList` 底层使用的是 **双向链表** 数据结构（JDK1.6 之前为循环链表，JDK1.7 取消了循环。注意双向链表和双向循环链表的区别）

* **插入和删除是否受元素位置的影响：**
  * `ArrayList` 采用**数组**存储，所以插入和删除元素的时间复杂度**受元素位置的影响**。 比如：执行`add(E e)`方法的时候， `ArrayList` 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`），时间复杂度就为 O(n)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。
  * `LinkedList` 采用**链表**存储，所以在头尾插入或者删除元素**不受元素位置的影响**（`add(E e)`、`addFirst(E e)`、`addLast(E e)`、`removeFirst()`、 `removeLast()`），时间复杂度为 O(1)，如果是要在指定位置 `i` 插入和删除元素的话（`add(int index, E element)`，`remove(Object o)`,`remove(int index)`）， 时间复杂度为 O(n) ，因为需要**先移动到指定位置**再插入和删除。
  * 总结：ArrayList查询O(1)，开头或指定位置插入删除O(n)。LinkedList查询O(n)，插入删除自身操作O(1)，所以在中间特定位置插入删除整体O(n)

* **是否支持快速随机访问：** `LinkedList` 不支持高效的随机元素访问，而 `ArrayList`（实现了 `RandomAccess` 接口） 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)。
  * LinkedList是双向链表，不能根据下标直接取元素；ArrayList是动态数组，所以支持快速随机访问。

* **内存空间占用：** `ArrayList` 的空间浪费主要体现在在 **list 列表的结尾会预留一定的容量空间**，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为**要存放直接后继和直接前驱**以及数据）。

#### CopyOnWriteArrayList

在 JDK1.5 之前，如果想要使用**并发安全**的 `List` 只能选择 `Vector`。而 `Vector` 是一种老旧的集合，已经被淘汰。`Vector` 对于增删改查等方法基本都加了 **`synchronized`**，这种方式虽然能够保证同步，但这相当于对整个 `Vector` 加上了一把大锁，使得**每个方法执行的时候都要去获得锁，导致性能非常低下**。

JDK1.5 引入了 `Java.util.concurrent`（JUC）包，其中提供了很多线程安全且并发性能良好的容器，其中唯一的线程安全 `List` 实现就是 `CopyOnWriteArrayList` 。

> 对于大部分业务场景来说，读取操作往往是远大于写入操作的。由于读取操作不会对原有数据进行修改，因此，对于每次读取都进行加锁其实是一种资源浪费。相比之下，我们应该**允许多个线程同时访问 `List` 的内部数据**，毕竟对于读取操作来说是安全的。

为了将读操作性能发挥到极致，`CopyOnWriteArrayList` 中的**读取操作是完全无需加锁的**。**写入操作也不会阻塞读取操作**，只有写写才会互斥。这样一来，读操作的性能就可以大幅度提升。

`CopyOnWriteArrayList`名字中的“Copy-On-Write”即写时复制，简称 COW，是线程安全的核心。

> 写入时复制（英语：Copy-on-write，简称 COW）是一种计算机程序设计领域的优化策略。其核心思想是，如果有多个调用者（callers）同时请求相同资源（如内存或磁盘上的数据存储），他们会共同**获取相同的指针指向相同的资源**，直到某个调用者试图**修改资源**的内容时，**系统才会真正复制一份专用副本（private copy）给该调用者**，而其他调用者所见到的最初的资源仍然保持不变。这过程对其他的调用者都是透明的。此作法主要的优点是如果调用者没有修改该资源，就不会有副本（private copy）被创建，因此多个调用者只是读取操作时可以共享同一份资源。

当需要修改（ `add`，`set`、`remove` 等操作） `CopyOnWriteArrayList` 的内容时，不会直接修改原数组，而是会先创建底层数组的副本，对副本数组进行修改，修改完之后再将修改后的数组赋值给底层数组的引用，替换掉旧的数组，这样就可以保证写操作不会影响读操作了。写时复制机制非常**适合读多写少**的并发场景，能够极大地提高系统的并发性能。

**缺点：**

* 内存占用：每次写操作都需要复制一份原始数据，会占用额外的内存空间，在数据量比较大的情况下，可能会导致内存资源不足。
* 写操作开销：每一次写操作都需要复制一份原始数据，然后再进行修改和替换，所以写操作的开销相对较大，在写入比较频繁的场景下，性能可能会受到影响。
* 数据一致性问题：修改操作不会立即反映到最终结果中，还需要等待复制完成，这可能会导致一定的数据一致性问题。

##### 核心机制

```java
public class CopyOnWriteArrayList<E>
extends Object
implements List<E>, RandomAccess, Cloneable, Serializable
{
  //...
}
```

实现list，randomaccess，cloneable，serializable，和arraylist一样

`CopyOnWriteArrayList` 的 `add()`方法有三个版本：

- `add(E e)`：在 `CopyOnWriteArrayList` 的尾部插入元素。
- `add(int index, E element)`：在 `CopyOnWriteArrayList` 的指定位置插入元素。
- `addIfAbsent(E e)`：如果指定元素不存在，那么添加该元素。如果成功添加元素则返回 true。

这里以`add(E e)`为例进行介绍：

```java
// 插入元素到 CopyOnWriteArrayList 的尾部
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    // 加锁
    lock.lock();
    try {
        // 获取原来的数组
        Object[] elements = getArray();
        // 原来数组的长度
        int len = elements.length;
        // 创建一个长度+1的新数组，并将原来数组的元素复制给新数组
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        // 元素放在新数组末尾
        newElements[len] = e;
        // array指向新数组
        setArray(newElements);
        return true;
    } finally {
        // 解锁
        lock.unlock();
    }
}
```

* `add`方法内部用到了 `ReentrantLock` 加锁，保证了同步，避免了多线程写的时候会复制出多个副本出来。锁被`final`修饰保证了锁的内存地址肯定不会被修改，并且，释放锁的逻辑放在 `finally` 中，可以保证锁能被释放。

- `CopyOnWriteArrayList` 中并没有类似于 `ArrayList` 的 `grow()` 方法扩容的操作。

**读取元素**：`CopyOnWriteArrayList` 的读取操作是基于内部数组 `array` 并没有发生实际的修改，因此在读取操作时不需要进行同步控制和锁操作，可以保证数据的安全性。这种机制下，多个线程可以同时读取列表中的元素。不过，`get`方法是弱一致性的，**在某些情况下可能读到旧的元素值。**（比如，线程1读数据，线程2写数据，线程1取值，此时取值就是旧的值）



### Set

**Comparable 和 Comparator 的区别**

`Comparable` 接口和 `Comparator` 接口都是 Java 中用于排序的接口，它们在实现类对象之间比较大小、排序等方面发挥了重要作用：

- `Comparable` 接口实际上是出自`java.lang`包 它有一个 `compareTo(Object obj)`方法用来排序
- `Comparator`接口实际上是出自 `java.util` 包它有一个`compare(Object obj1, Object obj2)`方法用来排序

一般我们需要对一个集合使用自定义排序时，我们就要重写`compareTo()`方法或`compare()`方法，当我们需要对某一个集合实现两种排序方式，比如一个 `song` 对象中的歌名和歌手名分别采用一种排序方法的话，我们可以重写`compareTo()`方法和使用自制的`Comparator`方法或者以两个 `Comparator` 来实现歌名排序和歌星名排序，第二种代表我们只能使用两个参数版的 `Collections.sort()`

```java
// void sort(List list),按自然排序的升序排序
Collections.sort(arrayList);
System.out.println("Collections.sort(arrayList):");
System.out.println(arrayList);
// 定制排序的用法
Collections.sort(arrayList, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o2.compareTo(o1);
    }
});

// person对象没有实现Comparable接口，所以必须实现，这样才不会出错，才可以使treemap中的数据按顺序排列
// 前面一个例子的String类已经默认实现了Comparable接口，详细可以查看String类的API文档，另外其他
// 像Integer类等都已经实现了Comparable接口，所以不需要另外实现了
public  class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(String name, int age) {
        super();
        this.name = name;
        this.age = age;
    }

    // set, get methods

    /**
     * T重写compareTo方法实现按年龄来排序
     */
    @Override
    public int compareTo(Person o) {
        if (this.age > o.getAge()) {
            return 1;
        }
        if (this.age < o.getAge()) {
            return -1;
        }
        return 0;
    }
}
```

#### **无序性和不可重复性**

- 无序性不等于随机性 ，无序性是指**存储的数据在底层数组中**并非按照数组索引的顺序添加 ，而是根**据数据的哈希值决定**的。
  - 所以HashSet/HashMap是无序的，而LinkedHashSet通过链表维护了插入和取出的顺序，是有序的
- 不可重复性是指添加的元素按照 `equals()` 判断时 ，返回 false，需要同时重写 `equals()` 方法和 `hashCode()` 方法。

#### HashSet、LinkedHashSet 和 TreeSet

- `HashSet`、`LinkedHashSet` 和 `TreeSet` 都是 `Set` 接口的实现类，都能保证元素唯一，并且都不是线程安全的。
  - 不安全的原因是因为HashMap不是线程安全的。在HashSet中，底层源码，其实就是一个HashMap，HashMap的key为HashSet中的值，而value为一个Object对象常量。
- `HashSet`、`LinkedHashSet` 和 `TreeSet` 的主要区别在于底层数据结构不同。`HashSet` 的底层数据结构是哈希表（基于 `HashMap` 实现）。`LinkedHashSet` 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 FIFO。`TreeSet` 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
- 底层数据结构不同又导致这三者的应用场景不同。`HashSet` 用于不需要保证元素插入和取出顺序的场景，`LinkedHashSet` 用于保证元素的插入和取出顺序满足 FIFO 的场景，`TreeSet` 用于支持对元素自定义排序规则的场景。

**HashSet如何检查重复**

> 当你把对象加入`HashSet`时，`HashSet` 会先计算对象的`hashcode`值来判断对象加入的位置，同时也会与其他加入的对象的 `hashcode` 值作比较，如果没有相符的 `hashcode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashcode` 值的对象，这时会调用`equals()`方法来检查 `hashcode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让加入操作成功。

在 JDK1.8 中，实际上无论`HashSet`中是否已经存在了某元素，`HashSet`都会直接插入，只是会在`add()`方法的返回值处告诉我们插入前是否存在相同元素。

### Queue

#### ArrayDeque 与 LinkedList

`ArrayDeque` 和 `LinkedList` 都实现了 `Deque` 接口，两者都具有队列的功能，但两者有什么区别呢？

- `ArrayDeque` 是基于**可变长的数组和双指针**来实现，而 `LinkedList` 则通过**链表**来实现。
- `ArrayDeque` 不支持存储 `NULL` 数据，**但 `LinkedList` 支持。**
- `ArrayDeque` 是在 JDK1.6 才被引入的，而`LinkedList` 早在 JDK1.2 时就已经存在。
- `ArrayDeque` 插入时可能存在扩容过程, 不过**均摊后的插入操作依然为 O(1)**。虽然 `LinkedList` 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。
- 从性能的角度上，选用 `ArrayDeque` 来实现队列要比 `LinkedList` 更好。此外，`ArrayDeque` 也可以用于实现栈。

#### PriorityQueue

`PriorityQueue` 是在 JDK1.5 中被引入的, 其与 `Queue` 的区别在于元素出队顺序是与优先级相关的，即总是**优先级最高的元素先出队**。

这里列举其相关的一些要点：

- `PriorityQueue` 利用了**二叉堆**的数据结构来实现的，底层使用**可变长的数组**来存储数据
- `PriorityQueue` 通过堆元素的上浮和下沉，实现了在 **O(logn)** 的时间复杂度内插入元素和删除堆顶元素。
- `PriorityQueue` 是**非线程安全**的，且不支持存储 `NULL` 和 `non-comparable` 的对象。
- `PriorityQueue` 默认是小顶堆，但可以接收一个 `Comparator` 作为构造参数，从而来自定义元素优先级的先后。

#### BlockingQueue

`BlockingQueue` （阻塞队列）是一个**接口**，继承自 `Queue`。`BlockingQueue`阻塞的原因是其支持当队列没有元素时一直阻塞，直到有元素；还支持如果队列已满，一直等到队列可以放入新元素时再放入。

`BlockingQueue` 常用于生产者-消费者模型中，生产者线程会向队列中添加数据，而消费者线程会从队列中取出数据进行处理。

**实现类：**

Java 中常用的阻塞队列实现类有以下几种：

1. `ArrayBlockingQueue`：使用**数组**实现的有界阻塞队列。**在创建时需要指定容量大小**，并支持公平和非公平两种方式的锁访问机制。
2. `LinkedBlockingQueue`：使用**单向链表**实现的可选有界阻塞队列。在创建时可以指定容量大小，如果不指定则默认为`Integer.MAX_VALUE`。和`ArrayBlockingQueue`不同的是， 它仅支持**非公平**的锁访问机制。
3. `PriorityBlockingQueue`：支持优先级排序的**无界**阻塞队列。元素必须实现`Comparable`接口或者在构造函数中传入`Comparator`对象，并且不能插入 null 元素。
4. `SynchronousQueue`：**同步队列**，是一种不存储元素的阻塞队列。每个插入操作都必须等待对应的删除操作，反之删除操作也必须等待插入操作。因此，`SynchronousQueue`通常用于线程之间的直接传递数据。
5. `DelayQueue`：**延迟队列**，其中的元素只有到了其指定的延迟时间，才能够从队列中出队。

#### ArrayBlockingQueue 和 LinkedBlockingQueue

`ArrayBlockingQueue` 和 `LinkedBlockingQueue` 是 Java 并发包中常用的两种阻塞队列实现，它们都是**线程安全**的。它们之间存在下面这些区别：

- **底层实现**：`ArrayBlockingQueue` 基于数组实现，而 `LinkedBlockingQueue` 基于链表实现。
- **是否有界**：`ArrayBlockingQueue` 是有界队列，必须在创建时指定容量大小。`LinkedBlockingQueue` 创建时可以不指定容量大小，默认是`Integer.MAX_VALUE`，也就是无界的。但也**可以指定**队列大小，从而成为有界的。
- **锁是否分离**： `ArrayBlockingQueue`中的锁是没有分离的，即**生产和消费用的是同一个锁**；`LinkedBlockingQueue`中的锁是分离的，即**生产用的是`putLock`，消费是`takeLock`**，这样可以防止生产者和消费者线程之间的锁争夺。
- **内存占用**：`ArrayBlockingQueue` 需要提前分配数组内存，而 `LinkedBlockingQueue` 则是动态分配链表节点内存。这意味着，`ArrayBlockingQueue` 在创建时就会占用一定的内存空间，且往往申请的内存比实际所用的内存更大，而`LinkedBlockingQueue` 则是根据元素的增加而逐渐占用内存空间。

### Map

#### HashMap

HashMap 主要用来存放键值对，它基于哈希表的 Map 接口实现，是常用的 Java 集合之一，是非线程安全的。

**JDK1.8 之前**

JDK1.8 之前 `HashMap` 底层是 **数组和链表** 结合在一起使用也就是 **链表散列**。HashMap 通过 key 的 `hashcode` 经过扰动函数处理过后得到 hash 值，然后通过 `(n - 1) & hash` 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

所谓扰动函数指的就是 HashMap 的 `hash` 方法。使用 `hash` 方法也就是扰动函数是为了防止一些实现比较差的 `hashCode()` 方法 换句话说使用扰动函数之后可以减少碰撞。

**JDK 1.8 HashMap 的 hash 方法源码:**

JDK 1.8 的 hash 方法 相比于 JDK 1.7 hash 方法更加简化，但是原理不变。

**JDK1.8 之后**

相比于之前的版本， JDK1.8 之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，**如果当前数组的长度小于 64，那么会选择先进行数组扩容**，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。

<img src="Java\jdk1.8_hashmap.png" alt="jdk1.8之后的内部结构-HashMap" style="zoom:80%;" />

> TreeMap、TreeSet 以及 JDK1.8 之后的 HashMap 底层都用到了红黑树。红黑树就是为了解决二叉查找树的缺陷，因为二叉查找树在某些情况下会退化成一个线性结构。

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {
    // 序列号
    private static final long serialVersionUID = 362498820763181265L;
    // 默认的初始容量是16
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
    // 最大容量
    static final int MAXIMUM_CAPACITY = 1 << 30;
    // 默认的负载因子
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
    // 当桶(bucket)上的结点数大于等于这个值时会转成红黑树
    static final int TREEIFY_THRESHOLD = 8;
    // 当桶(bucket)上的结点数小于等于这个值时树转链表
    static final int UNTREEIFY_THRESHOLD = 6;
    // 桶中结构转化为红黑树对应的table的最小容量
    static final int MIN_TREEIFY_CAPACITY = 64;
    // =====存储元素的数组，总是2的幂次倍=====
    transient Node<k,v>[] table;
    // 一个包含了映射中所有键值对的集合视图
    transient Set<map.entry<k,v>> entrySet;
    // 存放元素的个数，注意这个不等于数组的长度。
    transient int size;
    // 每次扩容和更改map结构的计数器
    transient int modCount;
    // 阈值(容量*负载因子) 当实际大小超过阈值时，会进行扩容
    int threshold;
    // 负载因子
    final float loadFactor;
}

// Node节点类，继承自 Map.Entry<K,V>
static class Node<K,V> implements Map.Entry<K,V> {
       final int hash;// 哈希值，存放元素到hashmap中时用来与其他元素hash值比较
       final K key;//键
       V value;//值
       // 指向下一个节点->链式结构
       Node<K,V> next;
       Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }
        // 重写hashCode()方法
        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }
        // 重写 equals() 方法
        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
}

// 树节点类 -- 红黑树
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // 父
        TreeNode<K,V> left;    // 左
        TreeNode<K,V> right;   // 右
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;           // 判断颜色
        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }
        // 返回根节点
        final TreeNode<K,V> root() {
            for (TreeNode<K,V> r = this, p;;) {
                if ((p = r.parent) == null)
                    return r;
                r = p;
       }
```

* **loadFactor 负载因子**
  * loadFactor 负载因子是控制**数组存放数据的疏密程度**，loadFactor 越趋近于 1，那么 数组中能存放的数据(entry)也就越多（要达到临界值**threshold = capacity \* loadFactor**的时候才会扩容），也就越密，也就是会让链表的长度增加（因为要很久才扩容，这段数组本身很密，冲突的数据也多，链表就长），loadFactor 越小，也就是趋近于 0，数组中存放的数据(entry)也就越少，也就越稀疏。
  * **loadFactor 太大导致查找元素效率低，太小导致数组的利用率低，存放的数据会很分散。loadFactor 的默认值为 0.75f 是官方给出的一个比较好的临界值**。
  * 给定的默认容量为 16，负载因子为 0.75。Map 在使用过程中不断的往里面存放数据，当数量超过了 16 * 0.75 = 12 就需要将当前 16 的容量进行扩容，而扩容这个过程涉及到 rehash、复制数据等操作，所以非常消耗性能。

##### 核心机制

put方法插入元素：如果定位到的数组位置没有元素 就直接插入。

如果定位到的数组位置有元素就和要插入的 key 比较，如果 key 相同就直接覆盖，如果 key 不相同，就判断 p 是否是一个树节点，如果是就调用`e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value)`将元素添加进入。如果不是就遍历链表插入(插入的是链表尾部)。

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // table未初始化或者长度为0，进行扩容
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    // (n - 1) & hash 确定元素存放在哪个桶中，桶为空，新生成结点放入桶中(此时，这个结点是放在数组中)
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    // 桶中已经存在元素（处理hash冲突）
    else {
        Node<K,V> e; K k;
        //快速判断第一个节点table[i]的key是否与插入的key一样，若相同就直接使用插入的值p替换掉旧的值e。
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
        // 判断插入的是否是红黑树节点
        else if (p instanceof TreeNode)
            // 放入树中
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        // 不是红黑树节点则说明为链表结点
        else {
            // 在链表最末插入结点
            for (int binCount = 0; ; ++binCount) {
                // 到达链表的尾部
                if ((e = p.next) == null) {
                    // 在尾部插入新结点
                    p.next = newNode(hash, key, value, null);
                    // 结点数量达到阈值(默认为 8 )，执行 treeifyBin 方法
                    // 这个方法会根据 HashMap 数组来决定是否转换为红黑树。
                    // 只有当数组长度大于或者等于 64 的情况下，才会执行转换红黑树操作，以减少搜索时间。否则，就是只是对数组扩容。
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    // 跳出循环
                    break;
                }
                // 判断链表中结点的key值与插入的元素的key值是否相等
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    // 相等，跳出循环
                    break;
                // 用于遍历桶中的链表，与前面的e = p.next组合，可以遍历链表
                p = e;
            }
        }
        // 表示在桶中找到key值、hash值与插入元素相等的结点
        if (e != null) {
            // 记录e的value
            V oldValue = e.value;
            // onlyIfAbsent为false或者旧值为null
            if (!onlyIfAbsent || oldValue == null)
                //用新值替换旧值
                e.value = value;
            // 访问后回调
            afterNodeAccess(e);
            // 返回旧值
            return oldValue;
        }
    }
    // 结构性修改
    ++modCount;
    // 实际大小大于阈值则扩容
    if (++size > threshold)
        resize();
    // 插入后回调
    afterNodeInsertion(evict);
    return null;
}
```

**resize 方法**

进行扩容，会伴随着一次重新 hash 分配，并且会遍历 hash 表中所有的元素，是非常耗时的。在编写程序中，要尽量避免 resize。resize 方法实际上是将 table 初始化和 table 扩容 进行了整合，底层的行为都是给 table 赋值一个新的数组。

**HashMap 为什么线程不安全**

JDK1.7 及之前版本，在多线程环境下，`HashMap` 扩容时会造成**死循环和数据丢失**的问题。

数据丢失这个在 JDK1.7 和 JDK 1.8 中都存在，这里以 JDK 1.8 为例进行介绍。

JDK 1.8 后，在 `HashMap` 中，多个键值对可能会被分配到同一个桶（bucket），并以链表或红黑树的形式存储。多个线程对 `HashMap` 的 `put` 操作会导致线程不安全，具体来说会有**数据覆盖**的风险。

举个例子：

- 两个线程 1,2 同时进行 put 操作，并且发生了哈希冲突（hash 函数计算出的插入下标是相同的）。
- 不同的线程可能在不同的时间片获得 CPU 执行的机会，当前线程 1 执行完哈希冲突判断后，由于时间片耗尽挂起。线程 2 先完成了插入操作。
- 随后，线程 1 获得时间片，由于之前已经进行过 hash 碰撞的判断，所有此时会直接进行插入，这就导致线程 2 插入的数据被线程 1 覆盖了。

#### HashMap vs HashTable

* **线程是否安全：** **`HashMap` 是非线程安全的**，`Hashtable` 是线程安全的,因为 `Hashtable` 内部的方法基本都经过`synchronized` 修饰。（如果你要保证线程安全的话就使用 `ConcurrentHashMap` 吧！）；

* **效率：** 因为线程安全的问题，`HashMap` 要比 `Hashtable` 效率高一点。另外，`Hashtable` 基本被淘汰，不要在代码中使用它；

* **对 Null key 和 Null value 的支持：** **`HashMap` 可以存储 null 的 key 和 value**，但 null 作为键只能有一个，null 作为值可以有多个；Hashtable 不允许有 null 键和 null 值，否则会抛出 `NullPointerException`。（ConcurrentHashMap也不支持存储null）

* **初始容量大小和每次扩充容量大小的不同：** ① 创建时如果不指定容量初始值，`Hashtable` 默认的初始大小为 11，之后每次扩充，容量变为原来的 2n+1。**`HashMap` 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。**② 创建时如果给定了容量初始值，那么 `Hashtable` 会直接使用你给定的大小，而 `HashMap` 会将其**扩充为 2 的幂次方大小**（`HashMap` 中的`tableSizeFor()`方法保证，下面给出了源代码）。也就是说=== **`HashMap` 总是使用 2 的幂作为哈希表的大小**==。
  * Hash函数的算法设计：**取余**(%)操作中如果除数是 2 的幂次则**等价于**与其除数减一的与(&)操作（也就是说 `hash%length==hash&(length-1)`的前提是 length 是 2 的 n 次方；）。并且 采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 HashMap 的长度为什么是 2 的幂次方。

* **底层数据结构：** JDK1.8 以后的 `HashMap` 在解决哈希冲突时有了较大的变化，**当链表长度大于阈值（默认为 8）时，将链表转化为红黑树**（将链表转换成红黑树前会判断，**如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树**），以减少搜索时间。`Hashtable` 没有这样的机制。

#### HashMap vs TreeMap

`TreeMap` 和`HashMap` 都继承自`AbstractMap` ，但是需要注意的是`TreeMap`它还实现了`NavigableMap`接口和`SortedMap` 接口。

`NavigableMap` 接口提供了丰富的方法来探索和操作键值对，可以对集合元素进行**搜索**:

1. **定向搜索**: `ceilingEntry()`, `floorEntry()`, `higherEntry()`和 `lowerEntry()` 等方法可以用于定位**大于、小于、大于等于、小于等于给定键**的最接近的键值对。
2. **子集操作**: `subMap()`, `headMap()`和 `tailMap()` 方法可以高效地创建原集合的子集视图，而无需复制整个集合。
3. **逆序视图**:`descendingMap()` 方法返回一个逆序的 `NavigableMap` 视图，使得可以反向迭代整个 `TreeMap`。
4. **边界操作**: `firstEntry()`, `lastEntry()`, `pollFirstEntry()`和 `pollLastEntry()` 等方法可以方便地访问和移除元素。

这些方法都是基于**红黑树**数据结构的属性实现的，红黑树保持平衡状态，从而保证了搜索操作的时间复杂度为 O(log n)，这让 `TreeMap` 成为了处理**有序集合搜索问题**的强大工具。

实现`SortedMap`接口让 `TreeMap` 有了对集合中的元素根据键排序的能力。默认是按 key 的升序排序，不过我们也可以指定排序的比较器。

**综上，相比于`HashMap`来说， `TreeMap` 主要多了对集合中的元素根据键排序的能力以及对集合内元素的搜索的能力**

#### ConcurrentHashMap

##### 1.7版本

1.7版本：Java 7 中 `ConcurrentHashMap` 的存储结构如上图，`ConcurrnetHashMap` 由很多个 `Segment` 组合，而每一个 `Segment` 是一个类似于 `HashMap` 的结构，所以**每一个 `HashMap` 的内部可以进行扩容**。但是 `Segment` 的个数一旦**初始化就不能改变**，默认 `Segment` 的个数是 16 个，你也可以认为 `ConcurrentHashMap` **默认支持最多 16 个线程并发。**

<img src="Java\java7_concurrenthashmap.png" alt="Java 7 ConcurrentHashMap 存储结构" style="zoom:80%;" />

在 Java 7 中 ConcurrentHashMap 的**初始化**逻辑。

1. 必要参数校验。
2. 校验并发级别 `concurrencyLevel` 大小，如果大于最大值，重置为最大值。无参构造**默认值是 16.**
3. 寻找并发级别 `concurrencyLevel` 之上最近的 **2 的幂次方**值，作为初始化容量大小，**默认是 16**。
4. 记录 `segmentShift` 偏移量，这个值为【容量 = 2 的 N 次方】中的 N，在后面 Put 时计算位置时会用到。**默认是 32 - sshift = 28**.
5. 记录 `segmentMask`，默认是 ssize - 1 = 16 -1 = 15.
6. **初始化 `segments[0]`**，**默认大小为 2**，**负载因子 0.75**，**扩容阀值是 2\*0.75=1.5**，插入第二个值时才会进行扩容。

**========put========**

`ConcurrentHashMap` 在**put **一个数据时的处理流程：

1. 计算要 put 的 key 的位置，获取指定位置的 `Segment`。

2. 如果指定位置的 `Segment` 为空，则初始化这个 `Segment`.

   **初始化 Segment 流程：**

   1. 检查计算得到的位置的 `Segment` 是否为 null.
   2. 为 null 继续初始化，使用 `Segment[0]` 的容量和负载因子创建一个 `HashEntry` 数组。
   3. 再次检查计算得到的指定位置的 `Segment` 是否为 null.
   4. 使用创建的 `HashEntry` 数组初始化这个 Segment.
   5. 自旋判断计算得到的指定位置的 `Segment` 是否为 null，使用 CAS 在这个位置赋值为 `Segment`

3. **`Segment.put` 插入 key,value 值。**

   由于 `Segment` 继承了 `ReentrantLock`，所以 `Segment` 内部可以很方便的获取锁，put 流程就用到了这个功能。

   1. `tryLock()` 获取锁，获取不到使用 **`scanAndLockForPut`** 方法继续获取。

   2. 计算 put 的数据要放入的 index 位置，然后获取这个位置上的 `HashEntry` 。

   3. 遍历 put 新元素，为什么要遍历？因为这里获取的 `HashEntry` 可能是一个空元素，也可能是链表已存在，所以要区别对待。

      如果这个位置上的 **`HashEntry` 不存在**：

      1. 如果当前容量大于扩容阀值，小于最大容量，**进行扩容**。
      2. 直接头插法插入。

      如果这个位置上的 **`HashEntry` 存在**：

      1. 判断链表当前元素 key 和 hash 值是否和要 put 的 key 和 hash 值一致。一致则替换值
      2. 不一致，获取链表下一个节点，直到发现相同进行值替换，或者链表表里完毕没有相同的。 
         1. 如果当前容量大于扩容阀值，小于最大容量，**进行扩容**。
         2. 直接链表头插法插入。

   4. 如果要插入的位置之前已经存在，替换后返回旧值，否则返回 null.


**========扩容rehash========**

`ConcurrentHashMap` 的扩容只会扩容到原来的两倍。老数组里的数据移动到新的数组时，位置要么不变，要么变为 `index+ oldSize`，参数里的 node 会在扩容之后使用链表**头插法**插入到指定位置。

**===========get=========**

1. 计算得到 key 的存放的segment的对应HashEntry数组位置。
2. 遍历指定位置的链表查找相同 key 的 value 值。

##### 1.8版本

可以发现 Java8 的 ConcurrentHashMap 相对于 Java7 来说变化比较大，不再是之前的 **Segment 数组 + HashEntry 数组 + 链表**，而是 **Node 数组 + 链表 / 红黑树**。当冲突链表达到一定长度时，链表会转换成红黑树。

<img src="Java\java8_concurrenthashmap.png" alt="Java8 ConcurrentHashMap 存储结构（图片来自 javadoop）" style="zoom: 80%;" />

**==========初始化==========**

`ConcurrentHashMap` 的初始化是通过**自旋和 CAS** 操作完成的。里面需要注意的是变量 `sizeCtl` （sizeControl 的缩写），它的值决定着当前的初始化状态。

1. -1 说明正在初始化，其他线程需要**自旋等待**
2. -N 说明 table 正在进行扩容，高 16 位表示扩容的标识戳，低 16 位减 1 为正在进行扩容的线程数
3. 0 表示 table 初始化大小，如果 table 没有初始化
4. \>0 表示 table 扩容的阈值，如果 table 已经初始化。

```java
/**
 * Initializes table, using the size recorded in sizeCtl.
 */
private final Node<K,V>[] initTable() {
    Node<K,V>[] tab; int sc;
    while ((tab = table) == null || tab.length == 0) {
        //　如果 sizeCtl < 0 ,说明另外的线程执行CAS 成功，正在进行初始化。
        if ((sc = sizeCtl) < 0)
            // 让出 CPU 使用权，自旋等待
            Thread.yield(); // lost initialization race; just spin
        else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
            try {
                if ((tab = table) == null || tab.length == 0) {
                    int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                    @SuppressWarnings("unchecked")
                    Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                    table = tab = nt;
                    sc = n - (n >>> 2);
                }
            } finally {
                sizeCtl = sc;
            }
            break;
        }
    }
    return tab;
}
```

**==========put===========**

1. 根据 key 计算出 hashcode 。
2. 判断是否需要进行初始化。
3. 即为当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。
4. 如果当前位置的 `hashcode == MOVED == -1`,则需要进行扩容。
5. 如果都不满足(桶里有数据，数组不需要扩容），则利用 synchronized 锁写入数据，写入时判断结构是链表还是红黑树，执行对应的插入操作。
6. 如果数量大于 `TREEIFY_THRESHOLD` 则要执行树化方法，在 `treeifyBin` 中会首先判断当前数组长度 ≥64 时才会将链表转换为红黑树。

**==========get===========**

1. 根据 hash 值计算node数组位置。
2. 查找到指定位置，如果头节点就是要找的，直接返回它的 value.
3. 如果头节点 hash 值小于 0 ，说明正在扩容或者是红黑树，使用find查找。
4. 如果是链表，遍历查找之。

##### 线程安全实现

* JDK1.8之前：首先将数据分为一段一段（这个“段”就是 `Segment`）的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。`ConcurrentHashMap` 是由 `Segment` 数组结构和 `HashEntry` 数组结构组成。**`Segment` 继承了 `ReentrantLock`,所以 `Segment` 是一种可重入锁**，扮演锁的角色。`HashEntry` 用于存储键值对数据。对同一 `Segment` 的并发写入会被阻塞，不同 `Segment` 的写入是可以并发执行的。

* JDK1.8之后：`ConcurrentHashMap` 取消了 `Segment` 分段锁，采用 **`Node + CAS + synchronized`** 来保证并发安全。数据结构跟 `HashMap` 1.8 的结构类似，数组+链表/红黑二叉树。Java 8 在链表长度超过一定阈值（8）时将链表（寻址时间复杂度为 O(N)）转换为红黑树（寻址时间复杂度为 O(log(N))）。

  Java 8 中，锁粒度更细，`synchronized` **只锁定当前链表或红黑二叉树的首节点**，这样**只要 hash 不冲突，就不会产生并发**，就不会影响其他 Node 的读写，效率大幅提升。

总结：1.7中使用segment分段锁，锁范围较大，最大并发数为segment数量，默认是16。1.8中使用Node+CAS+synchronized，只锁定链表或红黑树的头节点，锁粒度更细，最大并发数是node数组的大小。

#### ConcurrentHashMap vs Hashtable

`ConcurrentHashMap` 和 `Hashtable` 的区别主要体现在实现线程安全的方式上不同。

- **底层数据结构：** JDK1.7 的 `ConcurrentHashMap` 底层采用 **分段的数组+链表** 实现，JDK1.8 采用的数据结构跟 `HashMap1.8` 的结构一样，**数组+链表/红黑二叉树**。`Hashtable` 和 JDK1.8 之前的 `HashMap` 的底层数据结构类似都是采用 **数组+链表** 的形式，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的；

- **实现线程安全的方式**（重要）：

  - 在 JDK1.7 的时候，`ConcurrentHashMap` 对整个桶数组进行了分割**分段**(`Segment`，分段锁)，**每一把锁只锁容器其中一部分数据**，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。

  - 到了 JDK1.8 的时候，`ConcurrentHashMap` 已经摒弃了 `Segment` 的概念，而是**直接用 `Node` 数组+链表+红黑树**的数据结构来实现，并发控制使用 **`synchronized` 和 CAS** 来操作。（JDK1.6 以后 `synchronized` 锁做了很多优化） 整个看起来就像是**优化过且线程安全的 `HashMap`**，虽然在 JDK1.8 中还能看到 `Segment` 的数据结构，但是已经简化了属性，只是为了兼容旧版本；

  - **Hashtable(同一把锁) **:使用 `synchronized` 来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入**阻塞或轮询**状态，如使用 put 添加元素，**另一个线程不能使用 put 添加元素，也不能使用 get**，竞争会越来越激烈效率越低。

<img src="Java\image-20240619175017147.png" alt="image-20240619175017147" style="zoom:80%;" />

#### LinkedHashMap

`LinkedHashMap` 是 Java 提供的一个集合类，它继承自 `HashMap`，并在 `HashMap` 基础上维护一条**双向链表**，使得具备如下特性:

1. 支持遍历时会**按照插入顺序**有序进行迭代。--`LinkedHashMap` 内部维护了一个双向链表，用于记录元素的插入顺序。因此，当使用迭代器迭代元素时，元素的顺序与它们最初插入的顺序相同。
2. 支持按照元素**访问**顺序**排序**,适用于**封装 LRU 缓存**工具。--`LinkedHashMap` 可以通过构造函数中的 `accessOrder` 参数指定按照访问顺序迭代元素。当 `accessOrder` 为 true 时，每次访问一个元素时，该元素会被移动到链表的末尾，因此下次访问该元素时，它就会成为链表中的最后一个元素，从而实现按照访问顺序迭代元素。
3. 因为内部使用双向链表维护各个节点，所以遍历时的效率和元素个数成正比，相较于和容量成正比的 HashMap 来说，迭代效率会高很多。

`LinkedHashMap` 逻辑结构如下图所示，它是在 `HashMap` 基础上在各个节点之间维护一条双向链表，使得原本散列在不同 bucket 上的节点、链表、红黑树有序关联起来。

<img src="Java\linkhashmap-structure-overview.png" alt="LinkedHashMap 逻辑结构" style="zoom:67%;" />

##### 核心机制

* `LinkedHashMap` 的**节点内部类 `Entry`** 基于 `HashMap` 的基础上，增加 `before` 和 `after` 指针使节点具备双向链表的特性。

* `HashMap` 的树节点 `TreeNode` 继承了具备双向链表特性的 `LinkedHashMap` 的 `Entry`。

总结：Entry类是LinkedHashMap中的节点类，充当HashMap中Node类的作用。

HashMap 的节点集合 Node则仅包含kv对和下一个元素指针，避免使用HashMap的时候也出现无关的双向链表元素。

TreeNode用于在内部链表转化为红黑树的时候使用，继承enry类来获取双向链表指针。但是这样做，也使得使用 `HashMap` 时的 `TreeNode` 多了两个没有必要的引用。对于这个问题,引用作者的一段注释，作者们认为**在良好的 `hashCode` 算法时，`HashMap` 转红黑树的概率不大。就算转为红黑树变为树节点，也可能会因为移除或者扩容将 `TreeNode` 变为 `Node`，所以 `TreeNode` 的使用概率不算很大，对于这一点资源空间的浪费是可以接受的。**

<img src="Java\map-hashmap-linkedhashmap.png" alt="LinkedHashMap 和 HashMap 之间的关系" style="zoom: 50%;" />

#### LinkedHashMap vs HashMap

`LinkedHashMap` 和 `HashMap` 都是 Java 集合框架中的 Map 接口的实现类。它们的最大区别在于**迭代元素的顺序**。`HashMap` 迭代元素的顺序是不确定的，而 `LinkedHashMap` 提供了按照**插入顺序或访问顺序**迭代元素的功能。此外，`LinkedHashMap` 内部维护了一个**双向链表，**用于记录元素的插入顺序或访问顺序，而 `HashMap` 则没有这个链表。因此，`LinkedHashMap` 的插入性能可能会比 `HashMap` 略低，但它提供了更多的功能并且**迭代效率相较于 `HashMap` 更加高效**。

## 并发

### 线程

进程是程序的一次执行过程，是系统运行程序的基本单位。线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程**共享**进程的**堆**和**方法区**（**JDK1.8 之后的元空间**）资源，但每个线程有自己的**程序计数器**、**虚拟机栈**和**本地方法栈**，所以系统在产生一个线程，或是在各个线程之间做切换工作时，负担要比进程小得多，也正因为如此，线程也被称为**轻量级进程**。

**线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。线程执行开销小，但不利于资源的管理和保护；而进程正相反。**

私有：

**程序计数器**：字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。在多线程的情况下，程序计数器用于**记录当前线程执行的位置**，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。计数器私有是为了各线程之间切换，便于恢复到正确的执行位置。

**虚拟机栈：** 每个 Java 方法在执行之前会创建一个栈帧用于存储===**局部变量表、操作数栈、常量池引用**===等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。

**本地方法栈：** 和虚拟机栈所发挥的作用非常相似，区别是：**虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。** 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。

为了**保证线程中的===局部变量===不被别的线程访问到**，虚拟机栈和本地方法栈是线程私有的。

公有：

堆和方法区是所有线程共享的资源，其中堆是进程中最大的一块内存，主要用于存放新创建的**对象** (几乎所有对象都在这里分配内存)，方法区主要用于存放已被加载的**类信息、常量、静态变量、即时编译器编译后的代码**等数据。

================================================

- 用户线程：由用户空间程序管理和调度的线程，运行在用户空间（专门给应用程序使用）。
- 内核线程：由操作系统内核管理和调度的线程，运行在内核空间（只有内核程序可以访问）。

**现在的 Java 线程的本质其实就是操作系统的线程**。

线程模型：线程模型是用户线程和内核线程之间的关联方式。

1. 一对一（一个用户线程对应一个内核线程）
2. 多对一（多个用户线程映射到一个内核线程）
3. 多对多（多个用户线程映射到多个内核线程）

在 Windows 和 Linux 等主流操作系统中，Java 线程采用的是一对一的线程模型，也就是一个 Java 线程对应一个系统内核线程。

#### 生命周期

* 线程创建之后它将处于 **NEW（新建）** 状态，调用 `start()` 方法后开始运行，线程这时候处于 **READY（可运行）** 状态。可运行状态的线程获得了 CPU 时间片（timeslice）后就处于 **RUNNING（运行）** 状态。
  * 在操作系统层面，线程有 **READY 和 RUNNING** 状态；而在 JVM 层面，只能看到 RUNNABLE 状态， Java 系统一般将这两个状态统称为 **RUNNABLE（运行中）** 状态 。JVM没有区分这两种状态，时分（time-sharing）多任务（multi-task）操作系统架构通常都是用“时间分片”方式进行抢占式（preemptive）轮转调度（round-robin 式）。这个时间分片通常是很小的，**一个线程一次最多只能在 CPU 上运行比如 10-20ms 的时间（此时处于 running 状态）**，也即大概只有 0.01 秒这一量级，时间片用后就要被切换下来放入调度队列的末尾等待再次调度。（也即回到 ready 状态）。线程切换的如此之快，区分这两种状态就没什么意义了。

* 当线程执行 `wait()`方法之后，线程进入 **WAITING（等待）** 状态。进入等待状态的线程需**要依靠其他线程的通知**才能够返回到运行状态。（等待状态，表示该线程需要等待其他线程做出一些特定动作如通知或中断）
* **TIMED_WAITING(超时等待)** 状态相当于在等待状态的基础上增加了超时限制，比如通过 `sleep（long millis）`方法或 `wait（long millis）`方法可以将线程置于 TIMED_WAITING 状态。当超时时间结束后，线程将会返回到 RUNNABLE 状态。
* 当线程进入 `synchronized` 方法/块或者调用 `wait` 后（被 `notify`）重新进入 `synchronized` 方法/块，但是锁被其它线程占有，这个时候线程就会进入 **BLOCKED（阻塞）** 状态。、
* 线程在执行完了 `run()`方法之后将会进入到 **TERMINATED（终止）** 状态。

<img src="Java\640.png" alt="Java 线程状态变迁图" style="zoom:80%;" />

**线程上下文切换：**保存当前线程的上下文（线程运行过程中的条件和状态），留待线程下次占用 CPU 的时候恢复现场。并加载下一个将要占用 CPU 的线程上下文。线程切换可能发生在这些场景：**主动让出 CPU**，比如调用了 sleep(), wait() 等。**时间片用完**，因为操作系统要防止一个线程或者进程长时间占用 CPU 导致其他线程或者进程饿死。调用了阻塞类型的**系统中断**，比如请求 IO，线程被阻塞。被终止或**结束运行**。

#### 一些方法

**Thread#sleep() 方法和 Object#wait() 方法**：都可以暂停线程的执行。区别是sleep是让当前线程休眠一会，之后就会自动恢复，所以不会释放锁。而wait（）对应线程生命周期中的等待状态，目的是线程之间的通信和交互，需要释放锁等待其他线程通知才能回到运行状态。`sleep()` 是 `Thread` 类的静态本地方法，`wait()` 则是 `Object` 类的本地方法。

* `wait()` 是让获得**对象锁**的线程实现等待，会自动释放当前线程占有的对象锁。**每个对象（`Object`）都拥有对象锁**，既然要释放当前线程占有的对象锁并让其进入 WAITING 状态，自然是要**操作对应的对象**（`Object`）而非当前的线程（`Thread`）。
* `sleep()` 是让当前**线程**暂停执行，不涉及到对象类，也不需要获得对象锁。所以定义在Thread中。

关于run和start：调用 `start()` 方法启动线程并使线程进入就绪状态，会执行线程的相应准备工作，然后**自动执行 `run()` 方法**的内容。如果开发者手动直接执行 `run()` 方法的话，会把 `run()` 方法当成一个 main 线程下的普通方法去执行，不会以多线程的方式执行。

### 死锁

死锁是多线程或多进程并发编程中的一种常见问题，它发生在**两个或多个线程（或进程）相互等待对方释放资源**的情况下，导致它们都无法继续执行下去的状态。这种情况下，每个线程都在等待某个资源，而同时也拥有一些资源。

```java
public class DeadLockDemo {
    private static Object resource1 = new Object();//资源 1
    private static Object resource2 = new Object();//资源 2

    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (resource1) {
                System.out.println(Thread.currentThread() + "get resource1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource2");
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                }
            }
        }, "线程 1").start();

        new Thread(() -> {
            synchronized (resource2) {
                System.out.println(Thread.currentThread() + "get resource2");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource1");
                synchronized (resource1) {
                    System.out.println(Thread.currentThread() + "get resource1");
                }
            }
        }, "线程 2").start();
    }
}
```

死锁的四个必要条件：

1. 互斥条件：该资源任意一个时刻只由一个线程占用。
2. **请求与保持**/占有并等待条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放。
3. 非抢占条件:线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
4. 循环等待条件:若干线程之间形成一种头尾相接的循环等待资源关系。

预防死锁：

1.破坏占有并等待条件：一次性申请所有资源；

2.破坏非抢占条件：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。

3.破坏循环等待条件：按顺序申请资源，反序释放资源

避免死锁：

避免死锁就是在资源分配时，借助于算法（比如银行家算法）对资源分配进行计算评估，使其进入安全状态。

### volatile

volatile是Java提供的一种轻量级的同步机制，在并发编程中，它也扮演着比较重要的角色在。`volatile` 关键字可以保证变量的可见性， **所谓可见性，是指当一个线程修改了某一个共享变量的值，其他线程是否能够立即知道该变更**。如果我们将变量声明为 **`volatile`** ，这就指示 JVM，这个变量是**共享且不稳定**的，每次使用它都到**主存**中进行读取。`volatile` 关键字能保证数据的可见性，**但不能保证数据的原子性**。`synchronized` 关键字两者都能保证。

**JMM规定了所有的变量都存储在主内存中**。普通变量不能保证内存可见性。而volatile则保证了**可见性和有序性**。

- 当**写**一个volatile变量时，JMM会把该线程对应的本地内存中的共享变量值**立即刷新回主内存中**。
- 当**读**一个volatile变量时，JMM会把该线程对应的本地内存设置为无效，重新回到主内存**中读取最新共享变量**。

有序性，即**禁止指令重排序**。在对volatile变量进行读写操作的时候，会通过插入特定的 **内存屏障** 的方式来禁止指令重排序。

* 重排序是指编译器和处理器为了优化程序性能**面对指令序列进行重新排序**的一种手段，有时候会改变程序予以的先后顺序。（但重排后的指令绝对不能改变原有串行语义）
  - 不存在数据依赖关系，可以重排序；
  - 存在数据依赖关系，禁止重排序。

内存屏障（Memory Barrier，或有时叫做内存栅栏，Memory Fence）是一种CPU指令，用于控制特定条件下的重排序和内存可见性问题。Java编译器也会根据内存屏障的规则禁止重排序。

- **读屏障**(Load Memory Barrier) ：在读指令之前插入读屏障，让工作内存或CPU高速缓存当中的缓存数据失效，重新回到主内存中获取最新数据。
- **写屏障**(Store Memory Barrier) ：在写指令之后插入写屏障，强制把写缓冲区的数据刷回到主内存中。

因此重排序时，不允许把内存屏障之后的指令重排序到内存屏障之前。一句话：**对一个volatile变量的写，先行发生于任意后续对这个volatile变量的读，也叫写后读。**

**双重校验锁实现对象单例（线程安全）**

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public  static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

`uniqueInstance` 采用 `volatile` 关键字修饰也是很有必要的， `uniqueInstance = new Singleton();` 这段代码其实是分为三步执行：

1. 为 `uniqueInstance` 分配内存空间
2. 初始化 `uniqueInstance`
3. 将 `uniqueInstance` 指向分配的内存地址

但是由于 JVM 具有指令重排的特性，执行顺序有可能变成 1->3->2。指令重排在单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。例如，线程 T1 执行了 1 和 3，此时 T2 调用 `getUniqueInstance`() 后发现 `uniqueInstance` 不为空，因此返回 `uniqueInstance`，但此时 `uniqueInstance` 还未被初始化。使用volatile修饰，就能禁止指令重排。

### 乐观锁和悲观锁
悲观锁总是假设最坏的情况，认为共享资源每次被访问的时候就会出现问题(比如共享数据被修改)，所以每次在获取资源操作的时候都会上锁，这样其他线程想拿到这个资源就会阻塞直到锁被上一个持有者释放。也就是说，**共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程**。

像 Java 中`synchronized`和`ReentrantLock`等独占锁就是悲观锁思想的实现。

> 高并发的场景下，激烈的锁竞争会造成线程阻塞，大量阻塞线程会导致系统的上下文切换，增加系统的性能开销。并且，悲观锁还可能会存在死锁问题，影响代码的正常运行。

乐观锁总是假设最好的情况，认为共享资源每次被访问的时候不会出现问题，线程可以不停地执行，无需加锁也无需等待，只是在提交修改的时候去验证对应的资源（也就是数据）是否被其它线程修改了（具体方法可以使用**版本号机制或 CAS 算法**）。

在 Java 中`java.util.concurrent.atomic`包下面的原子变量类（比如`AtomicInteger`、`LongAdder`）就是使用了乐观锁的一种实现方式 **CAS** 实现的。

> 高并发的场景下，乐观锁相比悲观锁来说，不存在锁竞争造成线程阻塞，也不会有死锁的问题，在性能上往往会更胜一筹。但是，如果冲突频繁发生（写占比非常多的情况），会频繁失败和重试，这样同样会非常影响性能，导致 CPU 飙升。

理论上来说：

- 悲观锁通常多用于写比较多的情况（多写场景，竞争激烈），这样可以**避免频繁失败和重试影响性能**，悲观锁的**开销是固定的**。不过，如果乐观锁解决了频繁失败和重试这个问题的话（比如`LongAdder`），也是可以考虑使用乐观锁的，要视实际情况而定。
- 乐观锁通常多用于写比较少的情况（多读场景，竞争较少），这样可以**避免频繁加锁影响性能**。不过，乐观锁主要针对的对象是单个共享变量（参考`java.util.concurrent.atomic`包下面的原子变量类）。

#### CAS

CAS 的全称是 **Compare And Swap（比较与交换）** ，用于实现乐观锁，被广泛应用于各大框架中。CAS 的思想很简单，就是**用一个预期值和要更新的变量值进行比较，两值相等才会进行更新。**

CAS 是一个原子操作，底层依赖于一条 CPU 的原子指令。

CAS 涉及到三个操作数：**V**：要更新的变量值(Var)；**E**：预期值(Expected)；**N**：拟写入的新值(New)

当且仅当 V 的值等于 E 时，CAS 通过原子方式用新值 N 来更新 V 的值。如果不等，说明已经有其它线程更新了 V，则当前线程放弃更新。（和版本号机制思想一致）

问题：如果一个变量 V 初次读取的时候是 A 值，并且在准备赋值的时候检查到它仍然是 A 值，那我们就能说明它的值没有被其他线程修改过了吗？很明显是不能的，因为在这段时间它的值可能被改为其他值，然后又改回 A，那 CAS 操作就会误认为它从来没有被修改过。这个问题被称为 CAS 操作的 **"ABA"问题。**

解决方案：在变量前面追加上**版本号或者时间戳**。`AtomicStampedReference` 类的 `compareAndSet()` 方法就是首先检查当前**引用**是否等于预期引用，并且当前**标志**是否等于预期标志，如果**全部相等**，则以原子方式将该引用和该标志的值设置为给定的更新值。

另一个问题：**循环时间长开销大**。CAS 经常会用到自旋操作来进行重试，也就是不成功就一直循环执行直到成功。如果长时间不成功，会给 CPU 带来非常大的执行开销。如果 JVM 能支持处理器提供的 pause 指令那么效率会有一定的提升。

### Synchronized

`synchronized` 是 Java 中的一个关键字，翻译成中文是同步的意思，主要解决的是**多个线程之间访问资源的同步性**，可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。

在 Java 早期版本中，`synchronized` 属于 **重量级锁**，效率低下。因为早期锁依赖于操作系统底层mutex lock实现，Java线程要映射到操作系统原生线程之上，也就是挂起或唤醒线程进行线程上下文切换时，**都需要从用户态转换成内核态**，这个状态之间的转换需要相对比较长的时间，时间成本相对较高。

在 Java 6 之后， `synchronized` 引入了大量的优化如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销，这些优化让 `synchronized` 锁的效率提升了很多。（JDK18 中，偏向锁已经被彻底废弃）锁主要存在四种状态，依次是：**无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态**，他们会随着竞争的激烈而逐渐升级。注意**锁可以升级不可降级**，这种策略是为了提高获得锁和释放锁的效率。

#### 使用

```java
// 修饰实例方法，给当前对象实例加锁，进入同步代码前要获得 当前对象实例的锁 。
synchronized void method() {
    //业务代码
}

// 修饰静态方法，当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 当前 class 的锁。
synchronized static void method() {
    //业务代码
}

// 修饰代码块，对括号里指定的对象/类加锁：synchronized(object)或synchronized(类.class)
synchronized(this) {
    //业务代码
}
```

#### 原理

synchronized 关键字底层原理属于 JVM 层面的东西。

**同步语句块**

`synchronized` **同步语句块**的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。

当执行 `monitorenter` 指令时，线程试图获取锁也就是获取 **对象监视器 `monitor`** 的持有权。如果锁的计数器为 0 则表示锁可以被获取，获取后将锁计数器设为 1 也就是加 1。对象锁的的拥有者线程才可以执行 `monitorexit` 指令来释放锁。在执行 `monitorexit` 指令后，将锁计数器设为 0，表明锁被释放，其他线程可以尝试获取锁。如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。

**同步方法**

`synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。**不过两者的本质都是对对象监视器 monitor 的获取。**JVM 通过该 `ACC_SYNCHRONIZED` 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。如果是实例方法，JVM 会尝试获取实例对象的锁。如果是静态方法，JVM 会尝试获取当前 class 的锁。

#### synchronized vs volatile

`synchronized` 关键字和 `volatile` 关键字是两个**互补**的存在，而不是对立的存在！

- `volatile` 关键字是线程同步的**轻量级**实现，所以 `volatile`性能肯定比`synchronized`关键字要好 。但是 `volatile` 关键字**只能用于变量**而 `synchronized` 关键字可以修饰方法以及代码块 。
- `volatile` 关键字能保证**数据的可见性**，但不能保证数据的原子性。`synchronized` 关键字**两者都能保证**。
- `volatile`关键字主要用于解决**变量在多个线程之间的可见性**，而 `synchronized` 关键字解决的是**多个线程之间访问资源的同步性**。

### ReentrantLock

`ReentrantLock` 实现了 `Lock` 接口，是一个**可重入且独占**式的锁，和 `synchronized` 关键字类似。不过，`ReentrantLock` 更灵活、更强大，增加了**轮询、超时、中断、公平锁和非公平锁**等高级功能。

```java
public class ReentrantLock implements Lock, java.io.Serializable {}
```

`ReentrantLock` 里面有一个**内部类 `Sync`**，`Sync` 继承 AQS（`AbstractQueuedSynchronizer`），添加锁和释放锁的大部分操作实际上都是在 `Sync` 中实现的。`Sync` 有公平锁 `FairSync` 和非公平锁 `NonfairSync` 两个子类。

`ReentrantLock` 默认使用非公平锁，也可以通过构造器来显式的指定使用公平锁。`ReentrantLock` 的底层就是由 AQS 来实现的。

<img src="Java\reentrantlock-class-diagram.png" alt="img" style="zoom: 50%;" />

**公平锁** : 锁被释放之后，**先申请的线程先得到锁**。性能较差一些，因为公平锁为了保证时间上的绝对顺序，**上下文切换更频繁**。

- 优点：所有的线程都能得到资源，不会饿死在队列中。
- 缺点：吞吐量会下降很多，队列里面除了第一个线程，其他的线程都会阻塞，**cpu唤醒阻塞线程的开销会很大**。

**非公平锁**：锁被释放之后，后申请的线程可能会先获取到锁，是随机或者按照其他优先级排序的。**性能更好**，但可能会导致某些线程永远无法获取到锁。

* 非公平锁比公平锁效率高的原因主要在于**减少了线程切换和同步操作的次数**。
* 当线程在运行期间直接抢占到锁资源时，不需要进行“执行现场保存和恢复”的操作，从而能够更快地执行业务代码。相比之下，如果一个就绪态的线程想要获得锁资源，首先需要恢复现场，之后争抢锁（可能成功也可能失败），这个过程浪费了大量的CPU资源，只有在获取锁成功后才能继续执行业务代码。因此，非公平锁在效率上优于公平锁，主要原因就在于是否需要进行现场恢复和不同态之间的切换。**非公平锁减少了线程挂起的几率**，后来的线程有一定几率逃离被挂起的开销。

### synchronized vs ReentrantLock

* 两者都是可重入锁。**可重入锁** 也叫**递归锁**，指的是**线程可以再次获取自己的内部锁**。比如一个线程获得了某个对象的锁，此时这个对象锁还没有释放，当其再次想要获取这个对象的锁的时候还是可以获取的，**如果是不可重入锁的话，就会造成死锁。**
  * 可重入锁主要用在线程需要多次进入临界区代码时，需要使用可重入锁。
  * 每一个锁关联一个线程持有者和计数器，当计数器为 0 时表示该锁没有被任何线程持有，那么任何线程都可能获得该锁而调用相应的方法；当某一线程请求成功后，JVM会**记下锁的持有线程**，并且将计数器置为 1；此时其它线程请求该锁，则必须等待；而该持有锁的线程如果再次请求这个锁，就可以**再次拿到这个锁，同时计数器会递增**；当线程**退出同步代码块时，计数器会递减**，如果计数器为 0，则释放该锁。

* `synchronized` 是依赖于 JVM 实现的，前面我们也讲到了 虚拟机团队在 JDK1.6 为 synchronized 关键字进行了很多优化，但是**这些优化都是在虚拟机层面实现的，并没有直接暴露给我们**。ReentrantLock 是 JDK 层面实现的（也就是 API 层面，需要 lock() 和 unlock() 方法配合 try/finally 语句块来完成），所以我们可以**通过查看它的源代码，来看它是如何实现的**。
* 相比`synchronized`，`ReentrantLock`增加了一些高级功能。主要来说主要有三点：
  * **等待可中断** : `ReentrantLock`提供了一种能够**中断等待锁的线程**的机制，通过 `lock.lockInterruptibly()` 来实现这个机制。也就是说**正在等待的线程可以选择放弃等待，改为处理其他事情。**
    * **可中断锁**：获取锁的过程中可以被中断，不需要一直等到获取锁之后 才能进行其他逻辑处理。`ReentrantLock` 就属于是可中断锁。
    * **不可中断锁**：一旦线程申请了锁，就只能**等到拿到锁以后才能进行其他的逻辑处理**。 `synchronized` 就属于是不可中断锁。
  * **可实现公平锁** : `ReentrantLock`可以指定是公平锁还是非公平锁。而**`synchronized`只能是非公平锁**。所谓的公平锁就是先等待的线程先获得锁。`ReentrantLock`默认情况是非公平的，可以通过 `ReentrantLock`类的`ReentrantLock(boolean fair)`构造方法来指定是否是公平的。
  * **可实现选择性通知（锁可以绑定多个条件）**: `synchronized`关键字与`wait()`和`notify()`/`notifyAll()`方法相结合可以实现等待/通知机制。`ReentrantLock`类当然也可以实现，但是需要借助于`Condition`接口与`newCondition()`方法。
    * `Condition`是 JDK1.5 之后才有的，它具有很好的灵活性，比如可以实现**多路通知功能**也就是在一个`Lock`对象中可以创建多个`Condition`实例（即对象监视器），**线程对象可以注册在指定的`Condition`中，从而可以有选择性的进行线程通知，**在调度线程上更加灵活。 在使用`notify()/notifyAll()`方法进行通知时，被通知的线程是由 JVM 选择的，用`ReentrantLock`类结合`Condition`实例可以实现“选择性通知” ，这个功能非常重要，而且是 `Condition` 接口默认提供的。而**`synchronized`关键字就相当于整个 `Lock` 对象中只有一个`Condition`实例**，所有的线程都注册在它一个身上。如果执行`notifyAll()`方法的话就会通知所有处于等待状态的线程，这样会造成很大的效率问题。而**`Condition`实例的`signalAll()`方法，只会唤醒注册在该`Condition`实例中的所有等待线程。**

### ReentrantReadWriteLock

`ReentrantReadWriteLock` 实现了 `ReadWriteLock` ，是一个**可重入的读写锁**，既可以保证多个线程同时读的效率，同时又可以保证有写入操作时的线程安全。

```java
public class ReentrantReadWriteLock
        implements ReadWriteLock, java.io.Serializable{
}
public interface ReadWriteLock {
    Lock readLock();
    Lock writeLock();
}
```

`ReentrantReadWriteLock` 其实是两把锁，一把是 `WriteLock` (写锁)，一把是 `ReadLock`（读锁） 。**读锁是共享锁，写锁是独占锁**。读锁可以被同时读，可以同时被多个线程持有，而写锁最多只能同时被一个线程持有。和 `ReentrantLock` 一样，`ReentrantReadWriteLock` 底层也是基于 AQS 实现的。`ReentrantReadWriteLock` 也支持公平锁和非公平锁，默认使用非公平锁，可以通过构造器来显示的指定。

在**读多写少**的情况下，使用 `ReentrantReadWriteLock` 能够明显提升系统性能。

- **共享锁**：一把锁可以被多个线程同时获得。
- **独占锁**：一把锁只能被一个线程获得。

读写锁进行并发控制的规则：读读不互斥、读写互斥、写写互斥（只有读读不互斥）。

**在线程持有读锁的情况下，该线程不能取得写锁。**在线程持有写锁的情况下，该线程可以继续获取读锁。

**当线程获取读锁的时候，可能有其他线程同时也在持有读锁，因此不能把获取读锁的线程“升级”为写锁；而对于获得写锁的线程，它一定独占了读写锁，因此可以继续让它获取读锁，当它同时获取了写锁和读锁后，还可以先释放写锁继续持有读锁，这样一个写锁就“降级”为了读锁。**

### StampedLock

`StampedLock` 是 JDK 1.8 引入的性能更好的读写锁，**不可重入且不支持条件变量** `Condition`。

`StampedLock` 并不是直接实现 `Lock`或 `ReadWriteLock`接口，而是基于 **CLH 锁** 独立实现的（AQS 也是基于这玩意），CLH 锁是对自旋锁的一种改良，是一种隐式的链表队列。`StampedLock` 通过 CLH 队列进行线程的管理，通过同步状态值 `state` 来表示锁的状态和类型。

`StampedLock` 提供了三种模式的读写控制模式：读锁、写锁和乐观读。

- **写锁**：独占锁，一把锁只能被一个线程获得。当一个线程获取写锁后，其他请求读锁和写锁的线程必须等待。类似于 `ReentrantReadWriteLock` 的写锁，不过这里的写锁是**不可重入**的。
- **读锁** （悲观读）：共享锁，没有线程获取写锁的情况下，多个线程可以同时持有读锁。如果己经有线程持有写锁，则其他线程请求获取该读锁会被阻塞。类似于 `ReentrantReadWriteLock` 的读锁，不过这里的读锁是不可重入的。
- **乐观读**：**允许多个线程获取乐观读以及读锁。同时允许一个写线程获取写锁。**

`StampedLock` 在获取锁的时候会返回一个 long 型的数据戳，该数据戳用于稍后的锁释放参数，如果返回的数据戳为 0 则表示锁获取失败。当前线程持有了锁再次获取锁还是会返回一个新的数据戳，这也是`StampedLock`不可重入的原因。

**StampedLock比ReentrantReadWriteLock性能更好，主要体现在以下几个方面：**
1、增加乐观读功能，减少写线程饥饿现象出现

​	当线程尝试获取乐观读锁时，StampedLock 会检查当前是否有写锁被持有。如果没有，它会增加一个读锁计数器并返回一个 stamp（通常是当前状态的一个快照）。乐观读锁不会阻塞其他读线程或写线程，但可能在写线程获得锁后读取到不一致的数据。

2、StampedLock要比ReentrantReadWriteLock消耗小

3、StampedLock增加了更多的无锁操作，使线程间阻塞减少到最小。

### ThreadLocal

`ThreadLocal`类主要解决的就是让每个线程绑定自己的值，拥有自己的私有数据（专属本地变量）。如果你创建了一个`ThreadLocal`变量，那么访问这个变量的**每个线程都会有这个变量的本地副本**，这也是`ThreadLocal`变量名的由来。他们可以使用 `get()` 和 `set()` 方法来获取默认值或将其值更改为当前线程所存的副本的值，从而**避免了线程安全问**题。

ThreadLocal 适用于每个线程需要自己独立的实例且该实例需要在多个方法中被使用，也即变量在线程间隔离而在方法或类间共享的场景

**对比synchronized**

ThreadLocal和Synchonized都用于解决**多线程并发访问**。但是ThreadLocal与synchronized有本质的区别：

1、Synchronized用于线程间的**数据共享**，而ThreadLocal则用于线程间的**数据隔离**。

2、Synchronized是利用锁的机制，使变量或代码块在某一时该只能被一个线程访问。而ThreadLocal为每一个线程都提供了变量的副本，使得每个线程在某一时间访问到的**并不是同一个对象**，这样就隔离了多个线程对数据的数据共享。

### 原理

> 一句话理解ThreadLocal，ThreadLocal是作为**当前线程Thread中**  属性ThreadLocalMap集合  中的**某一个Entry的key值**Entry（threadlocal, value），虽然不同的线程之间ThreadLocal这个key值是一样，但是不同的线程所拥有的**ThreadLocalMap是独一无二的**，也就是不同的线程间同一个ThreadLocal（key）对应存储的值(value)不一样，从而到达了线程间变量隔离的目的，但是在同一个线程中这个value变量地址是一样的。

**ThreadLocal的set()方法**

```java
public class Thread implements Runnable {
    //......
    //与此线程有关的ThreadLocal值。由ThreadLocal类维护
    ThreadLocal.ThreadLocalMap threadLocals = null;

    //与此线程有关的InheritableThreadLocal值。由InheritableThreadLocal类维护
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
    //......
}
```

`Thread` 类中有一个 `threadLocals` 和 一个 `inheritableThreadLocals` 变量，它们都是 `ThreadLocalMap` 类型的变量,我们可以把 `ThreadLocalMap` 理解为`ThreadLocal` 类实现的定制化的 `HashMap`。默认情况下这两个变量都是 null，只有当前线程**调用 `ThreadLocal` 类的 `set`或`get`方法**时才创建它们，实际上调用这两个方法的时候，我们调用的是`ThreadLocalMap`类对应的 `get()`、`set()`方法。

```java
public void set(T value) {
    //1、获取当前线程
    Thread t = Thread.currentThread();
    //2、获取线程中的属性 threadLocalMap ,如果threadLocalMap 不为空，
    //则直接更新要保存的变量值，否则创建threadLocalMap，并赋值
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        // 初始化thradLocalMap 并赋值
        createMap(t, value);
}
```

ThreadLocal set赋值的时候首先会获取当前线程thread,并获取thread线程中的ThreadLocalMap属性。如果map属性不为空，则直接更新value值，如果map为空，则实例化threadLocalMap,并将value值初始化。

**每个`Thread`中都具备一个`ThreadLocalMap`，而`ThreadLocalMap`可以存储以`ThreadLocal`为 key ，Object 对象为 value 的键值对。**比如在同一个线程中声明了两个 `ThreadLocal` 对象的话， `Thread`内部都是使用仅有的那个`ThreadLocalMap` 存放数据的，`ThreadLocalMap`的 key 就是 `ThreadLocal`对象，value 就是 `ThreadLocal` 对象调用`set`方法设置的值。

<img src="Java\threadlocal-data-structure.png" alt="ThreadLocal 数据结构" style="zoom:80%;" />

### ThreadLocal 内存泄露问题是怎么导致的

`ThreadLocalMap` 中使用的 **key 为 `ThreadLocal` 的弱引用，而 value 是强引用。**所以，如果 `ThreadLocal` 没有被外部强引用的情况下，在垃圾回收的时候，key 会被清理掉，而 value 不会被清理掉。

这样一来，`ThreadLocalMap` 中就会出现 key 为 null 的 Entry。假如我们不做任何措施的话，value 永远无法被 GC 回收，这个时候就可能会产生内存泄露。`ThreadLocalMap` 实现中已经考虑了这种情况，在调用 `set()`、`get()`、`remove()` 方法的时候，会清理掉 key 为 null 的记录。使用完 `ThreadLocal`方法后最好手动调用`remove()`方法

## 线程池

线程池就是管理一系列线程的资源池。当有任务要处理时，直接从线程池中获取线程来处理，处理完之后线程并不会立即被销毁，而是等待下一个任务。

### 为什么要用线程池

**线程池**提供了一种限制和管理资源（包括执行一个任务）的方式。 每个**线程池**还维护一些基本统计信息，例如已完成任务的数量。

**使用线程池的好处**：

- **降低资源消耗**。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。（池化技术，减少每次获取资源的消耗，提高对资源的利用率）
- **提高响应速度**。当任务到达时，任务可以不需要等到线程创建就能立即执行。
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的**分配，调优和监控**。

### 创建线程池

```java
/**
     * 用给定的初始参数创建一个新的ThreadPoolExecutor。
     */
public ThreadPoolExecutor(int corePoolSize,//线程池的核心线程数量
                          int maximumPoolSize,//线程池的最大线程数
                          long keepAliveTime,//===当线程数大于核心线程数时===，多余的空闲线程存活的最长时间
                          TimeUnit unit,//时间单位
                          BlockingQueue<Runnable> workQueue,//任务队列，用来储存等待执行任务的队列
                          ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
                          RejectedExecutionHandler handler//拒绝策略，当提交的任务过多而不能及时处理时，我们可以定制策略来处理任务
                         ) {
    if (corePoolSize < 0 ||
        maximumPoolSize <= 0 ||
        maximumPoolSize < corePoolSize ||
        keepAliveTime < 0)
        throw new IllegalArgumentException();
    if (workQueue == null || threadFactory == null || handler == null)
        throw new NullPointerException();
    this.corePoolSize = corePoolSize;
    this.maximumPoolSize = maximumPoolSize;
    this.workQueue = workQueue;
    this.keepAliveTime = unit.toNanos(keepAliveTime);
    this.threadFactory = threadFactory;
    this.handler = handler;
}
```

最重要的三个参数：

* `corePoolSize` : 任务队列未达到队列容量时，最大可以同时运行的线程数量。
* `maximumPoolSize` : 任务队列中存放的任务达到队列容量的时候，**当前可以同时运行的线程数量变为最大线程数。**
* `workQueue`: 新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。

**使用示例**

- execute用于提交不需要返回值的任务。
- submit用于提交需要返回值的任务。
- shutdown平缓关闭线程池，不再接受新任务，已提交任务继续执行。
- shutdownNow试图停止所有正在执行的活动任务，暂停处理正在等待的任务，并返回等待执行的任务列表。

```java
// 实际项目中使用ThreadPoolExecutor的示例
import java.util.concurrent.*;

public class RequestHandler {
    private final ThreadPoolExecutor executor;

    public RequestHandler(int corePoolSize, int maxPoolSize, long keepAliveTime, TimeUnit unit, int queueCapacity) {
        BlockingQueue<Runnable> workQueue = new LinkedBlockingQueue<>(queueCapacity);
        this.executor = new ThreadPoolExecutor(corePoolSize, maxPoolSize, keepAliveTime, unit, workQueue);
    }

    public void handleRequest(Runnable task) {
        // 提交一个任务
        executor.execute(task);
    }

    // 停止线程池的方法，通常在服务停止时调用
    public void shutdown() {
        executor.shutdown();
    }
}
```

### 线程池的拒绝策略

如果**当前同时运行的线程数量达到最大线程数量并且队列也已经被放满了任务时**，`ThreadPoolExecutor` 定义一些策略:

* `ThreadPoolExecutor.AbortPolicy`：抛出 `RejectedExecutionException`来拒绝新任务的处理。
* `ThreadPoolExecutor.CallerRunsPolicy`：调用执行自己的线程运行任务，也就是**直接在调用`execute`方法的线程中运行(`run`)被拒绝的任务**，如果执行程序已关闭，则会丢弃该任务。因此这种策略会**降低对于新任务提交速度**，影响程序的整体性能。如果你的应用程序可以承受此延迟并且你要求任何一个任务请求都要被执行的话，你可以选择这个策略。
  * 将任务回退给调用者，使用调用者的线程来执行任务。
* `ThreadPoolExecutor.DiscardPolicy`：不处理新任务，直接丢弃掉。
* `ThreadPoolExecutor.DiscardOldestPolicy`：此策略将丢弃最早的未处理的任务请求。

如果不允许丢弃任务，只能选择CallerRunsPolicy，**问题**：如果走到`CallerRunsPolicy`的任务是个非常耗时的任务，且处理提交任务的线程是主线程，可能会**导致主线程阻塞，影响程序的正常运行。**

**解决思路**

我们从问题的本质入手，调用者采用`CallerRunsPolicy`是**希望所有的任务都能够被执行**，暂时无法处理的任务又被保存在阻塞队列`BlockingQueue`中。这样的话，在内存允许的情况下，我们可以**增加阻塞队列`BlockingQueue`的大小**并调整堆内存以容纳更多的任务，确保任务能够被准确执行。

为了充分利用 CPU，我们还可以调整线程池的`maximumPoolSize` （最大线程数）参数，这样可以提高任务处理速度，避免累计在 `BlockingQueue`的任务过多导致内存用完。

**进一步：为了保证任务不被丢弃且后续能被处理，可以把任务持久化到数据库/缓存/消息队列**

如果服务器资源已达到可利用的极限，这就意味我们要在**设计策略**上改变线程池的调度了，我们都知道，导致主线程卡死的本质就是因为我们不希望任何一个任务被丢弃。换个思路，有没有办法**既能保证任务不被丢弃且在服务器有余力时及时处理呢？**

这里提供的一种**任务持久化**的思路，这里所谓的任务持久化，包括但不限于:

1. 设计一张任务表将任务存储到 MySQL 数据库中。
2. `Redis`缓存任务。
3. 将任务提交到消息队列中。

### 线程池常用阻塞队列

不同的线程池会选用不同的阻塞队列，我们可以结合内置线程池来分析。

- 容量为 `Integer.MAX_VALUE` 的 `LinkedBlockingQueue`**（无界队列）**：`FixedThreadPool`（可重用固定线程数的线程池） 和 `SingleThreadExector` 。`FixedThreadPool`最多只能创建核心线程数的线程（核心线程数和最大线程数相等），`SingleThreadExector`只能创建一个线程（核心线程数和最大线程数都是 1），二者的**任务队列永远不会被放满。**
- `SynchronousQueue`**（同步队列）**：`CachedThreadPool`（根据需要创建新线程的线程池） 。`SynchronousQueue` 没有容量，不存储元素，目的是保证对于提交的任务，如果有空闲线程，则使用空闲线程来处理；否则新建一个线程来处理任务。也就是说，`CachedThreadPool` 的最大线程数是 `Integer.MAX_VALUE` ，可以理解为**线程数是可以无限扩展的，可能会创建大量线程**，从而导致 OOM。
  - `CachedThreadPool` 的`corePoolSize` 被设置为空（0），`maximumPoolSize`被设置为 `Integer.MAX.VALUE`，即它是无界的，这也就意味着如果主线程提交任务的速度高于 `maximumPool` 中线程处理任务的速度时，`CachedThreadPool` 会不断创建新的线程。极端情况下，这样会导致耗尽 cpu 和内存资源。
- `DelayedWorkQueue`**（延迟阻塞队列）**：`ScheduledThreadPool` 和 `SingleThreadScheduledExecutor` 。`DelayedWorkQueue` 的内部元素并不是按照放入的时间排序，而是会按照延迟的时间长短对任务进行排序，内部采用的是“堆”的数据结构，可以保证**每次出队的任务都是当前队列中执行时间最靠前的**。`DelayedWorkQueue` **添加元素满了之后会自动扩容**原来容量的 1/2，即永远不会阻塞，最大扩容可达 `Integer.MAX_VALUE`，所以最多只能创建核心线程数的线程。

对比：Java 中常用的阻塞队列实现类有以下几种：

1. `ArrayBlockingQueue`：使用**数组**实现的有界阻塞队列。**在创建时需要指定容量大小**，并支持公平和非公平两种方式的锁访问机制。
2. `LinkedBlockingQueue`：使用**单向链表**实现的可选有界阻塞队列。在创建时可以指定容量大小，如果**不指定则默认为`Integer.MAX_VALUE`**。和`ArrayBlockingQueue`不同的是， 它仅支持**非公平**的锁访问机制。
3. `PriorityBlockingQueue`：支持优先级排序的**无界**阻塞队列。元素必须实现`Comparable`接口或者在构造函数中传入`Comparator`对象，并且不能插入 null 元素。
4. `SynchronousQueue`：**同步队列**，是一种不存储元素的阻塞队列。每个插入操作都必须等待对应的删除操作，反之删除操作也必须等待插入操作。因此，`SynchronousQueue`通常用于线程之间的直接传递数据。
5. `DelayQueue`：**延迟队列**，其中的元素只有到了其指定的延迟时间，才能够从队列中出队。

### 线程池处理任务的流程

<img src="Java\thread-pool-principle.png" alt="图解线程池实现原理" style="zoom:80%;" />

* 如果当前运行的线程数小于核心线程数，那么就会**新建一个线程**来执行任务。
  * 当提交一个新任务到线程池时，如果线程池中的线程数量小于核心线程数，即使其他工作线程是空闲的，也会创建一个新线程来处理该任务。
* 如果当前运行的线程数等于或大于核心线程数，但是小于最大线程数，那么就把该任务放入到任务队列里等待执行。
* 如果向任务队列投放任务失败（任务队列已经满了），但是当前运行的线程数是小于最大线程数的，就**新建一个线程**来执行任务。
* 如果当前运行的线程数已经等同于最大线程数了，新建线程将会使当前运行的线程超出最大线程数，那么当前任务会被拒绝，拒绝策略会调用`RejectedExecutionHandler.rejectedExecution()`方法。

### 线程异常后，销毁还是复用

**使用`execute()`提交任务**：当任务通过`execute()`提交到线程池并在执行过程中抛出异常时，如果这个异常没有在任务内被捕获，那么该异常会导致当前线程终止，并且异常会被打印到控制台或日志文件中。线程池会检测到这种线程终止，并创建一个新线程来替换它，从而保持配置的线程数不变。

**使用`submit()`提交任务**：对于通过`submit()`提交的任务，如果在任务执行中发生异常，这个异常不会直接打印出来。相反，异常会被封装在由`submit()`返回的`Future`对象中。当调用`Future.get()`方法时，可以捕获到一个`ExecutionException`。在这种情况下，线程不会因为异常而终止，它会继续存在于线程池中，准备执行后续的任务。

### execute和submit区别

1、**返回结果**：submit()方法可以接受并**返回Future对象，用于表示异步任务的结果**。你可以通过Future对象获取任务的执行结果，或者等待任务执行完成。而execute()方法没有返回值，无法获取任务的执行结果。
2、**异常处理**：submit()方法能够处理任务执行过程中抛出的异常。你可以通过调用Future对象的get()方法来获取任务执行过程中的异常，或者通过捕获ExecutionException异常来处理异常情况。而execute()方法无法处理任务执行过程中的异常，异常会被传播到线程池的未捕获异常处理器(UncaughtExceptionHandler)。
3、**方法重载**：submit()方法有多种重载形式，可以接受**Runnable、Callable和其他可执行任务作为参数**。它们的返回值类型分别为Future、Future和Future，其中T为Callable返回结果的类型。这使得submit()方法更加灵活，可以处理不同类型的任务。而**execute()方法只接受Runnable类型的任务作为参数**，没有方法重载的选项。

### Runnable与Callable

- Callable规定的方法是 call(), Runnable规定的方法是 run()。
- Callable的任务执行后可返回值，而 Runnable的任务是不能返回值。
- call方法可以抛出异常， run方法不可以。
- 运行 Callable任务可以拿到一个 Future对象

### shutdown()  VS  shutdownNow()

- **`shutdown（）`** :关闭线程池，线程池的状态变为 `SHUTDOWN`。线程池不再接受新任务了，但是队列里的任务得执行完毕。
- **`shutdownNow（）`** :关闭线程池，线程池的状态变为 `STOP`。线程池会终止当前正在运行的任务，并停止处理排队的任务并返回正在等待执行的 List。

### isTerminated()  VS  isShutdown()

- **`isShutDown`** 当调用 `shutdown()` 方法后返回为 true。
- **`isTerminated`** 当调用 `shutdown()` 方法后，并且所有提交的任务完成后返回为 true

### 设定线程池大小

* 如果我们设置的线程池数量太小的话，如果同一时间有大量任务/请求需要处理，可能会导致大量的请求/任务在任务队列中排队等待执行，甚至会出现**任务队列满了之后任务/请求无法处理**的情况，或者**大量任务堆积在任务队列导致 OOM**。这样很明显是有问题的，CPU 根本没有得到充分利用。
* 如果我们设置线程数量太大，**大量线程可能会同时在争取 CPU 资源**，这样会导致大量的上下文切换，从而增加线程的执行时间，影响了整体执行效率。

有一个简单并且适用面比较广的公式：

- **CPU 密集型任务(N+1)：** 这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1。比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。
- **I/O 密集型任务(2N)：** 这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 2N。

CPU 密集型简单理解就是利用 CPU 计算能力的任务比如你在内存中对大量数据进行排序。但凡涉及到网络读取，文件读取这类都是 IO 密集型，这类任务的特点是 CPU 计算耗费时间相比于等待 IO 操作完成的时间来说很少，大部分时间都花在了等待 IO 操作完成上。

### 优先级任务线程池

假如我们需要实现一个优先级任务线程池的话，那可以考虑使用 `PriorityBlockingQueue` （**优先级阻塞队列**）作为任务队列（`ThreadPoolExecutor` 的构造函数有一个 `workQueue` 参数可以传入任务队列）。

`PriorityBlockingQueue` 是一个支持优先级的无界阻塞队列，可以看作是线程安全的 `PriorityQueue`，两者底层都是使用小顶堆形式的二叉堆，即值最小的元素优先出队。不过，`PriorityQueue` 不支持阻塞操作。

要想让 `PriorityBlockingQueue` 实现对任务的排序，**传入其中的任务必须是具备排序能力**的，方式有两种：

1. 提交到线程池的任务实现 `Comparable` 接口，并重写 `compareTo` 方法来指定任务之间的优先级比较规则。
2. **创建 `PriorityBlockingQueue` 时传入一个 `Comparator` 对象来指定任务之间的排序规则(推荐)。**

不过，这存在一些风险和问题，比如：

- `PriorityBlockingQueue` 是无界的，可能堆积大量的请求，从而导致 OOM。
  - 解决：继承`PriorityBlockingQueue` 并重写一下 `offer` 方法(入队)的逻辑，**当插入的元素数量超过指定值就返回 false 。**
- 可能会导致饥饿问题，即低优先级的任务长时间得不到执行。
  - 解决：优化设计，等待时间过长的任务会被移除并重新添加到队列中，但是优先级会被提升。
- 由于需要对队列中的元素进行**排序操作以及保证线程安全**（并发控制采用的是可重入锁 `ReentrantLock`），因此会降低性能。

### 线程池实践规范

线程池必须手动通过 `ThreadPoolExecutor` 的构造函数来声明，避免使用`Executors`类创建线程池，会有 OOM 风险。**使用有界队列，控制线程创建数量。**

除了避免 OOM 的原因之外，不推荐使用 `Executors`提供的两种快捷的线程池的原因还有：

- 实际使用中需要根据自己机器的性能、业务场景来手动配置线程池的参数比如核心线程数、使用的任务队列、饱和策略等等。
- 我们应该**显示地给我们的线程池命名**，这样有助于我们定位问题。

不同的业务使用不同的线程池，配置线程池的时候根据当前业务的情况对当前线程池进行配置，因为不同的业务的并发以及对资源的使用情况都不同，重心优化系统性能瓶颈相关的业务。如果**父业务和子业务调用同一个线程池，可能产生死锁；**

当线程池不再需要使用时，应该**显式地关闭线程池**，释放线程资源。

调用完 `shutdownNow` 和 `shuwdown` 方法后，并不代表线程池已经完成关闭操作，它只是异步的通知线程池进行关闭处理。如果要同步等待线程池彻底关闭后才继续往下执行，需要调用`awaitTermination`方法进行同步等待。

在调用 `awaitTermination()` 方法时，应该设置合理的超时时间，以避免程序长时间阻塞而导致性能问题。另外。由于线程池中的任务可能会被取消或抛出异常，因此在使用 `awaitTermination()` 方法时还需要进行异常处理。`awaitTermination()` 方法会抛出 `InterruptedException` 异常，需要捕获并处理该异常，以避免程序崩溃或者无法正常退出。

线程池本身的目的是为了**提高任务执行效率，避免因频繁创建和销毁线程而带来的性能开销**。如果将耗时任务提交到线程池中执行，可能会导致线程池中的线程被长时间占用，无法及时响应其他任务，甚至会导致线程池崩溃或者程序假死。

因此，在使用线程池时，我们应该**尽量避免将耗时任务提交到线程池中执行**。对于一些比较耗时的操作，如网络请求、文件读写等，可以采用**异步操作**的方式来处理，以避免阻塞线程池中的线程

线程池和 `ThreadLocal`共用，可能会导致线程从`ThreadLocal`获取到的是旧值/脏数据。这是因为线程池会复用线程对象，与线程对象绑定的类的静态属性 `ThreadLocal` 变量也会被重用，这就导致一个线程可能获取到其他线程的`ThreadLocal` 值。

## Future类

`Future` 类是**异步**思想的典型运用，主要用在一些需要执行耗时任务的场景，**避免程序一直原地等待耗时任务执行完成**，执行效率太低。具体来说是这样的：当我们执行某一耗时的任务时，可以**将这个耗时任务交给一个子线程去异步执行**，同时我们可以干点其他事情，不用傻傻等待耗时任务执行完成。等我们的事情干完后，我们再**通过 `Future` 类获取到耗时任务的执行结果**。这样一来，程序的执行效率就明显提高了。这其实就是**多线程中**经典的 **Future 模式**，你可以将其看作是**一种设计模式，核心思想是异步调用，主要用在多线程领域**，并非 Java 语言独有。

在 Java 中，`Future` 类只是一个泛型接口，位于 `java.util.concurrent` 包下，其中定义了 5 个方法，主要包括下面这 4 个功能：

- 取消任务；
- 判断任务是否被取消;
- 判断任务是否已经执行完成;
- 获取任务执行结果。

**FutureTask 提供了 Future 接口的基本实现**，常用来封装 Callable 和 Runnable。`ExecutorService.submit()` 方法返回的其实就是 `Future` 的实现类 `FutureTask` 。FutureTask 实现了`Runnable` 接口，因此可以作为任务直接被线程执行。

`FutureTask` 有两个构造函数，可传入 `Callable` 或者 `Runnable` 对象。实际上，传入 `Runnable` 对象也会在方法内部转换为`Callable` 对象。

**`FutureTask`相当于对`Callable` 进行了封装，管理着任务执行的情况，存储了 `Callable` 的 `call` 方法的任务执行结果。**

### CompletableFuture

`Future` 在实际使用过程中存在一些局限性比如不支持异步任务的编排组合、获取计算结果的 `get()` 方法为阻塞调用。

Java 8 才被引入`CompletableFuture` 类可以解决`Future` 的这些缺陷。`CompletableFuture` 除了提供了更为好用和强大的 `Future` 特性之外，还提供了**函数式编程、异步任务编排组合**（可以将多个异步任务串联起来，组成一个完整的链式调用）等能力。

`CompletableFuture` 同时实现了 `Future` 和 `CompletionStage` 接口。

<img src="Java\completablefuture-class-diagram.jpg" alt="img" style="zoom:80%;" />

`CompletionStage` 接口描述了一个异步计算的阶段。很多计算可以分成多个阶段或步骤，此时可以通过它将所有步骤组合起来，形成异步计算的流水线。

## AQS

AQS 的全称为 `AbstractQueuedSynchronizer` ，翻译过来的意思就是抽象队列同步器。这个类在 `java.util.concurrent.locks` 包下面。**AQS 就是一个抽象类，主要用来构建锁和同步器。**使用 AQS 能简单且高效地构造出应用广泛的大量的同步器，比如我们提到的 `ReentrantLock`，`Semaphore`，其他的诸如 `ReentrantReadWriteLock`，`SynchronousQueue`等等皆是基于 AQS 的。

### 原理

AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为**有效的工作线程**，并且将共享资源设置为**锁定状态**。如果被请求的共享资源被占用，那么就需要一套**线程阻塞等待以及被唤醒时锁分配**的机制，这个机制 AQS 是用 **CLH 队列锁** 实现的，即将暂时**获取不到锁的线程加入到队列**中。

CLH(Craig,Landin,and Hagersten) 队列是一个**虚拟的双向队列**（虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系）。AQS 是将每条请求共享资源的线程封装成一个 **CLH 锁队列的一个结点**（Node）来实现锁的分配。在 CLH 同步队列中，**一个节点表示一个线程**，它保存着线程的引用（thread）、 当前节点在队列中的状态（waitStatus）、前驱节点（prev）、后继节点（next）。

<img src="Java\40cb932a64694262993907ebda6a0bfe~tplv-k3u1fbpfcp-zoom-1.png" alt="img" style="zoom: 80%;" />

<img src="Java\CLH.png" alt="img" style="zoom:80%;" />

AQS 使用 **int 成员变量 `state` 表示同步状态**，通过内置的 **线程等待队列** 来完成获取资源线程的排队工作。

`state` 变量由 `volatile` 修饰，用于展示当前临界资源的获锁情况。

```java
// 共享变量，使用volatile修饰保证线程可见性
private volatile int state;
```

另外，状态信息 `state` 可以通过 `protected` 类型的`getState()`、`setState()`和`compareAndSetState()` 进行操作。并且，这几个方法都是 `final` 修饰的，在子类中无法被重写。

### Semaphore 

`synchronized` 和 `ReentrantLock` 都是一次只允许一个线程访问某个资源，而`Semaphore`(信号量)可以用来**控制同时访问特定资源的线程数量。**

Semaphore 的使用简单，我们这里假设有 N(N>5) 个线程来获取 `Semaphore` 中的共享资源，下面的代码表示同一时刻 N 个线程中只有 5 个线程能获取到共享资源，其他线程都会阻塞，只有获取到共享资源的线程才能执行。等到有线程释放了共享资源，其他阻塞的线程才能获取到。

```java
// 初始共享资源数量
final Semaphore semaphore = new Semaphore(5);
// 获取1个许可
semaphore.acquire();
// 释放1个许可
semaphore.release();
```

当初始的资源个数为 1 的时候，`Semaphore` 退化为排他锁。

`Semaphore` 有两种模式：。

- **公平模式：** 调用 `acquire()` 方法的顺序就是获取许可证的顺序，遵循 FIFO；
- **非公平模式：** 抢占式的。

`Semaphore` 通常用于那些资源有明确访问数量限制的场景比如限流（仅限于单机模式，实际项目中推荐使用 Redis +Lua 来做限流）

`Semaphore` 是共享锁的一种实现，它默认构造 AQS 的 `state` 值为 `permits`，你可以将 `permits` 的值理解为**许可证的数量**，只有拿到许可证的线程才能执行。

### CountDownLatch 

`CountDownLatch` 允许 `count` 个线程阻塞在一个地方，直至所有线程的任务都执行完毕。

`CountDownLatch` 是一次性的，计数器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当 `CountDownLatch` 使用完毕后，它不能再次被使用。

**原理**

`CountDownLatch` 是共享锁的一种实现,它默认构造 AQS 的 `state` 值为 `count`。当线程使用 `countDown()` 方法时,其实使用了`tryReleaseShared`方法以 CAS 的操作来减少 `state`,直至 `state` 为 0 。当调用 `await()` 方法的时候，如果 `state` 不为 0，那就证明任务还没有执行完毕，`await()` 方法就会一直阻塞，也就是说 `await()` 方法之后的语句不会被执行。**直到`count` 个线程调用了`countDown()`使 state 值被减为 0，或者调用`await()`的线程被中断，该线程才会从阻塞中被唤醒，`await()` 方法之后的语句得到执行。**

详解：https://blog.csdn.net/hbtj_1216/article/details/109655995

## 参考

https://javaguide.cn/

泛型：[Java 中的泛型（两万字超全详解）_java 泛型-CSDN博客](https://blog.csdn.net/weixin_45395059/article/details/126006369)

反射：[Java反射详解-CSDN博客](https://blog.csdn.net/weixin_74268571/article/details/131345164)

volatile：[Java中的volatile_java volatile-CSDN博客](https://blog.csdn.net/m0_49183244/article/details/125493673)