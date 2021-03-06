---

layout: post
title: c++ 编程要点
category: cPlus
description: c++ 编程需要注意的地方

---



##  关于类

### 实例化

1 Human Tom; 

2 Human * tom = new Human();

实例化方式不同，访问成员的方式分别是.和->

### 类访问权限控制

访问修饰符

|  修饰符   | 类内部 | 类对象 | 子类（派生类） | 友元 |
| :-------: | :----: | :----: | :------------: | :--: |
|  private  |   可   |  不可  |      不可      |  可  |
|  public   |   可   |   可   |       可       |  可  |
| protected |   可   |  不可  |       可       |  可  |

总之：private成员只能被类内和友元访问，不能被派生访问；protected可以被派生访问。

继承权限控制

| 继承方式  | 父类中的private | protected |  public   |
| :-------: | :-------------: | :-------: | :-------: |
|  private  |     private     |  private  |  private  |
| protected |     private     | protected | protected |
|  public   |     private     | protected |  public   |

总之：私有继承最厉害，可以访问将父类中的（private权限及以下的 ）都变成private权限；protected可以把(protected及以下的)都变成protected，pubilc继承什么都不变

### 构造函数/析构函数

默认构造函数，构造函数可以重载

析构函数不可以重载，每个类只能有一个，如果没有手动实现，编译器将会创建一个伪析构函数，是空的。

**复制构造函数**

复制构造函数是一个特殊的重载构造函数，必须提供。很多时候，都会调用，每当对象被复制时（包括将对象按值传递给函数时）。

复制构造函数的定义：必须是const修饰的 引用类型的传递参数。const是保证不对源目标破坏，引用类型是因为：如果按值传递时，会调用复制构造函数本身，那么本身调用本身，无限循环下去，不可能。

**构造函数和析构函数的其他作用**

例子1： 单例模式  关键点是：构造函数（包括拷贝构造函数，赋值运算符）是私有的，实例化函数是静态的。

例子2：禁止在栈中实例化的类  关键点是：析构函数是私有的，创建一个烧毁实例的函数（是静态的）。

### 重载

函数重载和运算符重载

**函数重载**： 同名函数的形式参数（指参数的个数、类型或者顺序）必须不同

**运算符重载**：可以是类成员函数，或者是非成员函数

一个类成员函数加法运算符重载的例子：

```c++
      // 重载 + 运算符，用于把两个 Box 对象相加
      Box operator+(const Box& b)
      {
         Box box;
         box.length = this->length + b.length;
         box.breadth = this->breadth + b.breadth;
         box.height = this->height + b.height;
         return box;
      }
```

一个重要的运算符重载：赋值运算符函数

还记得拷贝构造函数吗，我们要用=实现相同的功能

实现例子：

```c++

```



### 类中的this指针

this是当前对象的地址，对一般的类成员方法中传参，会隐式的传递this，但是对静态成员函数不会传递。因为静态函数不与类实例相关联，而是所有实例共享。

### 类占据的内存

sizeof() 可用于类，也可以用于类的对象。

用于类：是指出类声明中所有数据属性占用的总内存量，不考虑成员函数和局部变量

用于对象时：结果相同。

### 友元

一个类内部可以定义它的友元，可以是友元函数，也可以是友元类。

友元可以访问该类的私有成员。

### 继承

c++ 面向对象编程基于 ：封装，继承，多态

继承在默认情况下是，private继承。

private继承的关系一般是 has a ，比如 发动机 派生 汽车  也就是：汽车有发动机

public继承的关机一般是 is a， 比如  鱼 派生 金鱼  金鱼是一种鱼

### 多态

面向对象的核心-多态

多态行为： 将派生类对象视为基类对象，并执行派生类的成员函数。

**虚函数**

**虚函数** 是在基类中使用关键字 **virtual** 声明的函数；在派生类重新定义基类中的虚函数时，会告诉编译器不要静态链接到该函数，而是根据程序中任意点所调用的对象类型来选择调用的函数，这种成为动态链接，或后期绑定。

