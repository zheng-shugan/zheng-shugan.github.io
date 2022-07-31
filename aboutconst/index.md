# const 的困惑


## const 是为了解决什么问题？

> 有时候我们希望定义这样一种变量，它的值不能被改变。例如，用一个变量来表示缓冲区的大小。使用变量的好处是当我们觉得缓冲区的大小不再合适的时，很容易对其进行调整。另一方面，也随时警惕防止程序一不小心改变了这个值。为了满足这一要求，可以使用关键字 **const** 对变量的类加以限定：
>
> ```c++
> const int BUFSIZE = 512;
> ```

以上便是 `const` 在《C++ Primer》中的解释。

第一次看书的时候觉得 const 不就是定义一个常量用的东西，没啥好关注的。直到写出 `const int x = 10;` 和 `int const x = 10` 一时竟不知道其区别是什么，于是决定再翻翻《C++ Primer》看一下 const 的使用方法，也解决一下心中的困惑。（还顺便发现 LoveIt 主题一个 bug：代码块中不能识别 C/C++ 代码）

## const 的引用

可以把引用绑定到 const 对象上，一般称之为**对常量的引用（reference to const)**，和普通引用唯一不同的是，对常量的引用不能改变它所绑定的对象：

```c++
const int obj = 1024;
const int &r1 = obj;	// 正确：引用及其对象都是常量
r1 = 512;							// 错误：常量引用不可改变其绑定的对象
int &r2 = obj;				// 错误：试图让一个非常量引用指向一个常量对象

int i = 1024;
const int &r3 = i;		// 正确：对 const 的引用可以引用一个非 const 的对象
r3 = 0;								// 错误：r3 是一个常量引用
```

## const 和指针

与引用一样，也可以领指针指向常量对象，类似于对常量的引用，**指向常量的指针（pointer to const）**不能用于改变其所指向对象的值：

```c++
const double pi = 3.14;
double *p2Pi = &pi;					// 错误：p2Pi 是一个普通指针
const double *cp2Pi = &pi;	// 正确：cp2Pi 可以指向一个 double 常量
*cp2Pi = 3.1415;						// 错误：指向常量的指针不能用于改变其所指向对象的值
```

### const 指针

> 以上都是些开胃小菜，下面的内容会对 const 有了一个新的认识。

指针是一个对象，所以**常量指针（const pointer）**必须初始化，而且一旦初始化指针指向的地址就不可以改变了。把 `*` 放在 const 关键字之前就说明该指针是一个常量指针。

```c++
double pi = 3.14;
const double *const p2Pi = &pi;     // p2Pi 是一个指向常量对象的常量指针

int num = 1024;
int *const currNum = &num;          // currNum 将一直指向 num
```

这样的书写方式也隐含着：不变的指针本地的值而非指向的那个值。

想要弄清楚这些含义最好的方式就是从从右向左看。此例中：离 `currNum` 最近的符号是 const，意味着这是一个常量对象，下一个符号是 `*`，意思是 `currNum` 是一个常量的指针，最后的类型部分确定了常量指针指向对象的类型。类似的 `p2Pi` 就可以解读为一个指向常量的指针常量。

指针本身是一个常量并不意味着不能通过指针修改对象的值，能否这样做取决于指向对象的类型。

### const 的位置

指针本身使用过对象，它可以指向另外一个对象。所以，指针本身是不是常量以及指针所指向的是不是一个常量就是两个独立的问题。用名词**顶层 const（top-level const）**表示指针本身是一个常量，而用**底层 const（low-level const）**表示指针指向的对象是一个常量。

```c++
double pi = 3.14;
const double d = 2.0;           // 不能修改 d 的值，这是一个顶级 const
const double *p1 = &pi;
double *const p2 = &pi;
const double *p3 = &pi;
*p1 = 3.1415;                   // 错误：p1 是指向常量的指针（底层 const）
p2 = &d;                        // 错误：p2 是指针常量，不能修改指向的对象（顶层 const）
*p3 = 10;                       // 错误：不允许改变 *p3，p3 是一个指向常量的指针
const double *const p4 = p2;    // 靠右的 const 是顶层 const，靠左的是 底层 const
const double &r = pi;           // 用于声明应用的 const 都是底层 const
```

如果是在变量中使用了顶/低层 const：

```c++
const int x = 10;
int const x = 10;
```

按照前面的定义可以说这两个声明的意思是一样的，定义了一个变量名为 x，值等于 10 的一个常量。两种方式都可以声明一个常量，我还是习惯于些 `const int x = 10`。

## 函数中的 const

当形参是 const 时，如前面所述，顶层 const 作用于对象本身：

```c++
const double pi = 3.14;
double num = pi;					// 正确：拷贝 pi 时忽略其顶层 const
double *const p = &num;		// 错误：const 是顶层的，不能给 p 复制
*p = 0;										// 正确：通过 p 改变 num 是合法的: => num = 0
```

和其他初始化过程一样，当实参初始化形参时会忽略其顶层 const。换句话说，当形参顶层有 const 时，传给它的值可以是常量也可以是非常量。

```c++
void foo(const int &num);
void foo(int &num);					// 错误：因为前一个函数顶层参数被忽略了，所以这两个函数的参数列表是一样的
```

### class 中的 const 函数

假设我们现在有这样一个 class：

```c++
#include <string>
#include <vector>

using std::string;
using std::vector;

class Stack {
public:
    int size() {
        return stk.size();
    }

private:
    std::vector<string> stk;
};
```

还有这样一个函数：

```c++
int getStackSize(const Stack *stk) {
    return stk->size();
}
```

因为 stk 是一个 const reference 参数，因此在调用 `getStackSize()` 的时候应保证其不会修改 stk 的值，但是在 stk 上调用的每一个方法都有可能会影响 stk 的值，我们必须在成员函数上标注 const 来告诉编译器：Hi，编译器，在 Stack 对象调用 size() 方法的时候不会修改其对象的值:

```c++
#include <string>
#include <vector>

using std::string;
using std::vector;

class Stack {
public:
    int size() const {			// const 修饰符紧接在函数参数列表之后
        return stk.size();
    }

private:
    std::vector<string> stk;
};
```


