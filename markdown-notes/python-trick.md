## 翻转
```python
x = x[-1::-1]
```
```python
x = x[-1::-1,-1::-1]
```

## 作用于
全局（外部）变量在函数中不加`global`修饰符，如果不赋值，只是引用，是不会报错的：

```python
a = 123

def f():
    global a
    a = 456

def g():
    print(a)

def h():
    a = 456
```