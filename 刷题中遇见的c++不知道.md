## 值初始化与默认初始化

1、值初始化

   顾名思义，就是用数值初始化变量。如果没有给定一个初始值，就会根据变量或类对象的类型提供一个初始值。对于int类型其值初始化后的值为0。

2、默认初始化：如果定义变量时没有指定初值，则变量被默认初始化。其初始值和变量的类型以及变量定义的位置相关。默认初始化类对象和默认初始化内置类型变量有所不同。

对于默认初始化内置类型变量来说：

   1）定义在函数体之外的变量是全局变量，一般存储在全局区，存储在全局区的变量一般会执行值初始化。此时，其初始值和变量的类型有关。对于int类型其初始值为0，对于char类型其默认初始值为' '。例子如下：

```c++
#include <iostream>

using namespace std;

int i;
double d;
char c;

int main() 
{
   cout << "i = " << i << endl;
   cout << "d = " << d << endl;
   cout << "char" << c << "char" << endl;

   system("pause");
   return 0;
}
```

**在`VS2015`中编译结果如下：**

 ![1](E:\csdn_blog博客\c++学习\c++_xmind\image\1.png)

   2）定义在函数体内部的是局部变量，其存储在栈区中，如果没有指定初值，那么该局部变量将不会被初始化，也就是说这个局部变量的值是未定义的，是个**`随机值`**。**此时，如果不给这个局部变量赋值，那么就不能使用该局部变量，否则就会出错**，注意这种情况是没有被初始化，既没有使用默认初始化也没有使用值初始化，没有初始化的值是不能使用的。例子如下：

```c++
#include <iostream>

using namespace std;

int main() 
{
   int i;
   cout << "i = " << i << endl;

   system("pause");
   return 0;
}
```

**在`VS2015`中编译结果如下：**

![2](E:\csdn_blog博客\leetcode刷题\image\2.png)

对于默认初始化类对象来说：不仅在创建类对象时，要使用默认构造函数，而且类成员要没有类内初始值，类才会执行默认初始化。

   如果是自定义的默认构造函数，不进行任何操作或者是编译器合成的默认构造函数，其成员变量经过默认初始化之后的初始化值和局部变量的初始值是一样的，即是随机值，**但是可以使用**(类普通成员变量也存放在栈区)。例子如下：

```c++
#include <iostream>
#include <string>

using namespace std;

class Init 
{
public:
   int getA() 
   {
     return a;
   }
   double getD()
   {
     return d;
   }
   char getC()
   {
     return c;
   }
   string getStr()
   {
     return str;
   }

private:
   int a;
   char c;
   double d;
   string str;
};

int main() 
{
   Init init;

   cout << "a = " << init.getA() << endl;
   cout << "d = " << init.getD() << endl;
   cout << "ccc" << init.getC() << "ccc" << endl;
   cout << "str" << init.getStr() << "str" << endl;

   system("pause");
   return 0;
}
```

**在`VS2015`中编译结果如下：**

![3](image\3.png)

对于string类型的成员变量是个空串，这可能和string类的内部实现有关。

3、默认初始化的出现场景。

   1）当我们在块作用域内不使用任何初始值定义一个非静态变量或者数组时。数组和非静态变量它们的值都是随机值，但是数组可以使用，而非静态变量需要在赋值以后才能使用。例子如下;

```c++
#include <iostream>

using namespace std;

int main()
{
   int a[5];

   for (int i = 0; i < 5; i++)
   {
     cout << "数组元素值为：" << a[i] << endl;
   }
```

对于string类型的成员变量是个空串，这可能和string类的内部实现有关。

3、默认初始化的出现场景。

   1）当我们在块作用域内不使用任何初始值定义一个非静态变量或者数组时。数组和非静态变量它们的值都是随机值，但是**数组可以使用**，而**非静态变量需要在赋值以后才能使用**。例子如下;

