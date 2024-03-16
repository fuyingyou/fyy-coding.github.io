---
title: Lambda表达式和Stream API
author: 
tags: 
       - java

category: 
       - Java学习

date: 2023-08-19 16:26:01
---
### Lambda表达式

**1. 有且仅有一个参数时，小括号可以省略（无参数时，小括号不能省略）**

**2. 语句只有一条时，可以省略大括号和return**
```js 
Runnable runnable = ()-> System.out.println("Hello,World!");

BinaryOperator<Long> bo1 = x -> x+1;

BinaryOperator<Long> bo2 = (x,y) -> x+y;

BinaryOperator<Long> bo3 = (x,y) -> {
    
	System.out.println("Hello,World!");
	return x+y;
};
```
 
Lambda 表达式中无需指定参数类型，程序依然可以编译，这是因为 javac 根据程序的上下文，在后台推断出了参数的类型。

### Stream API

“集合讲的是数据，流讲的是计算！”

##### 创建

Collection创建

```js 
stream() 			//返回一个顺序流
parallelStream()	//返回一个并行流
```

Array创建

```js 
stream(T[] array)	//返回一个流
```

值创建

```js 
public static<T> Stream<T> of(T... values)		//返回一个流
```

函数创建无限流

```js 
// 迭代
public static<T> Stream<T> of(T... values)

// 生成
public static<T> Stream<T> generate(Supplier<T> s)
```

##### 中间操作

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”。

筛选与切片(filter、distinct、limit、skip)

```js 
List<Integer> integerList = Arrays.asList(4, 5, 6, 7, 7, 7, 8, 8, 9);

//filter(Predicate p)接收 Lambda，从流中排除某些元素。
Stream<Integer> integerStream = integerList.stream().filter(x -> x > 6);
System.out.println(integerStream.collect(Collectors.toList()));//[7, 7, 7, 8, 8, 9]

//distinct()筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
Stream<Integer> distinctStream = integerList.stream().distinct();
System.out.println(distinctStream.collect(Collectors.toList()));//[4, 5, 6, 7, 8, 9]

//limit(long maxSize) 截断流，使其元素不超过给定数量。
Stream<Integer> limitStream = integerList.stream().limit(5);
System.out.println(limitStream.collect(Collectors.toList()));//[4, 5, 6, 7, 7]

//skip(long n)跳过元素，返回一个扔掉了前 n 个元素的流。
//				若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
Stream<Integer> skipStream = integerList.stream().skip(5);
System.out.println(skipStream.collect(Collectors.toList()));//[7, 8, 8, 9]
```

映射（map、flatMap）

```js 
public static Stream<Character> filterCharacter(String str){
    
    List<Character> list = new ArrayList<>();
    for (Character ch : str.toCharArray()) {
    
        list.add(ch);
    }
    return list.stream();
}

@Test
public void test2(){
    
    List<String> stringsList = Arrays.asList("aaa", "bbb");

    //map(Function f 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
    Stream<String> mapStream = stringsList.stream().map(String::toUpperCase);
    System.out.println(mapStream.collect(Collectors.toList()));//[AAA, BBB]


    //flatMap(Function f)  接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
    Stream<Character> characterStream = stringsList.stream().flatMap(StreamTest::filterCharacter).map(Character::toUpperCase);
    System.out.println(characterStream.collect(Collectors.toList()));//[A, A, A, B, B, B]
}
```

排序(sorted)

```js 
//sorted() 生一个新流，其中按自然顺序排序
List<Integer> integerList = Arrays.asList(7, 4, 5, 2, 6, 9);
Stream<Integer> sorted = integerList.stream().sorted();
System.out.println(sorted.collect(Collectors.toList()));//[2, 4, 5, 6, 7, 9]

//sorted(Comparator comp) 产生一个新流，其中按比较器顺序排序
Stream<Integer> sorted1 = integerList.stream().sorted((a, b) -> b - a);
System.out.println(sorted1.collect(Collectors.toList()));//[9, 7, 6, 5, 4, 2]
```

##### 终止操作(allMatch、anyMatch、noneMatch、findFirst、findAny、count…)

```js 
List<Integer> integerList = Arrays.asList(7, 4, 5, 2, 6, 9);
//检查是否匹配所有元素
boolean b = integerList.stream().allMatch(s -> s == 6);
//检查是否至少匹配一个元素
boolean b1 = integerList.stream().anyMatch(s -> s == 6);
//检查是否没有匹配所有元素
boolean b2 = integerList.stream().noneMatch(s -> s == 6);
//返回第一个元素
Optional<Integer> first= integerList.stream().findFirst();
//返回当前流中的任意元素
Optional<Integer> any = integerList.stream().findAny();
//返回流中元素总数
long count = integerList.stream().count();
//返回流中最大值
Optional<Integer> max = integerList.stream().max(Integer::compare);
//返回流中最小值
Optional<Integer> min = integerList.stream().min(Integer::compare);
System.out.println(b);//false
System.out.println(b1);//true
System.out.println(b2);//false
System.out.println(first.get());//7
System.out.println(any.get());//7
System.out.println(count);//6
System.out.println(max.get());//9
System.out.println(min.get());//2
//内部迭代
integerList.stream().forEach(System.out::print);//745269
```
 
1. Stream 自己不会存储元素。
1. Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
1. Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。