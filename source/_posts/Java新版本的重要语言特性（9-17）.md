---
title: Java新版本的重要语言特性（9-17）
author: 
tags: 
       - java

category: 
       - java

date: 2023-09-21 09:06:04
---
## JDK9

#### 允许在接口中使用私有方法

## JDK10

#### 局部变量类型推断

```js 
var list = new ArrayList<String>();
```
 
* 声明时必须初始化

可以使用在：

* 具有初始化器的局部变量
* 增强型 for 循环中的索引变量
* 传统 for 循环中声明的局部变量

不能使用在：

* 推断方法的参数类型
* 构造函数参数类型推断
* 推断方法返回类型
* 字段类型推断
* 捕获表达式（或任何其他类型的变量声明）

建议：为了程序的易读性和可维护性，尽量显式定义变量类型。

## JDK11

#### 用于 Lambda 参数的局部变量语法

将局部变量和 Lambda 表达式的用法进行了统一，并且可以将注释应用于局部变量和 Lambda 表达式
 
```js 
@Nonnull var x = new Foo();

(@Nonnull var x, @Nullable var y) -> x.process(y)
```
 
* @NonNull 注解可以标注在方法、字段、参数之上，表示对应的值不能为空；
* @Nullable 注解可以标注在方法、字段、参数之上，表示对应的值可以为空；

## JDK12

## JDK13

## JDK14

#### Switch 表达式

旧版：

* 一般使用冒号 ：来作为语句分支代码的开始。
* 在每个分支结束之前，需要加上 break 关键字进行分支跳出，以防 switch 语句一直往后执行到整个 switch 语句结束。

新版：

* 提供了新的分支切换方式，即 -> 符号右则表达式方法体。
* 在执行完分支方法之后，自动结束 switch 分支。
* -> 右则方法块中可以是表达式、代码块或者是手动抛出的异常。
```js 
//旧版：
int dayOfWeek;
switch (day) {
    
    case MONDAY:
    case TUESDAY:
    case WEDNESDAY:
    case THURSDAY:
    case FRIDAY:
        dayOfWeek = 5;
        break;
    case SATURDAY:
        dayOfWeek = 6;
        break;
    case SUNDAY:
        dayOfWeek = 7;
        break;
    default:
        dayOfWeek = 0;
        break;
}

//新版：
int dayOfWeek = switch (day) {
    
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> 5;
    case SATURDAY -> 6;
	case SUNDAY -> 7;
    default -> 0;
};
```

**注意**：

* 之前需要用变量来接收返回值，现在直接返回需要返回的结果。
* 不再需要显式地跳出当前分支，表达式执行完后会自动跳出，不会继续往后执行。
* 对于多个相同的 case 方法块，可以将 case 条件并列，不再通过每个 case 后面故意不加 break 关键字来使用相同方法块。

## JDK15

#### 文本块

文本块就是以三个引号开始，并以三个引号结束的字符串字面量。
文本块减少了转义，大大提高了代码可读性，尤其是代码中对SQL、HTML、JSON字符串进行拼接的情况。
 
```js 
// 旧版
String sqlTemplate = "SELECT\n" +
        "    name,\n" +
        "    age,\n" +
        "    phone,\n" +
        "    wechat\n" +
        "FROM\n" +
        "    csdn_user;";
// 新版
String sqlTemplate = """
        SELECT
            name,
            age,
            phone,
            wechat
        FROM
            csdn_user;
        """;
```

## JDK16

#### instanceof 模式匹配

对 instanceof 的改进，主要目的是为了让创建对象更简单、简洁和高效，并且可读性更强、提高安全性。
 
```js 
// 旧版
// 每次在检查类型之后，都需要强制进行类型转换。
// 类型转换后，需要提前创建一个局部变量来接收转换后的结果，代码显得多余且繁琐。
if (person instanceof Student) {
    
    Student student = (Student) person;
    student.say();
   // other student operations
} else if (person instanceof Teacher) {
    
    Teacher teacher = (Teacher) person;
    teacher.say();
    // other teacher operations
}

// 新版
// 对 person 对象进行类型匹配，校验 person 对象是否为 Student 类型
// 如果类型匹配成功，则会转换为 Student 类型，并赋值给模式局部变量 student
if (person instanceof Student student) {
    
	//这里的 student 变量只能在 if 块中使用，而不能在 else if/else 中使用
    student.say();
   // other student operations
} else if (person instanceof Teacher teacher) {
    
    teacher.say();
    // other teacher operations
}
```

