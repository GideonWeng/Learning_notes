# C++函数的返回值——返回引用类型&非引用类型
函数的返回主要分为以下几种情况：
----
Date: 2019-2-26  
Source: https://www.cnblogs.com/fly1988happy/archive/2011/12/14/2286908.html
## 1、主函数main的返回值：
允许主函数main没有返回值就可结束；可将主函数main返回的值视为状态指示器，返回0表示程序运行成功，其他大部分返回值则表示失败。
## 2、返回非引用类型：
+ 函数的返回值用于初始化在调用函数时创建的*临时对象(temporary object)*，如果返回类型不是引用，在调用函数的地方会将函数返回值*复制*给临时对象。
+ 在求解表达式的时候，如果需要一个地方存储其运算结果，编译器会创建一个没命名的对象，这就是临时对象。C++程序员通常用temporary这个术语来代替temporary object。
+ 用函数返回值初始化临时对象与用实参初始化形参的方法是一样的。
+ 当函数返回非引用类型时，其返回值既可以是局部对象，也可以是求解表达式的结果。

## 3、返回引用类型：
+ 当函数返回引用类型时，没有***复制返回值***，相反，返回的是 ***对象本身***。
+ ***千万不要返回局部对象的引用！千万不要返回指向局部对象的指针！***
当函数执行完毕时，将释放分配给局部对象的存储空间。此时对局部对象的引用就会指向不确定的内存！返回指向局部对象的指针也是一样的，当函数结束时，局部对象被释放，返回的指针就变成了不再存在的对象的悬垂指针。
+ 返回引用时，要求***在函数的参数中，包含有以引用方式或指针方式存在的，需要被返回的参数。***
正确的方式
```c++
int& abc(int a, int b, int c, int& result)

{
     result = a + b + c;
     return result;
}
```
错误的方式
```c++
int& abc(int a, int b, int c)

{
 return a + b + c;
}
```
## 4、返回const类型
由于返回值直接指向了一个生命期尚未结束的变量，因此，对于函数返回值（或者称为函数结果）本身的任何操作，都在实际上，是对那个变量的操作，这就是引入const类型的返回的意义。当使用了const关键字后，即意味着函数的返回值不能立即得到修改！如下代码，将无法编译通过，这就是因为返回值立即进行了++操作（相当于对变量z进行了++操作），而这对于该函数而言，是不允许的。如果去掉const，再行编译，则可以获得通过，并且打印形成z = 7的结果。 
```c++
include <iostream>
include <cstdlib>
const int& abc(int a, int b, int c, int& result)
{
    result = a + b + c;
    return result;
}
int main()
{
   int a = 1; int b = 2; int c=3;
   int z;
   abc(a, b, c, z)++;  //wrong: returning a const reference
   cout << "z= " << z << endl;
   return 0;
}
```
## 5、例子
下面是一个段有错误的代码，找出其中的错误。
错误代码
```c++
#include <iostream>
using namespace std;
int val() 
{ 

     int i = 1; 
     return i; 
} 
int & ref() 
{ 
     int &i = j; 
     return i;
} 
  
int main() 
{ 

     int   vv = val(); 
     int & rv = val(); 
     int   vr = ref(); 
     int & rr = ref(); 
     return 0;
}
```
正确代码
```c++
#include <iostream>
using namespace std;
int j=3;//j是全局变量
int val() 
{ 

     int i = 1; 
     return i; 
} 
int & ref() 
{ 
//   int j=3;j不能是局部变量！
    int &i = j; 
      return i; //不能返回局部对象的引用
} 
  
int main() 
{ 

      int   vv = val(); 
      int   rv = val();//int   &rv = val()错误！val()返回的是一个int型的数，而给引用&rv 赋值的必须是一个同类型的变量。
    int   vr = ref(); 
      int & rr = ref(); 
      cout<<vv<<endl;
      cout<<rv<<endl;
      cout<<vr<<endl;
      cout<<rr<<endl;
      return 0;
}
```