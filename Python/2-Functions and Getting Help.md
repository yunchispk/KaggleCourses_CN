# Intro

您已经看到并使用了诸如`print`和`abs`之类的函数。但是Python还有其他许多函数，还有，定义自己的函数是python编程的重要组成部分。

在本课程中，您将学习有关使用和定义函数的更多信息。

# Getting Help

您在上一教程中看到了`abs`函数，但是如果忘记了它会做什么呢？

`help()`函数可能是您可以学习的最重要的Python函数。 如果您还记得如何使用`help()`，那么您就掌握了理解大多数其他函数的关键。

以下是示例:

In [1]:

```
help(round)
Help on built-in function round in module builtins:

round(number, ndigits=None)
    Round a number to a given precision in decimal digits.
    
    The return value is an integer if ndigits is omitted or None.  Otherwise
    the return value has the same type as the number.  ndigits may be negative.
```

`help()`显示下面两条信息:

1. 函数`round(number[, ndigits])`告诉我们`round()`需要传入一个数字`number`作为参数. 另外我们可以选择给出另外一个单独的参数`ndigits`
2. 有关该功能作用的简短英文说明。

**Common pitfall(常见的陷阱):** 在查找函数时，请记住传递函数的名字，而不要传递调用该函数的结果。

如果我们在在`help()`函数上调用`abs()`函数会发生什么？ 取消隐藏下面单元格的输出以查看。



Output

In [2]:

```
help(round(-2.01))
```

Python的执行是由内到外的。首先，它计算`round（-2.01）`的值, 然后再把计算结果传给`help()`函数

(事实证明，关于整数有很多话要说！稍后我们讨论Python中的对象，方法和属性之后，上面大量的帮助输出将更有意义。)

`round` 是一个非常简单的函数，它带有短的文档. 当你使用例如`print`等更复杂的函数时`help`函数会更牛逼. 下面的输出可能有点难懂，但问题不大 ，现在只需要能从`help`函数中了解到新知识就行.

In [3]:

```
help(print)
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
```

If you were looking for it, you might learn that print can take an argument called `sep`, and that this describes what we put between all the other arguments when we print them.

如果您正在寻找它，您可能会学到`print`可以接受一个名为`sep`的参数，当打印多个字符串时，它可以指定在各个字符串中间插入你指定的字符串。例如

```
print('a','b',sep="!!!") 
输出  a!!!b
```



## Defining functions

内置函数很棒，但是在我们需要开始定义自己的函数之前，我们只能了解它们。 下面是一个简单的示例。阿巴阿巴一堆没营养的话……

In [4]:

```
def least_difference(a, b, c):
    diff1 = abs(a - b)
    diff2 = abs(b - c)
    diff3 = abs(a - c)
    return min(diff1, diff2, diff3)
```

这个函数叫`least_difference`,它需要三个参数`a`,`b`,`c`

函数以`def`关键字作为开头.当函数被调用是，将执行`:`后的缩进代码块

`return`是另一个关键字.当Python遇到`return`语句时，它立即退出函数，并将`return`右边的内容传递个contex

`least_difference`函数做了什么事情很显然吧？如果不确定，我们可以用下面的例子尝试一下

In [5]:

```
print(
    least_difference(1, 10, 100),
    least_difference(1, 10, 10),
    least_difference(5, 6, 7), # Python allows trailing commas in argument lists. How nice is that?
)
9 0 1
```

Or maybe the `help()` function can tell us something about it. 或许`help()`函数可以告诉我们它做了什么

In [6]:

```
help(least_difference)
Help on function least_difference in module __main__:

least_difference(a, b, c)
```

Python并没有聪明到读懂我们的代码(指这个函数的功能是什么)。然而，当我写这个函数时，我可以自己提供注释作为这个函数的**docstring**(文档)

### Docstrings

In [7]:

```
def least_difference(a, b, c):
    """Return the smallest difference between any two numbers
    among a, b and c.
    
    >>> least_difference(1, 5, -5)
    4
    """
    diff1 = abs(a - b)
    diff2 = abs(b - c)
    diff3 = abs(a - c)
    return min(diff1, diff2, diff3)
```

docstring是一个三引号字符串(可以跨越多行)，紧接在函数头之后，当对`least_difference`调用`help()`函数时，会显示函数名下的字符串文档