如果 if 条件中有 && 运算符时，当 instanceof 类型匹配成功，模式局部变量的作用范围也可以相应延长，如下面代码：

```js 
if (obj instanceof String s && s.length() > 5) {
    .. s.contains(..) ..}
```
 
注意：这种作用范围延长，并不适用于或 || 运算符，因为即便 || 运算符左边的 instanceof 类型匹配没有成功也不会造成短路，依旧会执行到||运算符右边的表达式.但是如果左边instanceof 类型匹配没有成功，局部变量并未定义赋值，此时使用会产生问题。

#### Records类型

* Record 类型允许在代码中使用紧凑的语法形式来声明类，而这些类能够作为不可变数据类型的封装持有者。Record 这一特性主要用在特定领域的类上；
* 与枚举类型一样，Record 类型是一种受限形式的类型，主要用于存储、保存数据，并且没有其它额外自定义行为的场景下。
* 效果有些类似 Lombok 的 @Data 注解、Kotlin 中的 data class，但是又不尽完全相同，它们的共同点都是类的部分或者全部可以直接在类头中定义、描述，并且这个类只用于存储数据而已。
 
```js 
// 示例
public record Person(String name, int age) {
    
    public static String address;

    public String getName() {
    
        return name;
    }
}


// 编译后反编译的结果
public final class Person extends java.lang.Record {
    
    private final java.lang.String name;
    private final java.lang.String age;

    public Person(java.lang.String name, java.lang.String age) {
     /* compiled code */ }

    public java.lang.String getName() {
     /* compiled code */ }

    public java.lang.String toString() {
     /* compiled code */ }

    public final int hashCode() {
     /* compiled code */ }

    public final boolean equals(java.lang.Object o) {
     /* compiled code */ }

    public java.lang.String name() {
     /* compiled code */ }

    public java.lang.String age() {
     /* compiled code */ }
}
```

可以得出，当用 Record 来声明一个类时，该类将自动拥有下面特征：

* 拥有一个构造方法
* 获取成员属性值的方法：name()、age()
* hashCode() 方法和 euqals() 方法
* toString() 方法类
* 对象被 final 关键字修饰，不能被继承；类的成员变量也都被 final 修饰，不能再被赋值使用。
* 可以在 Record 声明的类中定义静态属性和方法。
* 注意，不能在 Record 声明的类中定义成员变量，类也不能声明为抽象类等。

## JDK17

#### 密封的类和接口

用来增强 Java 编程语言，防止其他类或接口扩展或实现它们。

使用修饰符**sealed**，您可以将一个类声明为密封类。
密封的类使用关键字**permits**列出可以直接扩展它的类。
子类可以是最终的、非密封的或密封的。
**继承了密封类的子类可以使用non-sealed修饰，这样任何类都可以继承这个子类。**

```js 
// 旧版
public class Person {
     } //人
 
class Teacher extends Person {
     }//教师
 
class Worker extends Person {
     }  //工人
 
class Student extends Person{
     } //学生


// 新版
// 添加sealed修饰符，permits后面跟上只能被继承的子类名称
public sealed class Person permits Teacher, Worker, Student{
     } //人
 
// 子类可以被修饰为 final
final class Teacher extends Person {
     }//教师
 
// 子类可以被修饰为 non-sealed，此时 Worker类就成了普通类，谁都可以继承它
non-sealed class Worker extends Person {
     }  //工人
// 任何类都可以继承Worker
class AnyClass extends Worker{
    }
 
//子类可以被修饰为 sealed,同上
sealed class Student extends Person permits MiddleSchoolStudent,GraduateStudent{
     } //学生

final class MiddleSchoolStudent extends Student {
     }  //中学生

final class GraduateStudent extends Student {
     }  //研究生
```
 
可以限制类的层次结构。