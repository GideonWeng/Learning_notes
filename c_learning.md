# C learning notes
**Create date:** 2019-2-21  
**Author:** Gideon
---
[C learning notes](#c-learning-notes)
- [1. what does `\0` stand for?](#1-what-does-\0-stand-for)  
- [2. If the return value type is omitted from the function definition, the default is the `int` type.](#2-if-the-return-value-type-is-omitted-from-the-function-definition-the-default-is-the-int-type)
- [3. Type of #define variables?](#3-type-of-define-variables)
- [4. keyword `extern`](#4-keyword-extern)
- [5. keyword `static`](#5-keyword-static)
- [6. Initialization](#6-Initialization)
---

## 1. what does `\0` stand for?  

**source:** https://stackoverflow.com/questions/14461695/what-does-0-stand-for/14461711  
**date:** 2019-2-21 

The null character `'\0'` (also `null terminator`), abbreviated `NUL`, is a control character with the value `zero`. Its the same in **C** and **objective C**.  
The character has much more significance in C and it serves as a reserved character used to signify the end of a string,often called a [null-terminated string](http://en.wikipedia.org/wiki/Null-terminated_string).  
The length of a [C string](http://en.wikipedia.org/wiki/C_string) (an array containing the characters and terminated with a `'\0'` character) is found by searching for the **(first) NUL byte**.  

## 2. If the return value type is omitted from the function definition, the default is the `int` type.  
若函数定义中忽略了返回值类型，则默认为`int`类型。  

## 3. Type of #define variables?   
If I have:`#define MAXLINE 5000`  
What type is MAXLINE understood to be? Should I assume it is an `int`? Can I test it somehow?  
In general, how can one determine the type of `#define`ed variable?  

**source:** https://stackoverflow.com/questions/8584383/type-of-define-variables  
**date:** 2019-2-23  

(Very!) Broadly, your C compiler is going to perform 3 tasks when executed:

    1. Run a preprocessing pass over your source files,

    2. Run a compiler over the preprocessed source files

    3. Run a linker over the resulting object files.  
Lines starting with a ``#``, like the line
```c
#define MAXLINE    5000
```
is handled by the preprocessor phase. (simplistically) The preprocessor will parse a file and perform text substitutions for any macros that it detects. There is no concept of types within the preprocessor.

Suppose that you have the following lines in your source file:

```c
#define MAXLINE    5000
int someVariable = MAXLINE;     // line 2
char someString[] = "MAXLINE";  // line 3
```
The preprocessor will detect the macro ``MAXLINE`` on line 2, and will perform a text substitution. Note that on line 3 ``"MAXLINE"`` is not treated as a macro as it is a string literal.

After the preprocessor phase has completed, the compilation phase will only see the following:
```c
int someVariable = 5000;        // line 2
char someString[] = "MAXLINE";  // line 3
```
(comments have been left in for clarity, but are normally removed by the preprocessor)
You can probably use an option on the compiler to be able to inspect the output of the preprocessor. In gcc the ``-E`` option will do this.

Note that while the preprocessor has no concept of type, there is no reason that you can't include a type in your macro for completeness. e.g.
```c
#define MAXLINE    ((int)5000)
```

## 4. keyword `extern`
In all source files of a source program, an external variable can only be defined **once** in a file, while other files can be accessed through an `extern` declaration (the source file defining the external variable can also contain `extern` statement of a external variable). The length of the array **must be** specified in the definition of the external variable, but the `extern` declaration does not have to specify the length of the array.  
在一个源程序的所有源文件中，一个外部变量只能在某个文件中*定义***一次**，而其他文件可以通过`extern`声明来访问它（定义外部变量的源文件中也可以包含对该外部变量的`extern`声明）。外部变量的定义中必须指定数组的长度，但`extern`声明则不一定要指定数组的长度。

## 5. keyword `static`
`static` can be used to declare external variables, internal variables, and functions.
The internal variable of the `static` type is the same as the automatic variable. It is a local variable of a particular function. It can only be used in this function, but it is different from the automatic variable. It always exists regardless of whether its function is called or not. Rather than being an automatic variable, it exists and disappears as the function is called and exited. **In other words, an internal variable of type `static` is a variable that can only be used in a particular function but always occupies storage space.**  
`static` 可用于声明外部变量、内部变量和函数。  
`static`类型的内部变量同自动变量一样，是某个特定函数的局部变量，只能在该函数中使用，但它与自动变量不同的是，不管其所在函数是否被调用，它一直存在，而不像自动变量那样，随着所在函数的被调用和退出而存在和消失。**换句话说， `static` 类型的内部变量是一种只能在某个特定函数中使用但一直占据着储存空间但变量。**

## 6. Initialization
- In the absence of explicit initialization, external and static variables are guaranteed to be initialized to zero; automatic and register variables have undefined (i.e., garbage) initial values.  
在不进行显示初始化的情况下，外部变量和静态变量都将被初始化为0，而自动变量和寄存器变量的初值没有定义（即初值为无用的信息）。  
Question: what is the type of `0`?  
-  There is no way to specify repetition of an initializer, nor to initialize an element in the middle of an array without supplying all the preceding values as well. 
-  Character arrays are a special case of initialization; a string may be used instead of the braces and commas notation:
    ```c
    char pattern = "ould";
    ```
    is a shorthand for the longer but equivalent
    ```c
    char pattern[] = { 'o', 'u', 'l', 'd', '\0' };
    ```
    In this case, the array size is five (four characters plus the terminating '\0').

    # 7. Macro Substitution
    Substitutions are made only for tokens, and do not take place within quoted strings. For example, if `YES` is a defined name, there would be no substitution in `printf("YES")` or in `YESMAN`.
