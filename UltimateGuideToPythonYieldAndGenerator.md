## 一篇看懂Python `yield`

### 不容易理解的`yield` 

`yield`和`return`表面有点像：在一个函数里面，当执行到yield时，程序返回被yield的值。比如说，

```python
def yield1():
  yield 1
```

此时当调用该函数时，

```python
it = yield1()
print(next(it))
```

程序返回1。

但和return不一样的是，当yield返回时，程序仍然保持了函数的所有状态。当你下次调用时，程序进入该函数后，会从上一次执行到的地方继续往下执行。比如说，

```python
def yield123():
  yield 1
  yield 2
  yield 3
```

```python
it = yield1()
print(next(it))
print(next(it))
print(next(it))
```

依次打出1、2、3。换句话说，当第二次调用时，函数不会从头开始执行，而是从上一次的`yield 1`之后的`yield 2`开始执行。为了更方便地理解这个过程，我们可以把函数改写一下

```python
def yield_123():
    print('enter func for 1st time')
    yield 1

    print('enter func for 2nd time')
    yield 2

    print('enter func for 3rd time')
    yield 3

    print('enter func for 4th time')
```

```python
it = yield_123()
print(next(it))
print(next(it))
print(next(it))
```

此时输出分别是

```
enter func for 1st time
1
enter func for 2nd time
2
enter func for 3rd time
3
```

第一次执行时，打出了第一个print，然后在`yield 1`之后返回了。但是程序“记住”了“上一次”执行到的位置。当下一次进来时，从第二个print开始执行，然后在`yield 2`之后就返回了。

注意，这里程序不会执行到`print('enter func for 4th time')`，因为到`yield 3`就返回了。如果我们再来一个`print(next(it))`，你会发现程序有两个输出：

首先是

```
enter func for 4th time
```

这个很好理解，因为上一次是到`yield 3`，那么这一次就是继续往下执行`enter func for 4th time` 这句话。不过除了这个，还会报一个异常：

```
Traceback (most recent call last):
  File "path/main.py", line 24, in <module>
    print(next(it))
StopIteration
```

`StopIteration`告诉你迭代已经结束了，不能再往下走了。






    