In [8]:

```
help(least_difference)
Help on function least_difference in module __main__:

least_difference(a, b, c)
    Return the smallest difference between any two numbers
    among a, b and c.
    
    >>> least_difference(1, 5, -5)
    4
```

> **Aside: example calls** The last two lines of the docstring are an example function call and result. (The `>>>` is a reference to the command prompt used in Python interactive shells.) Python doesn't run the example call - it's just there for the benefit of the reader. The convention of including 1 or more example calls in a function's docstring is far from universally observed, but it can be very effective at helping someone understand your function. For a real-world example of, see [this docstring for the numpy function `np.eye`](https://github.com/numpy/numpy/blob/v1.14.2/numpy/lib/twodim_base.py#L140-L194).

好的程序员会使用文档字符串(docstring)，除非他们期望在使用后立即将其丢弃（这是很少见的）。 因此，您也应该开始编写文档字符串。

## Functions that don't return

如果没有写`return`语句会怎么样

In [9]:

```
def least_difference(a, b, c):
    """Return the smallest difference between any two numbers
    among a, b and c.
    """
    diff1 = abs(a - b)
    diff2 = abs(b - c)
    diff3 = abs(a - c)
    min(diff1, diff2, diff3)
    
print(
    least_difference(1, 10, 100),
    least_difference(1, 10, 10),
    least_difference(5, 6, 7),
)
None None None
```

Python允许我们定义这样的函数，输出结果是个特殊值`None`(这个和其他语言的'null'类似)

如果没有`return`语句，`least_difference`是完全没有意义的，但是没有`return`语句，带有副作用(side effects)的函数或许是有用的。我们已经见了两个例子：`print`和`help`没有返回任何东西.我们调用他们，只是为了他们的(side effects)(在屏幕上输出信息).还有一些其他的有用的副作用(side effects)函数，可以用来写入文件或者修改输入

In [10]:

```
mystery = print()
print(mystery)
None
```

## Default arguments

当调用`help(print)`时，我们看到`print`函数有许多可选参数。例如我们可以指定`sep`的值，来给我们需要打印的字符串插入特定的字符串。

In [11]:

```
print(1, 2, 3, sep=' < ')
1 < 2 < 3
```

如果没有指定值，`sep`的值会默认为`' '`(一个空格)

In [12]:

```
print(1, 2, 3)
1 2 3
```

将具有默认值的可选参数添加到我们定义的函数中非常容易：

In [13]:

```
def greet(who="Colin"):
    print("Hello,", who)
    
greet()
greet(who="Kaggle")
# (In this case, we don't need to specify the name of the argument, because it's unambiguous.)
greet("world")
Hello, Colin
Hello, Kaggle
Hello, world
```

## Functions Applied to Functions

下面的内容很牛逼，尽管他看起来很抽象。你可以把一个函数作为参数传给另一个函数，看下面的事例可能会好懂一点

In [14]:

```
def mult_by_five(x):
    return 5 * x

def call(fn, arg):
    """Call fn on arg"""
    return fn(arg)

def squared_call(fn, arg):
    """Call fn on the result of calling fn on arg"""
    return fn(fn(arg))

print(
    call(mult_by_five, 1),
    squared_call(mult_by_five, 1), 
    sep='\n', # '\n' is the newline character - it starts a new line
)
5
25
```

在其他函数上起作用的函数称为“高阶函数”。 您可能有一段时间不会写自己的了。 但是Python内置了一些高阶函数，您可能会发现它们对调用很有用。

这是一个使用`max`函数的有趣示例。

默认情况下，“ max”返回其最大的参数。 但是，如果我们使用可选的`key`参数传递给函数，则它将返回使`key（x）`（也称为'argmax'）最大化的参数`x`。





In [15]:

```
def mod_5(x):
    """Return the remainder of x after dividing by 5"""
    return x % 5

print(
    'Which number is biggest?',
    max(100, 51, 14),
    'Which number is the biggest modulo 5?',
    max(100, 51, 14, key=mod_5),
    sep='\n',
)
Which number is biggest?
100
Which number is the biggest modulo 5?
14
```

# Your Turn

函数给你打开了一个全新的Python编程世界**[Try using them yourself](https://www.kaggle.com/kernels/fork/1275158)**