```c++
#include <iostream>

using namespace std;

int main()
{
   int a[5];

   for (int i = 0; i < 5; i++)
   {
     cout << "数组元素值为：" << a[i] << endl;
   }

   system("pause");
   return 0;
}
```

**在`VS2015`中编译结果如下：**

![4](image\4.png)

  2）当一个类本身含有类类型的成员且使用合成的默认构造函数时。使用合成的默认构造函数那么该类和其类类型成员都会执行默认初始化。

  3）当类类型的成员没有在构造函数初始值列表中显示地初始化时。说明该类类型成员会调用默认构造函数。

4、值初始化的出现场景

  1）在数组初始化的过程中如果我们提供的初始值数量少于数组的大小时，剩余的为提供初始值部分就会执行值初始化。

```c++
#include <iostream>

using namespace std;

int main()
{
   int a[5] = {1, 2, 3};

   for (int i = 0; i < 5; i++)
   {
     cout << "数组元素值为：" << a[i] << endl;
   }

   system("pause");
   return 0;
}
```

**在`VS2015`中编译结果如下：**

![5](image\5.png)

由结果可知，没有提供初始值的数组元素按照值初始化为了0。

  2）当我们不使用初始值定义一个局部静态变量时。因为是静态的，所以该局部静态变量不再存储在栈区，而是存储在全局区。此时，就会根据类型进行设置该变量的初始值，即**值初始化**。例子如下：

```c++
#include <iostream>

using namespace std;

int main()
{
   static int a;
   static char c;

   cout << "a:" << a << endl;
   cout << "ccc" << c << "ccc" << endl;   

   system("pause");
   return 0;
}
```

**在`VS2015`中编译结果如下：**

  3）当我们通过书写形如T()的表达式显示请求值初始化时，其中T是类型名(vector的一个构造函数只接受一个实参用于说明vector的大小)，它就是使用一个这种形式的实参来对它的元素初始器进行值初始化）。

 

> 对于默认初始化，可以参考《C++ primer》`P40`页

> 对于值初始化，可以参考《C++ primer》`P88`页

> 对于默认初始化和值初始化的发生场景，可以参考《C++ primer》`P262`页

> 对于合成的默认构造函数，可以参考《C++ primer》`P235`页



## map容器的的小细节

- ```c++
  map<int, int> m;
  cout<<m[0]; // 是0   所以如果键不存在的话，可以用m[0]++,来创建一个键值对，且值为1
  ```

- ```c++
  map<int, int> m{{0, 1}};
  cout<<m[0];  // 是1
  ```

## 关于静态成员

### **静态成员变量**

- 内类声明，内外初始化

- 如果没有初始化则不可以访问，且编译失败

- ```c++
  class Person{
  public:
  	int static a;
  }
  int Person::a = 0;  // int不能丢
  ```

### 静态成员函数

- 不能访问非静态成员函数

- 因为类没有为静态成员函数分配this指针

- 如果想要其访问的话可以在静态成员函数中传入一个类的引用

  - ```c++
    class Foo
    {
        int m_f;
    public:
        static void f()
        {
            m_f=666;   //这是非法的，这个等价于this->m_f=666,而静态方法没有this
        }
        static void f(Foo&a)
        {
            a.m_f=666;   //这样就可以
        }
    };
    ```

## 字符串处理细节

- 数字`char`字符转`int`的转换

- 原理：`0~9`的`ascll`码为`[48, 57]`

  - ```c++
    char a = '8';
    int b = a - '0';  // b的值为8   56-48 == 8
    ```

## `memset`函数

按字节初始化：一个字节一个字节的初始化

下面给出源码：：可以在调试的时候调用内存窗口查看

```c++
void *(memset)(void * s, int c, size_t n)  // 查看内存 也是小端存储
{
	const unsigned char uc = c;
	unsigned char *su;
	for (su = (unsigned char*)s; 0 < n; ++su, --n)
		*su = uc;
	return s;
}
```

