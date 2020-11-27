# *Objective-C_Notes*

## 1、Objective-C代码的文件扩展名

| 扩展名 |                           内容类型                           |
| :----: | :----------------------------------------------------------: |
| **.h** |        头文件。头文件包含类，类型，函数和常数的声明。        |
| **.m** | 源代码文件。这是典型的源代码文件扩展名，可以包含 Objective-C 和 C 代码。 |

\#import 选项和 #include 选项完全相同，只是它可以确保相同的文件只会被包含一次。



## 2、消息传递

**C++：**类别与方法的关系严格清楚，一个方法必定属于一个类别，在编译时就已经紧密绑定。

**Objective-C：**类别与消息的关系比较松散，调用方法视为对对象发送消息，所有方法都被视为对消息的回应。所有消息处理直到运行时（runtime）才会动态决定，并交由类别自行决定如何处理收到的消息。

```c++
obj.method(argument);  //c++
```

```objective-c
[obj method: argument];  //objective-c
```

**区别：**

- C++强制要求所有的方法都必须有对应的动作，且编译期绑定使得函数调用非常快速。

- Objective-C允许发送未知消息给对象，可以发送消息给整个对象集合而不需要一一检查每个对象的类型，也具备消息转送机制。



## 3、字符串

**NSString类**提供了字符串的类包装，包括对保存任意长度字符串的内建内存管理机制，支持Unicode，printf风格的格式化工具，等等。

```objective-c
NSString* myString = @"My String\n";
NSString* anotherString = [NSString stringWithFormat:@"%d %s", 1, @"String"];
```



## 4、类

Objective-C 的类规格说明包含了两个部分：定义（interface）与实现（implementation）。

- 定义（interface）部分包含了类声明和实例变量的定义，以及类相关的方法。
- 实现（implementation）部分包含了类方法的实际代码。

### 4.1 类的定义

类声明总是由 `@interface `编译选项开始，由 `@end `编译选项结束。

类名之后的（用冒号分隔的）是父类的名字。类的实例（或者成员）变量声明在被大括号包含的代码块中。实例变量块后面就是类声明的方法的列表。每个实例变量和方法声明都以分号结尾。

- 加号（+）代表类方法（class method），不需要实例就可以调用，与C++ 的静态函数相似。
- 减号（-）即是一般的实例方法（instance method）。

```objective-c
@interface MyObject : NSObject {
    int memberVar1; // 实体变量
    id  memberVar2;
}

+(return_type) class_method; // 类方法

-(return_type) instance_method1; // 实例方法
-(return_type) instance_method2: (int) p1;
-(return_type) instance_method3: (int) p1 andPar: (int) p2;
@end
```

Objective-C定义一个新的方法时，名称内的冒号`:`代表参数传递。Objective-C方法使得参数可以夹杂于名称中间，不必全部附缀于方法名称的尾端，可以提高程序可读性。

```objective-c
- (void) setColorToRed: (float)red Green: (float)green Blue:(float)blue; /* 宣告方法*/

[myColor setColorToRed: 1.0 Green: 0.8 Blue: 0.2]; /* 呼叫方法*/
```

### 4.2 类的实现

实现区块则包含了公开方法的实现，以及定义私有变量及方法。 以关键字`@implementation`作为区块起头，`@end`结尾。

```objective-c
@implementation MyObject {
  int memberVar3; //私有成员函数
}

+(return_type) class_method {
    .... //method implementation
}
-(return_type) instance_method1 {
     ....
}
-(return_type) instance_method2: (int) p1 {
    ....
}
-(return_type) instance_method3: (int) p1 andPar: (int) p2 {
    ....
}
@end
```

**注意：**定义于Interface区块内的实体变量默认权限为protected，定义于implementation区块的实体变量则默认为private。故在Implementation区块定义私有成员更匹配面向对象之封装原则，因为如此类别之私有信息就不需曝露于公开interface（.h文件）中。

### 4.3 创建对象

Objective-C创建对象需通过`alloc`以及`init`两个消息。

alloc的作用是分配内存，init则是初始化对象。 

init与alloc都是定义在NSObject里的方法，父对象收到这两个信息并做出正确回应后，新对象才创建完毕。

```objective-c
MyObject * my = [[MyObject alloc] init];
```

## 5、方法

- 实例方法：在类的一个具体实例的范围内执行。在调用一个实例方法前，必须首先创建类的一个实例。

- 类方法：不需要创建一个实例。

![2011022010310145.jpg](https://www.runoob.com/wp-content/uploads/2015/08/2011022010310145.jpg)

声明由一个减号(-)开始，这表明这是一个实例方法。

方法实际的名字`insertObject:atIndex:`是所有方法标识关键的级联，包含了冒号。

冒号表明了参数的出现。如果方法没有参数，可以省略第一个(也是唯一的)方法标识关键字后面的冒号。本例中，这个方法有两个参数。



发送给对象的所有消息都会动态分发，如果子类定义了跟父类的具有相同标识符的方法，那么子类首先收到消息，然后可以有选择的把消息转发（也可以不转发）给他的父类。

消息被中括号`[ ] `包括。中括号中间，接收消息的对象在左边，消息（包括消息需要的任何参数）在右边。

```objective-c
[myArray insertObject:anObj atIndex:0];
```

