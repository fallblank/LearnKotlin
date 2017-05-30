# Kotlin学习笔记——面向对象

## 一、类和继承
### 类
Kotlin中类的声明通过3部分组成：
  - 类名
  - 类的头部
  - 类体

比如：
```Kotlin
class Kotlin(val version: String){

}
```
一个类可以拥有两种构造函数：
- 主构造函数：Primary constructor
- 次构造函数：Secondary constructor

主构造函数最多有一个，次构造函数可以有多个。如下：
```Kotlin
class Kotlin constructor(version: Float){
  init{
    //主构造函数初始化代码块
  }

  constructor(author: String) : this(1.1){
    //次构造函数 1，直接委托主构造函数
  }

  constructor() : this("JetBrain"){
    //次构造函数 2，间接委托主构造函数
  }

}
```
主构造函数是在类的声明的头部中决定，当没有权限声明和注解时，主构造函数的constructor可以忽略，即保留括号()说明存在主构造函数。<br />
<br />
次构造函数需要直接或者间接调用主构造函数。<br />
<br />
默认情况下所有构造函数的访问权限都是public的，可以显示更改。

### 继承
- 所有没加open限定的非抽象类都是final类型的，不能被继承。
- 所有没加open的非抽象方法都是final的，不能被重写（override）
- 重写的方法前都需要加override。
- 可以显示加入final中断方法的override链
- 当继承得到多个同名方法时，需要显示重写同名方法，通过super<supertype>来调用不用父类
- 在类前加abstract表明类为抽象，抽象类默认是open的。