**纯虚函数**

```c++
virtual int area() = 0;
```

=0 告诉编译器该虚函数是纯虚函数

**虚析构函数**

为什么需要虚析构函数？

与普通函数一样，虚函数是为了令派生类对象在程序中能够调用自己的函数实现，析构函数也是如此，如果没有虚析构函数，那么可能就只调用了基类的析构函数。

**虚函数的工作原理**

todo ：理解虚函数表

**抽象类**

如果类中有一个纯虚函数，就是抽象类，抽象类不能被实例化

**可以将拷贝构造函数声明位虚函数吗**

不可以，原因：

todo

**virtual虚继承**

todo

## 类型修饰符

### **const**

const可以修饰：内置类型变量，自定义对象，成员函数，返回值，函数参数。

作用：指定一个语义约束，编译器会强制实施这个约束，告诉编译器某值保持不变。

#### 1 修饰普通类型的变量

```c++
const int  a = 7; 
int  b = a; // 正确
a = 8; 
```

因为常量不能修改，所以在定义的时候必须初始化！

#### 2 修饰指针

const 修饰指针变量有以下三种情况。

- A: const 修饰指针指向的内容，则内容为不可变量。

- B: const 修饰指针，则指针为不可变量。

- C: const 修饰指针和指针指向的内容，则指针和指针指向的内容都为不可变量。

**对于A**

```c++
const int *p = 8;  //8 不可以改变，这句话就是错的，不能赋值
int a = 1;         
const int *p = &a; // p指向的内容不可以改变
a = 2;   // 正确，虽然按道理来说p指向的内容不可以改变，但是a可以改变，也就改变了p指向的内容
*p = 2;  // 错误，p指向的内容不可以改变
```

**对于B**

```c++
int a = 8;
int* const p = &a;
*p = 9; // 正确
int  b = 7;
p = &b; // 错误
```

**对于C**

```c++
int a = 1;
const int * const  p = &a;
*p = 9; // 错误，因为指向的内容不可以改变
int b = 2;
p = &b; // 错误，因为p指针本身的内容不可以改变
```

#### 3 修饰参数传递

修饰参数可以分为三种情况

A：值传递的 const 修饰传递，一般这种情况不需要 const 修饰，因为函数会自动产生临时变量复制实参值。

```c++
void Cpf(const int a)
{
    cout<<a;
    // ++a;  是错误的，a 不能被改变
}
 
int main(void)
{
    Cpf(8);
    system("pause");
    return 0;
}
```

B：当 const 参数为指针时，这种情况与上面修饰指针的情况一样，分三种情况

```c++
void Cpf(int *const a)
{
    cout<<*a<<" ";
    *a = 9; // 正确，因为const这时限定了本身不可以改变
    a++; // 错误，因为a指针本身不可以改变，这时候可以防止指针被意外纂改。
}
 
int main(void)
{
    int a = 8;
    Cpf(&a);
    cout<<a; // a 为 9
    system("pause");
    return 0;
}
```

C：自定义类型的参数传递，需要临时对象复制参数，对于临时对象的构造，需要调用构造函数，比较浪费时间，因此我们采取 const 外加引用传递的方法。

```c++
#include<iostream>
using namespace std;
 
class Test
{
public:
    Test(){}
    Test(int _m):_cm(_m){}
    int get_cm()const
    {
       return _cm;
    }
private:
    int _cm;
};

void Cmf(const Test& _tt)
{
    cout<<_tt.get_cm();
}
 
int main(void)
{
    Test t(8);
    Cmf(t);
    system("pause");
    return 0;
}
```

#### 4 修饰返回值

A：const 修饰内置类型的返回值，修饰与不修饰返回值作用一样。

```cpp
#include<iostream>
using namespace std;
 
const int Cmf()
{
    return 1;
}
 
int Cpf()
{
    return 0;
}
 
int main(void)
{
    int _m = Cmf();
    int _n = Cpf();
 
    cout<<_m<<" "<<_n;
    system("pause");
    return 0;
}
```

