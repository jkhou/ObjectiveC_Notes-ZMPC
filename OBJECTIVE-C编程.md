# OBJECTIVE-C编程

## 1.对象

框架是由多种相关的类、函数、常量以及数据类型组成的库。Foundation框架包含了许多基础的类。

Xcode创建项目的时候会自动导入Foundation框架。

```objective-c
#import<Foundation/Foundation.h>
```

- `#include`会告诉编译器做呆板的复制粘贴，将包含的内容粘贴到目标文件夹中。
- `#import`会让编译器先检查之间是否已经导入过这个文件，或是已经被包含到目标文件中。

### 消息发送

​	消息发送指令必须写在一对方括号中，并且必须包含接收方与选择器。

```objective-c
NSDate *now = [NSDate date]; //向NSDate类发送了date消息，让它执行date方法。
NSLog(@"This NSDate object lives at %p",now);
NSLog(@"The date is %@",now);
```

- **NSDate接收方**：指针，指向接收消息的对象的地址。
- **date选择器**：方法名，要触发的方法的方法名。
- **%@**会输出相应对象的描述信息。



### 类方法与实例方法

```objective-c
NSDate *now = [NSDate date]; //向NSDate类发送了date消息，让它执行date方法。
double seconds = [now timeIntervalSince1970]; 
NSLog(@"It has been %f seconds since the start of 1970.", seconds);
```

- **NSDate**类发送了**date**消息，**date**是一个类方法。类方法会创建类的实例，并初始化实例变量。
- 向now变量指向的**NSDate**实例发送了**timeIntervalSince1970**，是一个**实例方法**。



### 语言命名习惯

使用**驼峰命名法**。

指向实例的变量：now、weightLifter、myCurrentLocation。

方法的命名：date、bodyMassIndex、timeIntervalSince1970。

类的名称以大写字母开头：NSDate、Person、CLLocation。



## 2.消息

### 传递实参的消息

```objective-c
NSDate *now = [NSDate date]; //向NSDate类发送了date消息，让它执行date方法。
double seconds = [now timeIntervalSince1970]; 
NSLog(@"It has been %f seconds since the start of 1970.", seconds);
NSDate *later = [now dateByAddingTimeInterval:100000];
NSLog(@"In 100,000 seconds it will be %@.", later);
```

冒号**：**的意思是要向**dateByAddingTimeInterval:**方法传入一个实参。

### 多个实参

```objective-c
NSCalendar *cal = [NSCalendar currentCalendar];
unsigned long day = [cal ordinalityOfUnit:NSDayCalendarUnit
                    			   inUnit:NSMonthCalendarUnit
                                  forDate:now]
```

这个方法拥有三个实参，所以它的名字由三部分组成，但是它是一条消息，只能触发一个方法。

### 消息的嵌套发送

```objective-c
NSDate *now = [NSDate date]; //向NSDate类发送了date消息，让它执行date方法。
double seconds = [now timeIntervalSince1970]; 
NSLog(@"It has been %f seconds since the start of 1970.", seconds);
```

可以将上面的两条消息通过一行代码来实现**嵌套发送**。

```objective-c
double seconds = [[NSDate date] timeIntervalSince1970]; 
NSLog(@"It has been %f seconds since the start of 1970.", seconds);
```

- 嵌套发送的消息，系统会先执行最里面的消息，然后按由内到外的顺序依次执行外层的消息；
- 系统先向**NSDate**类发送date消息，然后向得到的返回值（即指向新创建的**NSDate**实例的指针）发送**timeIntervalSince1970**消息。

### alloc和init

唯一必须以嵌套的形式连续发送的消息是**alloc**和**init**。

### 消息的嵌套发送

```objective-c
//NSDate *now = [NSDate date]; //向NSDate类发送了date消息，让它执行date方法。
NSDate *now = [[NSDate alloc] init];
```

可以将上面的两条消息通过一行代码来实现**嵌套发送**。

每个类都有一个**alloc**类方法。它能够创建一个新的对象，并返回指向该对象的指针。通过**alloc**创建的对象，必须经过初始化才能使用。

每个类也有一个**init**实例方法。它用来初始化实例。



## 3.NSString

```objective-c
NSString *lament = @"Why me!?";
```

`@"..."`是Objective-C中的一个缩写，代表根据给定的字符串创建一个**NSString**对象。

```objective-c
- (NSUInteger)length;//获取字符串中字符的数量
```

```objective-c
- (BOOL)isEqualToString:(NSString *)other;//查看一个字符串是否和另一个字符串相等
```

```objective-c
- (NSString *)uppercaseString; //把一个字符串变成大写形式
```



## 4.NSArray

**NSArray**实例可以保存一组指向其他对象的指针。

### 创建数组

```objective-c
//创建三个NSDate对象
NSDate *now = [NSDate date]; 
NSDate *tomorrow = [now dateByAddingTimeInterval:24.0*60.0*60.0];
NSDate *yesterday = [now dateByAddingTimeInterval:-24.0*60.0*60.0];

//创建一个数组包含着这三个NSDate对象
NSArray *dateList = @[now, tomorrow, yesterday];
```

