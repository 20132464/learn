Python中，string、tuple和number是不可变对象，而list、dict、set等是可变对象，需要注意的是：这里说的不可变指的是值的不可变。

对于变量（相对于“对象”），python函数参数传递有时是引用传递，有时是值传递。如果这个变量对应的对象值可变，就认为是引用，函数内外是同一个变量，如果这个
变量对应的对象值不可改变，就认为是赋值，函数内外是不同的变量。下面举例说明这两种对象对函数的参数传递的影响：
-----------------------------------
①
# 不可变对象 --> 赋值
def ChangeInt( a ):
    a = 10
nfoo = 2 
ChangeInt(nfoo)
print(nfoo) #结果是2
#传入函数的变量 a 可以认为是一个新的变量，与 nfoo 一同指向对象 2 ，后来  a = 10 ，让这个新的变量指向了对象10，由于是两个变量，函数里 a 的改变并没有
改变 nfoo 的指向。
------------------------------------
②
# 可变对象 --> 引用
def ChangeList( a ):
    a[0] = 10
lstFoo = [2]
ChangeList(lstFoo )
print nfoo #结果是[10]
# 这里，a 和 lstFoo 是一个变量，函数内部的修改影响了函数外的变量。
------------------------------------
赋值是将一个对象的地址赋值给一个变量，让变量指向该地址（ 旧瓶装旧酒 ）。
修改不可变对象（str、tuple）需要开辟新的空间
修改可变对象（list等）不需要开辟新的空间
------------------------------------
浅拷贝和深拷贝：
我们常用到的拷贝方式：

1）没有限制条件的分片表达式（L[:]）能够复制序列，但此法只能浅层复制。

2）字典 copy 方法，D.copy() 能够复制字典，但此法只能浅层复制

3）有些内置函数，例如 list，能够浅拷贝 list(L)

4）copy 标准库模块能够生成完整拷贝：deepcopy 本质上是递归 copy
------------------------------------
浅拷贝是在另一块地址中创建一个新的变量或容器，但是容器内的元素的地址均是源对象的元素的地址的拷贝。也就是说新的容器中指向了旧的元素（ 新瓶装旧酒 ）。
浅拷贝属于“新瓶装旧酒”，即生成了一个新的变量，而变量所指向的对象和原来的是一样的：
①
l = ["hello", [2, 3, 4]]
id(l) # 3048239386824
[id(i) for i in l] # [1524761040, 1524761072]

k = l.copy()
id(k) # 3048239387080，地址不同，k是另一个变量
[id(i) for i in k] # [1524761040, 1524761072]，地址相同，指向同一个变量
②
import copy

a = ["张小鸡"]
b = a
c = copy.copy(a)
print('a的id', id(a))
print("赋值：id(b)->>>", id(b))
print("浅拷贝：id(c)->>>", id(c))
输出结果：
a的id 89587670216
赋值：id(b)->>> 89587670216
浅拷贝：id(c)->>> 89543170376
③
>>> a=['hello',[1,2,3]]
>>> b=a[:]
>>> [id(x) for x in a]
[55792504, 6444104]
>>> [id(x) for x in b]
[55792504, 6444104]
>>> a[0]='world'
>>> a[1].append(4)
>>> print(a)
['world', [1, 2, 3, 4]]
>>> print(b)
['hello', [1, 2, 3, 4]]
-----------------------------------
深拷贝属于“新瓶装新酒”，即生成了一个新变量，指向和原对象相等的新对象（不可变对象除外）：
import copy

l = ["hello world", [2, 3, 4]]
id(l) # 3048239386824
[id(i) for i in l] # [3048239385040, 3048239387080]

k = copy.deepcopy(l)
id(k) # 3048240927048，地址不同，k是另一个变量
[id(i) for i in k]  # [3048239385040, 3048240927304]
字符串是不可变对象，所以仍指向原地址，对于list则分配了一片新的内存空间，只是值与原对象相同
--------------------------------------------------------
当list类型的对象进行append操作时，实际上追加的是该对象的引用。
对于可变对象，对象的操作不会重建对象，而对于不可变对象，每一次操作就重建新的对象。
--------------------------------------------------------
import copy
a = [1, 2, 3, 4, ['a', 'b']] #原始对象
 
b = a                       #赋值，传对象的引用
c = copy.copy(a)            #对象拷贝，浅拷贝
d = copy.deepcopy(a)        #对象拷贝，深拷贝
 
