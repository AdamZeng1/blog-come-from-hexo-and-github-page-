---
title: python数据结构的性能
date: 2017-10-27 17:11:41
tags: [Git]
categories: GIT
author: AdamZeng
copyright: true
---
# python数据结构的性能

### 1.Python列表与字典操作的大O性能

然后我们将做一些基于时间的实验来说明每个数据结构的花销和使用这些数据结构的好处。重要的是了解这些数据结构的效率，因为它们是本书实现其他数据结构所用到的基础模块。<!--more-->

### 2.列表

>“python 的设计者在实现列表数据结构的时候有很多选择。每一个这种选择都可能影响列表操作的性能。为了帮助他们做出正确的选择，他们查看了最常使用列表数据结构的方式，并且优化了实现，以便使得最常见的操作非常快。当然，他们还试图使较不常见的操作快速，但是当需要做出折衷时，较不常见的操作的性能通常牺牲以支持更常见的操作。”

> 两个常见的操作是索引和分配到索引位置。无论列表有多大，这两个操作都需要相同的时间。当这样的操作和列表的大小无关时，它们是 O（1）。

>“另一个非常常见的编程任务是增加一个列表。有两种方法可以创建更长的列表，可以使用 append 方法或拼接运算符。append 方法是 O（1)。 然而，拼接运算符是 O（k），其中 k 是要拼接的列表的大小。这对你来说很重要，因为它可以帮助你通过选择合适的工具来提高你自己的程序的效率。”

让我们看看四种不同的方式，我们可以生成一个从0开始的n个数字的列表。首先我们将尝试一个for循环并通过创建列表，然后我们将使用append而不是拼接。接下来，我们使用列表生成器创建列表，最后，也是最明显的方式，通过调用列表构造函数包装range函数。

```python
import timeit
def test1():
    l=[]
    for i in range(1000):
        l=l+[i]

def test2():
    l=[]
    for i in range(1000):
        l.append(i)

def test3():
    l=[i for i in range (1000)]

def test4():
    l=list(range(1000))

t1 = timeit.Timer("test1()", "from __main__ import test1")
print("concat ",t1.timeit(number=1000), "milliseconds")
t2 = timeit.Timer("test2()","from __main__ import test2")
print("append",t2.timeit(number=1000),"milliseconds")
t3 = timeit.Timer("test3()", "from __main__ import test3")
print("comprehension ",t3.timeit(number=1000), "milliseconds")
t4=timeit.Timer("test4()","from __main__ import test4")
print("list range",t4.timeit(number=1000),"milliseconds")


concat  1.336322332994314 milliseconds
append 0.10088041100243572 milliseconds
comprehension  0.048220083001069725 milliseconds
list range 0.017883339009131305 milliseconds

```

从上面的试验清楚的看出，append操作比拼接快得多。其他两种方法，列表生成器的速度是append的两倍。

>“最后一点，你上面看到的时间都是包括实际调用函数的一些开销，但我们可以假设函数调用开销在四种情况下是相同的，所以我们仍然得到的是有意义的比较。因此，拼接字符串操作需要 6.54 毫秒并不准确，而是拼接字符串这个函数需要 6.54 毫秒。你可以测试调用空函数所需要的时间，并从上面的数字中减去它。”

现在我们已经看到了如何具体测试性能，见Table2，你可能想知道pop两个不同的时间。当列表末尾调用pop时，它需要O(1),但是当在列表中第一个元素或者中建任何地方调用pop时，它是O(n)。原因在于Python实现列表的方式，当一个项从列表前面取出，列表中的其他元素靠近起始位置移动一个位置。你会看到索引操作为O(1)。Python的实现者会权衡选择一个好的方案。

![image](https://facert.gitbooks.io/python-data-structure-cn/2.%E7%AE%97%E6%B3%95%E5%88%86%E6%9E%90/2.6.%E5%88%97%E8%A1%A8/assets/2.6.%E5%88%97%E8%A1%A8%20Table2.png)

作为一种演示性能差异的方法，我们用timeit来做一个实验。我们的目标是验证从列表从末尾pop元素和从开始pop元素的性能。同样，我们也想测量不用列表大小对这个时间的影响。我们期望看到的是，从列表末尾处弹出所需时间将保持不变，即使列表不断增长。而从列表开始处弹出元素时间将随列表增长而增加。

Listing 4展示了两种pop方式的比较。从第一个示例看出，从末尾弹出需要0.0003毫秒。从开始弹出要花费4.82毫秒。对于一个200万的元素列表，相差16000倍。

> Listing 4 需要注意的几点，第一， `from __main__ import x` , 虽然我们没有定义一个函数，我们确实希望能够在我们的测试中使用列表对象 x, 这种方法允许我们只计算单个弹出语句，获得该操作最精确的测量时间。因为 timer 重复了 1000 次，该列表每次循环大小都减 1。但是由于初始列表大小为 200万，我们只减少总体大小的 0.05%。

```python
import timeit
popzero=timeit.Timer("x.pop(0)","from __main__ import x")
popend=timeit.Timer("x.pop(0)","from __main__ import x")

x=list(range(2000000))
print(popzero.timeit(number=1000))
或者
print(popend.timeit(number=1000))
```

_listing 4_

虽然我们第一个测试显示pop(0)比pop()慢，但它没有证明pop(0)是O(n),pop()是O(1)，要验证它，我们需要看下一系列列表大小的调用效果。

```python
import timeit
popzero=timeit.Timer("x.pop(0)","from __main__ import x")
popend=timeit.Timer("x.pop()","from __main__ import x")

print("pop(0)","pop()")
for i in range(1000000,10000001,1000000):
    x=list(range(i))
    pt=popend.timeit(number=1000)
    x=list(range(i))
    pz=popzero.timeit(number=1000)
    print("%15.5f,%15.5f" %(pz,pt))
    
    
    pop(0) pop()
        0.51379,        0.00024
        1.29079,        0.00012
        1.58118,        0.00020
        2.21961,        0.00013
        2.95097,        0.00012
        3.48872,        0.00012
        3.98996,        0.00012
        4.80045,        0.00012
        5.23652,        0.00012
        6.04675,        0.00014
```

![image](https://facert.gitbooks.io/python-data-structure-cn/2.%E7%AE%97%E6%B3%95%E5%88%86%E6%9E%90/2.6.%E5%88%97%E8%A1%A8/assets/2.6.%E5%88%97%E8%A1%A8.poptime.png)