**NSArray**的实例是无法改变的。一旦**NSArray**实例被创建后，就无法添加或删除数组里的指针，也无法改变数组的指针顺序。



### 存取数组

**NSArray**中的指针是有序排列的，并可以通过相应的**索引（index）**来存取。

索引以0为起始：索引0的位置保存第一个指针，索引1的位置保存第二个指针，以此类推。

```objective-c
//输出其中的两个对象
NSLog(@"The first date is %@",dateList[0]);
NSLog(@"The third date is %@",dateList[2]);

//包含多少个对象
NSLog(@"There are %lu dates",[dateList count]);
```



### 遍历数组

```objective-c
//遍历数组
NSUInteger dateCount = [dateList count];
for (int i = 0; i < dateCount; i++)
{
    NSDate *d = dateList[i];
    NSLog(@"Here is a date: %@",d);
}
```



### NSMutableArray

**NSMutableArray**实例和**NSArray**实例类似，但是可以添加、删除或对指针重新进行排序。

```objective-c
//创建空数组
NSMutableArray *dateList = [NSMutableArray array];

//将两个NSDate对象加入新创建的数组
[dateList addObject:now];
[dateList addObject:yesterday];

//将yesterday指针插入数组的起始位置
[dateList inserObject:yesterday atIndex:0];

//遍历数组
NSUInteger dateCount = [dateList count];
for (int i = 0; i < dateCount; i++)
{
    NSDate *d = dateList[i];
    NSLog(@"Here is a date: %@",d);
}

//删除yesterday指针
[dateList removeObjectAtIndex:0];
```

- **addObject:**方法在数组的尾部添加对象；
- **insertObject:atIndex:**方法将对象添加到一个特定的索引上。
- **removeObject:atIndex:**方法删除数组中的对象，数组对象的计数也会随之减少。



## 5.自定义类

在BNRPerson.h文件中，声命两个实例变量和五个实例方法。

```objective-c
#import <Foundation/Foundation.h>
@interface BNRPerson : NSObject
{
    //BNRPerson类有两个实例变量
    float _heightInMeters;
    int _weightInKilos;
}

//BNRPerson类有可以读取并设置实例变量的方法
- (float)heightInMeters;
- (void)setHeightInMeters:(float)h;
- (int)weightInKilos;
- (void)setWeightInKilos:(int)w;
- (float)bodyMassIndex;
@end
```

在BNRPerson.m文件中，完成对类方法的实现。

在BNRPerson.h文件中，声命两个实例变量和五个实例方法。

```objective-c
#import "BNRPerson.h"
- (float)heightInMeters
{
    return _heightInMeters;
}

- (void)setHeightInMeters:(float)h;
{
    _heightInMeters = h;
}

- (int)weightInKilos
{
    return _weightInKilos;
}

- (void)setWeightInKilos:(int)w
{
    _weightInKilos = w;
}

- (float)bodyMassIndex
{
    return _weightInKilos / (_heightInMeters * _heightInMeters);
}
```

### 存取方法

- 取方法：**heightInMeters**和**weightInKilos**是取方法，通过取方法其他方法可以读取相应的实例变量。
- 存方法：**setWeightInKilos:**和**setHeightInMeters:**就是存方法，通过存方法可以为相应的实例变量赋值。

```objective-c
//创建实例
BNRPerson *mikey = [[BNRPerson alloc] init];

//使用set为实例变量赋值
[mikey setWeightInKilos : 96];
[mikey setHeightInMeters : 1.8];

int weight = [mikey setWeightInKilos];
float height = [mikey setHeightInMeters];
```

### self

self是指针，指向运行当前方法的对象。

```objective-c
- (float)bodyMassIndex
{
    //return _weightInKilos / (_heightInMeters * _heightInMeters);
    float h = [self heightInMeters];
    return [self weightInkilos] / (h * h);
}
```

此外，self可作为实参传给其他方法，以便访问当前的对象。

```objective-c
- (void)addYourselfToArray: (NSMutableArray *)theArray
{
	[theArray addObject:self];
}
```

通过self将当前对象作为实参传给了**addObject:**方法。self就是**BNRPerson实例的地址**。



## 6.属性

### 声明属性

属性的声明以**@property**开始，然后是属性的**类型**和**名称**。

```objective-c
#import <Foundation/Foundation.h>
@interface BNRPerson : NSObject

//BNRPerson类有两个属性
@property (nonatomic) float heightInMeters;
@property (nonatomic) int weightInKilos;
    
//{
    //BNRPerson类有两个实例变量
    //float _heightInMeters;
    //int _weightInKilos;
//}

//BNRPerson类有可以读取并设置实例变量的方法
//- (float)heightInMeters;
//- (void)setHeightInMeters:(float)h;
//- (int)weightInKilos;
//- (void)setWeightInKilos:(int)w;
- (float)bodyMassIndex;

@end
```

声明属性的时候，编译器不仅会声明存取方法，还会根据属性的声明实现存取方法。