B: const 修饰返回的指针或者引用，是否返回一个指向 const 的指针，取决于我们想让用户干什么。

C: const 修饰自定义类型的作为返回值，此时返回的值不能作为左值使用，既不能被赋值，也不能被修改。

#### 5 修饰类成员函数

1. 类的成员函数后面加 const，表明这个函数不会对这个类对象的数据成员（准确地说是非静态数据成员）作任何改变。有 const 修饰的成员函数（指 const 放在函数参数表的后面，而不是在函数前面或者参数表内），只能读取数据成员，不能改变数据成员；没有 const 修饰的成员函数，对数据成员则是可读可写的。

2. 常量（即 const）对象可以调用 const 成员函数，而不能调用非const修饰的函数.

3. 两个成员函数如果只是常量性不同，是可以被重载的

   ```c++
   class A
   {
       public:
       void f()
       {
           cout<<"non const"<<endl;
       } 
       void f() const
       {
           cout<<" const"<<endl;
       } 
   };
   ```

4. const 关键字不能与 static 关键字同时使用，因为 static 关键字修饰静态成员函数，静态成员函数不含有 this 指针，即不能实例化，const 成员函数必须具体到某一实例。

## 存储类修饰符

存储类定义 C++ 程序中变量/函数的范围（可见性）和生命周期。这些说明符放置在它们所修饰的类型之前。下面列出 C++ 程序中可用的存储类：

- auto
- register
- static
- extern
- mutable
- thread_local (C++11)

### 1 auto

自 C++ 11 以来，**auto**关键字用于两种情况：声明变量时根据初始化表达式自动推断该变量的类型、声明函数时函数返回值的占位符。

```cpp
auto x=1; // 自动推断x是int，看起来用处不大
std::vector<std::string>::iterator i=vs.begin();
auto i=vs.begin();  // 等价于上一行，就有用了
```

### 2 register

为提高执行效率，C++语言允许将局部变量的值放在运算器中的寄存器里，需要时直接从寄存器中取出参加运算，不必再到内存中去存取，这种变量叫做寄存器变量，

1 这意味着变量的最大尺寸等于寄存器的大小（通常是一个词），且不能对它应用一元的 '&' 运算符（因为它没有内存位置）。

2 寄存器只用于需要快速访问的变量，比如计数器。还应注意的是，定义 'register' **并不**意味着变量将被存储在寄存器中，它意味着变量**可能**存储在寄存器中，这取决于硬件和实现的限制。

```cpp
register int  count;
```

### 3 static

#### 1 修饰局部变量

 static 指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。因此，使用 static 修饰局部变量可以在函数调用之间保持局部变量的值。

#### 2 修饰全局变量

 static 也可以应用于全局变量。当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内。

#### 3 修饰静态成员数据

在 C++ 中，当 static 用在类数据成员上时，会导致仅有一个该成员的副本被类的所有对象共享。

#### 4 修饰静态成员函数

1. 静态函数只要使用类名加范围解析运算符 **::** 就可以访问，不需要实例存在 
2. 静态成员函数只能访问静态成员数据、其他静态成员函数和类外部的其他函数，因为他没有this，肯定不能访问该类的其他普通成员数据和普通成员函数！

### 4 extern

extern 修饰符通常用于当有两个或多个文件共享相同的全局变量或函数的时候，如下所示：

第一个文件：main.cpp

```cpp
#include <iostream>
 
int count ;
extern void write_extern();
 
int main()
{
   count = 5;
   write_extern();
}
```

第二个文件： support.cpp

```cpp
#include <iostream>
 
extern int count;
 
void write_extern(void)
{
   std::cout << "Count is " << count << std::endl;
}
```

### 5 mutable

mutable说明符仅适用于类的对象。它允许对象的成员替代常量。也就是说，mutable 成员可以通过 const 成员函数修改

### 6 thread_local

