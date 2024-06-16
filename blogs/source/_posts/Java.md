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



## 参考

https://javaguide.cn/

泛型：[Java 中的泛型（两万字超全详解）_java 泛型-CSDN博客](https://blog.csdn.net/weixin_45395059/article/details/126006369)

反射：[Java反射详解-CSDN博客](https://blog.csdn.net/weixin_74268571/article/details/131345164)