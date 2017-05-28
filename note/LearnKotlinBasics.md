# Kotlin学习笔记——基础

## 一、基本类型

### 数字类型
- kotlin没有隐式类型装换
- 字符在kotlin中不是数字类型
- 类型与机器长度表：</br>

type|length
:---|:---
Double|64
Float|32
Long|64
Int|32
Short|16
Byte|8

- kotlin不支持八进制字面值
- 可以通过使用下划线来分割数字字符串，让字面值更容易阅读
- 在Java平台上，数字是直接被转换为JVM对应实现进行存储，当需要一个可为空的数字类型时，数字会被装箱。
- 由于不同数字类型的表示不同，导致数字类型间不支持隐式类型装换，每个数字类型都有对应的转换方法如下：

method|Type
:---|:---
toByte()|Byte
toShort()|Short
toInt()|Int
toLong()|Long
toFloat()|Float
toDouble()|Double
toChar()|Char

- 位操作居然没有操作符号，是用函数实现的！！！
 - shl()：shift left，等同于java <<
 - shr()：shift right，等同于java >>
 - ushr()：unsigned shift right，等同于java >>>
 - and()：&
 - or()：|
 - xor()：^
 - inv(): ~

### 字符类型
- 字符类型不被直接当做数字类型处理,需要调用相应的转换函数

### Booleans类型
- 只支持惰性运算符：&&、||、！，不支持：&、|

### 数组类型
- arrayOf()：快速构建数组
- arrayOfNulls():快速构建空数组
- Array(size: Int, function: Lamda)
- 在每一个数组上调用的[]都会调用get、set方法
- Array是不可变的

### 字符类型
- 两种字面值："string\n"、"""string"""，效果和python相同
- trimMargin(c: Char)：去除空格个分割符号
- 字符模板：$变量，比如：val a=1 那么："a=$a"就表示a=1

## 二、package
- import...as...，python类似处理冲突的机制

## 三、控制流
#### if表达式
在Kotlin中，if是可用作为表达式（具有返回值）和语句块，作为表达式时表达式的值是if语句块最后一条语句。<br />
例如：
```Kotlin
  var max = if(a > b) a else b
```
等同于Java等C系语言中：
```Kotlin
  var max: Int
  if(a > b){
    max = a
  }else{
    max = b
  }
```
需要注意的是，作为表达式使用的if必须要有完整的if-[else if]-else结构

### when表达式
when作为代替C系语言中的switch语法结构的角色，同if一样，即可作为表达式也可以作为语句块。
when依次检查分支直到遇到满足条件的分支，如果作为表达式使用，else分支是必须的。<br />
<br />
如果多分支执行同样的逻辑，则可以通过逗号（,）进行条件合并，例如：
```Kotlin
when(x){
  0,1 -> println("x == 0 or x == 1")
  else -> println("otherwise")
}
```
条件除了常量外，还可以是任意表达式，只要表达式的值可以和when操作的变量类型相容。<br />
<br />
is、in等操作符都可以直接方法分支中。<br />
<br />
可以用when代替传统的if-[else if...]-else结构，当when不存在操作变量时，解释为if-else if
如下：
```Kotlin
when{
  x.isOdd() -> println("x is odd")
  x.isEven() -> println("x is even")
  else -> println("x is funny")
}
```
### for循环(迭代器循环)
for可以通过迭代器（iterator）迭代任何事物<br />
可以被for进行迭代循环的对象需要满足以下条件：
- 有一个名叫iterator()的成员方法或者拓展方法（C#语法类型）
- iterator()方法返回类型必须：
  - 有名为next()的成员方法或者拓展方法
  - 有返回值为Boolean类型的hasNext()的成员方法或者拓展方法

for循环数组时会被编译成基于索引的的循环，这其中不会创建iterator<br />
<br />
但需要显示基于索引迭代数组时，可以这样写：
```Kotlin
for (i in array.indices){
  println(array[i])
}
```
使用withIndex()库方法完成索引和值同时迭代：
```Kotlin
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```
### While循环
Kotlin支持与C系语言一样的while和do...while语法
### 循环打断和继续
Kotlin支持在循环中使用continue和break中断或者继续循环流程。

## 四、返回和跳转（Returns and Jumps）
标签break和continue，Kotlin通过在标签名后加@来加入标签，例如：
```Kotlin
loop@ for(i in 1..100){
  //...
}
```
可以使用break@便签名，进行跳出，continue@标签名进入下一步流程。<br />
<br />
Kotlin支持便签返回，在lambda表达式中，返回表达式本身而不是函数返回，可以使用标签返回：
```Kotlin
fun foo(ints: IntArray){
  ints.forEach{
    if(it == 0) return //lambda中的返回
    println(it)
  }
  println("test")
}
```
上面代码中lambda中的返回会直接将foo()函数返回,当ints数组中存在0时永远不会打印test
使用标签返回lambda表达式：
```Kotlin
fun foo(ints: IntArray){
  ints.forEach label@{
    if(it == 0) return@label //lambda中的返回,也可以写为return@foreach
    println(it)
  }
  println("test")
}
```
上述代码只会返回lambda表达式层，而不会返回到foo()函数层，这就是标签返回。更一般的会使用隐式标签返回，即return@lambda层函数名<br />
<br />
匿名函数的返回就只返回到匿名函数层。
```Kotlin
fun foo() {
    ints.forEach(fun(value: Int) {
        if (value == 0) return
        print(value)
    })
    println("test")
}
```
上面的代码使用匿名函数代替lambda，其中匿名函数的返回只限于匿名函数本身（lambda函数实现不压入方法栈？？？）
带返回值的标签返回：
```Kotlin
return@label value
```