a.append(5)                 #修改对象a
a[4].append('c')            #修改对象a中的['a', 'b']数组对象
a[1] = 9
print( 'a = ', a )
print( 'b = ', b )
print( 'c = ', c )
print( 'd = ', d )
得出的结果：
a =  [1, 9, 3, 4, ['a', 'b', 'c'], 5]
b =  [1, 9, 3, 4, ['a', 'b', 'c'], 5]
c =  [1, 2, 3, 4, ['a', 'b', 'c']]
d =  [1, 2, 3, 4, ['a', 'b']]
-----------------------------
http://wsfdl.com/python/2013/08/16/%E7%90%86%E8%A7%A3Python%E7%9A%84%E6%B7%B1%E6%8B%B7%E8%B4%9D%E5%92%8C%E6%B5%85%E6%8B%B7%E8%B4%9D.html
这个里的图画的特别好，通过画图很容易理解
引用：
>>> b = [1 , 2]
>>> a = [b, 3, 4]
>>>
>>> c = a
>>> print c
[[1, 2], 3, 4]
>>> id(a)
4408148552
>>> id(c)
4408148552
c = a 表示 c 和 a 指向相同的地址空间，并没有创建新的对象。
![引用](https://github.com/20132464/learn/blob/master/3.png)
-----------------------------------------
浅拷贝
>>> import copy
>>> d = copy.copy(a)
>>> print d
[[1, 2], 3, 4]
>>>
>>> id(a)
4408148552
>>> id(d)
4408199792
>>>
>>> id(a[0])
4408022944
>>> id(d[0])
4408022944
>>>
>>> d[0][0] = 5
>>> print a
>>> [[5, 2], 3, 4]
d = copy.copy(a) 创建了一个新对象，复制了原有对象的引用。
![浅拷贝](https://github.com/20132464/learn/blob/master/4.png)
----------------------------------------
深拷贝：
>>> e = copy.deepcopy(a)
>>> print e
>>> [[1, 2], 3, 4]
>>>
>>> id(a)
>>> 4408148552
>>> id(e)
>>> 4408394792
>>>
>>> id(a[0])
>>> 4408022944
>>> id(e[0])
>>> 4408398432
>>>
>>> e[0][0] = 5
>>> print a
>>> [[1, 2], 3, 4]
e = copy.deepcopy(a) 新建了一个新对象，完整的在内存中复制原有对象。
![深拷贝](https://github.com/20132464/learn/blob/master/5.png)
----------------------------------
https://zhuanlan.zhihu.com/p/57893374
这篇图解也较好
-----------------------------
import copy

a = ["张小鸡"]

print "改变前，a内部的元素id：id([a])->>>", [id(_) for _ in a]

c = copy.copy(a)

print "改变前，浅拷贝c内部的元素id：id([c])->>>", [id(_) for _ in c]

a[0] = "姬无命"

print "改变后，a内部的元素id：id([a])->>>", [id(_) for _ in a]
print "改变后，浅拷贝c内部的元素id：id([c])->>>", [id(_) for _ in c]
输出：
改变前，a内部的元素id：id([a])->>> [4318150256]
改变前，浅拷贝c内部的元素id：id([c])->>> [4318150256]
改变后，a内部的元素id：id([a])->>> [4318150352]
改变后，浅拷贝c内部的元素id：id([c])->>> [4318150256]

![理解图1](https://github.com/20132464/learn/blob/master/1.jpg)
---------------------------------------
操作不可变对象时，由于引用计数的特性，被拷贝的元素改变时，就相当于撕掉了原来的标签，重新贴上新的标签一样，对于我们已拷贝的元素没有任何影响。
因此在操作不可变对象时，浅拷贝和深拷贝是没有区别的
import copy
import json

a = [["张小鸡"], "姬无命"]

print "改变前，a的值", json.dumps(a, ensure_ascii=False)
print "改变前，a内部的元素id：id([a])->>>", [id(_) for _ in a]

c = copy.copy(a)

print "改变前，c的值", json.dumps(c, ensure_ascii=False)
print "改变前，浅拷贝c内部的元素id：id([c])->>>", [id(_) for _ in c]

a[0][0] = "Tom"
a[1] = "Jack"

print "改变后，a的值", json.dumps(a, ensure_ascii=False)
print "改变后，c的值", json.dumps(c, ensure_ascii=False)
print "改变后，a内部的元素id：id([a])->>>", [id(_) for _ in a]
print "改变后，浅拷贝c内部的元素id：id([c])->>>", [id(_) for _ in c]
输出：
改变前，a的值 [["张小鸡"], "姬无命"]
改变前，a内部的元素id：id([a])->>> [4385503208, 4373939232]
改变前，c的值 [["张小鸡"], "姬无命"]
改变前，浅拷贝c内部的元素id：id([c])->>> [4385503208, 4373939232]
改变后，a的值 [["Tom"], "Jack"]
改变后，c的值 [["Tom"], "姬无命"]
改变后，a内部的元素id：id([a])->>> [4385503208, 4373938320]
改变后，浅拷贝c内部的元素id：id([c])->>> [4385503208, 4373939232]
![理解图2](https://github.com/20132464/learn/blob/master/2.jpg)
