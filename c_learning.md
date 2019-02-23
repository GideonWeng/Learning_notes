# C learning notes
Create date: 2019-2-21  
Author: Gideon
--------

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
