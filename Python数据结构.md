# Python数据结构/函数/文件

## 数据结构

### 元组

定义为：`(1,2,3,4,5)` 元组的大小内容不可修改

生成式：`tuple`

元组拆分：`a,b,*rest=(1,2,3,4,5)` rest为常用写法，一般不需要的变量常写做：`_`

方法：`count`统计某元素出现的频率

### 列表

#### 基本介绍

生成式：`[]或list`

方法：`remove(val)`寻找第一个val并去除。

​		`  in与not in`检查列表是否包含某个值

​		`extend`在列表尾部追加多个元素	

​		`sort(key='')`，将列表原地排序，其中key为二级排序	



#### 二分搜索和维护

`bisect`模块支持二分查找：

`bisect.bisect`可以找到插入值后仍保证排序的位置

`bisect.insort`是向这个位置插入值

在使用前要将列表排序

```python
In [67]: import bisect
In [68]: c = [1, 2, 2, 2, 3, 4, 7]
    
In [70]: bisect.bisect(c, 5)
Out[70]: 6
    
In [71]: bisect.insort(c, 6)
In [72]: c
Out[72]: [1, 2, 2, 2, 3, 4, 6, 7]
```



#### 切片

形式为： `start:end:step`第一个冒号表示切片的起终点，第二个冒号表示step

负数表明从后向前切片

```python
In [73]: seq = [7, 2, 3, 7, 5, 6, 0, 1]
In [79]: seq[-4:]
Out[79]: [5, 6, 0, 1] 
```

间隔取元素

```python
In [81]: seq[::2]
Out[81]: [7, 3, 3, 6, 1] 
```

使用`-1`，可以将列表或元组颠倒过来：

```python
In [82]: seq[::-1]
Out[82]: [1, 0, 6, 5, 3, 6, 3, 2, 7]
```

 

迭代序列并跟踪当前位置的方法： `enumerate`函数，返回`(i, value)`元组序列

`sorted(key='')`函数可以从任意序列的元素返回一个新的排好序的列表：

`zip`可以将多个列表、元组或其它序列成对组合成一个**元组列表**：

```python
In [89]: seq1 = ['foo', 'bar', 'baz']

In [90]: seq2 = ['one', 'two', 'three']

In [91]: zipped = zip(seq1, seq2)

In [92]: list(zipped)
Out[92]: [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]

In [93]: seq3 = [False, True]

In [94]: list(zip(seq1, seq2, seq3))
Out[94]: [('foo', 'one', False), ('bar', 'two', True)] //元素的个数取决于最小的
```

利用`zip`函数同时迭代多个序列

```python
In [95]: for i, (a, b) in enumerate(zip(seq1, seq2)):
   ....:     print('{0}: {1}, {2}'.format(i, a, b))
   
0: foo, one
1: bar, two
2: baz, three
```

给出一个“被压缩的”序列，`zip`可用来解压序列。也可以当作把行的列表转换为列的列表。

```python
In [96]: pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'),
   ....:             ('Schilling', 'Curt')]

In [97]: first_names, last_names = zip(*pitchers)

In [98]: first_names
Out[98]: ('Nolan', 'Roger', 'Schilling')

In [99]: last_names
Out[99]: ('Ryan', 'Clemens', 'Curt')
```

`reversed`可以从后向前迭代一个序列



### 字典

#### 介绍

生成式：`{key:val}` `dict`

字典接受二元组列表(zip)

`mapping=dict(zip(range(5),reversed(range(5))))`

`in/not in`检查是否包含某个键

可以用`del`关键字或`pop`方法（返回值的同时删除键）删除值

`keys`和`values`是字典的键和值的迭代器方法。虽然键值对没有顺序，这两个方法可以用相同的顺序输出键和值

方法`a.update(b)`可以将一个字典与另一个融合，原地改变`a`

`get`方法，取默认值并返回`value = some_dict.get(key, default_value)`

`setdefault(key,value)`，设置默认值：

```python
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = {}

for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
    
In [126]: by_letter
Out[126]: {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
```



使用`collections`模块中的 `defaultdict`类，传输**类型**或**函数**生成每个位置的默认值

```python
from collections import defaultdict
by_letter = defaultdict(list)
```

#### 有效的键值类型

必须是可`hash`的值（标量，元组等）（相应的`hash`函数检测）

列表不可被哈希，可将列表转化为元组

### 列表、集合、字典推导式

列表推导式

```python
[expr for val in collection if condition]
//等价
result = []
for val in collection:
    if condition:
        result.append(expr)
```

字典推导式

```
dict_comp = {key-expr : value-expr for value in collection if condition}
```

集合推导式

```
set_comp = {expr for value in collection if condition}
```

### 嵌套列表推导

```python
In [161]: all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'],
   .....:             ['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar']]
   
names_of_interest = []
for names in all_data:
    enough_es = [name for name in names if name.count('e') >= 2]
    names_of_interest.extend(enough_es)  

#等价  注意嵌套的顺序和实际循环的顺序相同
In [162]: result = [name for names in all_data for name in names
   .....:           if name.count('e') >= 2]

In [163]: result
Out[163]: ['Steven']
```

## 函数

### 函数的参数

函数可以有一些位置参数（positional）和一些关键字参数（keyword）。关键字参数通常用于指定默认值或可选参数。

```python
def my_function(x, y, z=1.5): #x,y 为位置参数，z为关键字参数
my_function(5, 6, z=0.7)
my_function(3.14, 7, 3.5)
my_function(10, 20) 
#关键字参数必须位于位置参数（如果有的话）之后。你可以任何顺序指定关键字参数。
#也可以用关键字传递位置参数
my_function(x=5, y=6, z=7)
my_function(y=6, x=5, z=7)
```

### 返回多个值

```python
def f():
    a = 5
    b = 6
    c = 7
    return a, b, c

a, b, c = f()
#也可以写为
return f()
```

### 函数也是对象

将需要在一组给定字符串上执行的所有运算做成一个列表：

```python
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)

clean_ops = [str.strip, remove_punctuation, str.title]

def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
            value = function(value)
        result.append(value)
    return result
```

### 匿名函数(lambda)

仅由单条语句构成，该语句的结果就是返回值

```python
def apply_to_list(some_list, f):
    return [f(x) for x in some_list]

ints = [4, 0, 1, 5, 6]
apply_to_list(ints, lambda x: x * 2)
```

### 柯里化：部分参数应用

add_numbers的第二个参数称为“柯里化的”（curried）。这里没什么特别花哨的东西，因为我们其实就只是定义了一个可以调用现有函数的新函数而已。内置的`functools`模块可以用`partial`函数将此过程简化：

```python
from functools import partial
add_five = partial(add_numbers, 5)
```

### 生成器

生成器（`generator`）是构造新的可迭代对象的一种简单方式。一般的函数执行之后只会返回单个值，而生成器则是以延迟的方式返回一个值序列，即每返回一个值之后暂停，直到下一个值被请求时再继续。要创建一个生成器，只需将函数中的`return`替换为`yeild`即可：

生成器表达式：（方括号改为圆括号）

```python
In [189]: gen = (x ** 2 for x in range(100))

In [190]: gen
Out[190]: <generator object <genexpr> at 0x7fbbd5ab29e8>
```

### iretools模块

例： `groupby`接受序列和函数

```python
In [193]: import itertools
In [194]: first_letter = lambda x: x[0]
In [195]: names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']
In [196]: for letter, names in itertools.groupby(names, first_letter):
   .....:     print(letter, list(names)) # names is a generator
A ['Alan', 'Adam']
W ['Wes', 'Will']
A ['Albert']
S ['Steven']
```

### 异常处理

利用 `try/except/finally/else`处理异常

```python
def attempt_float(x):
    try:
        return float(x)
    except(异常类型：type error/value error 等):
        return x
    else(只在try成功时执行):
        print('success')
    或
    try:
        return float(x)
    finally(在任何情况下都执行):
        return x
```

## 文件和操作系统

`open()`打开文件，`with open()`打开文件并自动关闭

文件读取方法：`read`、`seek`和`tell`。read会从文件返回字符，`tell`给出当前的字节位置，`seek`移动到指定位置

`unicode`编码

注意**字节**与**字符**的区别

## numpy基础

### ndarray对象

`ndarray`同构多维容器，描述函数`shape、dtype、ndim(描述维度)`

```python
In [17]: data.shape
Out[17]: (2, 3)

In [18]: data.dtype
Out[18]: dtype('float64')
```

#### ndarray的创建

- `np.array`：例`arr1 = np.array([1, 2, 3], dtype=np.float64)`
- `zeros`和`ones`：分别可以创建指定长度或形状的全0或全1数组。
- `empty`：可以创建一个没有任何具体值的数组。(2，3)需传入一个表示形状的**元组**
- `arange`：Python内置函数range的数组版

```python
In [32]: np.arange(15)
Out[32]: array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])
```

#### ndarray的数据类型

用`dtype`限定，检查，可用`astype`方法转换 (注意 ``np.string_`类型)

 ` float_arr = arr.astype(np.float64/other_arr.dtype)` 小数转化为整数时小数点部分会被截断。

> 笔记：调用astype总会创建一个新的数组（一个数据的备份），即使新的dtype与旧的dtype相同。

#### ndarray的计算

大小相等的数组之间的计算会应用到元素级。

数组与标量的运算会传播到各个元素。

大小相同的数组之间的比较会生成布尔值数组。

```python
arr=([[ 1.,  2.,  3.],
       [ 4.,  5.,  6.]])
arr2=([[  0.,   4.,   1.],
       [  7.,   2.,  12.]])

In [59]: arr2 > arr
Out[59]:
array([[False,  True, False],
       [ True, False,  True]], dtype=bool)
```

#### 基本的索引和切片

数组的切片是原数组的试图，即对切片的修改会反映到原数组上。

例：

```python
In [66]: arr_slice = arr[5:8]
Out[67]: array([12, 12, 12])

In [68]: arr_slice[1] = 12345

In [69]: arr
Out[69]: array([0,1,2,3,4,    12,12345,12,     8,9])
```

> 注意：如果你想要得到的是ndarray切片的一份副本而非视图，就需要明确地进行复制操作，例如`arr[5:8].copy()`。

多维数组：

二维数组中：0轴为行，1轴为列

可以传入一个以逗号隔开的索引列表来选取单个元素。也就是说，下面两种方式是等价的：

```python
In [74]: arr2d[0][2]
Out[74]: 3

In [75]: arr2d[0, 2]
Out[75]: 3
```

#### 切片索引

切片是沿着一个轴向选取元素的。表达式`arr2d[:2]`选取`arr2d`的前两行。

多维切片类似多维索引

```python
In [90]: arr2d
Out[90]: array([[1, 2, 3],
       		  [4, 5, 6],
      		  [7, 8, 9]])
In [92]: arr2d[:2, 1:]
Out[92]: array([[2, 3],
         [5, 6]])
```

切片与索引结合，可以获得低维度切片

```python
In [93]: arr2d[1, :2]
Out[93]: array([4, 5])
```

#### 布尔型索引

即用`bool`型的数组作为索引 (如`data[names == 'Bob', 2:]`)

可更换比较符号为：`!=`或用 `&、|、~`与或非进行条件整合

> 注意：Python关键字and和or在布尔型数组中无效。要使用&与|。

#### 花式索引

指利用整数数组进行索引，如`arr[[1,3,4],[2,1,3],[]]`每一个数组对应原arr中的每一维。如上述选择的是 `(1,2),(3,1),(4,3)`

> 花式索引与切片不同，它总是将数据复制到新数组中。

#### 数组转置和轴对换

`arr.T`或`arr.transpose()`

`np.dot(a,b)`实现矩阵内积计算

对于高维数组，transpose需要得到一个由轴编号组成的元组，如`arr.transpose(1,0,2)`即将` 0，1`维进行互换

`arr.swapaxes(1, 2)`互换`1，2`维

### 通用函数 Ufuncs

一元函数`np.sqrt/np.exp` 

二元函数如 `add/maximum(a,b)`

`modf()`返回小数部分和整数部分

ufncs可接受一个可选参数表示输出到某对象，如 `sqrt(arr,arr)`完成数组原地操作![img](https://upload-images.jianshu.io/upload_images/7178691-1d494e73b61c7ced.png)

![img](https://upload-images.jianshu.io/upload_images/7178691-2be79faf68ab6ff8.png)

![img](https://upload-images.jianshu.io/upload_images/7178691-eff1e61e5464159f.png)

### 矢量化处理数据b

`np.where(condition,x,y)` true则取x，false则取y

#### 数学和统计方法

`sum/mean`分别计算和与平均值，可作为方法或函数使用

`std/var`标准差和方差

`argmin/argmax`最小和最大元素的索引

`arr.mean(axis=1)`计算行的平均值（把每列的数加总求平均）

`cumsum/cumprod`累加和累积

`any`检查是否存在一个true ，`all`检查是否全为true

`arr.sort()`就地排序，可传入需要排序的轴序号

#### 唯一化及其他集合逻辑

`np.unique`唯一化并返回已排序结果

`np.in1d`测试一个数组中的值在另一个数组中的成员资格。

#### 文件输入输出

`np.save/np.load`默认二进制格式保存

#### 线性代数

`dot(x.dot(y)/np.dot(x,y))`点积。或用@运算符

![img](https://upload-images.jianshu.io/upload_images/7178691-dcdb66e49e5f70ea.png)

#### 伪随机数生成

`np.random`库

`np.random.normal(size=(5,5))`生成标准正态分布5x5样本

`np.random.seed(x)`设置全局随机数种子

`rng = np.random.RandomState(1234)，rng.randn(10)`创建一个与其他隔离的随机数生成器

![img](https://upload-images.jianshu.io/upload_images/7178691-97ba09c96dab93a2.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

## pandas

### pandas基础数据结构

#### Series

Series：类似于一维数组的对象，由一组数据（各种NumPy数据类型）及索引组成。

属性：`values/index`值和索引  `name`

Series类似字典，可用字典创建series对象

检测缺失数据 `isnull/notnull`返回bool型的series

series 的重要功能：根据运算的索引标签自动对齐数据

#### Dataframe

表格型数据结构，可以看作是多个series组成的字典，他们共用一个索引

`head()`方法，取前5行。

创建：

```python
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'],index=['one', 'two', 'three', 'four','five', 'six'])
#其中index 为索引， colums为列标签
```

利用字典标记或属性标记获取series `frame2['state']/frame2.state`

为不存在的列赋值可创造新列，用`del`可以删除列

`frame2['eastern'] = frame2.state == 'Ohio'`(不能用 `frame2.eastern`创建新列)

`del frame2['eastern']`

> 注意：通过索引方式返回的列是相应数据的视图，不是副本。因此，对返回的Series所做的任何就地修改全都会反映到源DataFrame上。通过Series的copy方法即可指定复制列。

利用嵌套字典创建Dataframe

`In [65]: pop = {'Nevada': {2001: 2.4, 2002: 2.9},
   				 'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}`外层字典的键为列，内层键为索引（也可另外指定索引，会自动取值）

可用 `numpy`中类似的方法对进行行列交换

可设置 index 和 columns的 name属性：` frame3.index.name = 'year'; frame3.columns.name = 'state'`

Dataframe的values属性会以二维ndarray返回且按照index的顺序取。

#### 索引对象

即`dataframe.index` 包括行和列。

### 基本功能

#### 重新索引

pandas对象的方法，作用是创造一个新对象，数据符合新的索引

`obj2=obj.reindex(['a', 'b', 'c', 'd', 'e'])`

可使用 `method`选项进行如插值的处理。 `obj3.reindex(range(6),method='ffill')`

对于 `datafarme`的索引修改，行采用 `reindex()`，列采用 `reindex(columns=)`

#### 丢弃指定轴上的项

`drop`方法，返回删除了指定值的新对象

对于 `dataframe`的对象，可直接调用 `data.drop(['Colorado', 'Ohio'])`删除行。传递 `axis=1/'columns'`参数删除列 `data.drop('two', axis=1)`。（drop会原地修改对象）

可选参数： `inplace`本地文件修改

#### 索引、选取和过滤

`series`对象可用如 `obj[1]，obj[2:4]， obj[[1, 3]]`等索引

若采用标签切片如：`obj['b':'c']`其末端包含

`dataframe`中，数字取行`data[:2]`，标签取列

bool型索引，类似二维numpy，如 `data[data < 5] = 0`会对所有位置进行检查

#### loc与iloc

`loc`标签定位 `data.loc[行标签,列标签]`例如：`data.loc['Colorado', ['two', 'three']]`

`iloc`整数定位 `data.iloc[行标签,列标签]`例如： `data.iloc[2, [3, 0, 1]]`

也可添加多个标签 如 `data.iloc[:, :3][data.three > 5]`取 >5的所有行前3列

#### 整数索引

注意当索引为整数时，其被视为标签，不能再用顺序整数索引

#### 算数运算和数据对齐

对象相加时，结果的为并集

增加填充值`df1.add(df2, fill_value=0)`，没有重叠的位置设为0。 

算术方法：其中加r的副本会反转参数。

![img](https://upload-images.jianshu.io/upload_images/7178691-16857a1021f98d1f.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

在对Series或DataFrame重新索引时，也可以指定一个填充值：` df1.reindex(columns=df2.columns, fill_value=0)`

#### DataFrame与Series之间的运算

默认情况下，DataFrame和Series之间的算术运算会将Series的索引匹配到DataFrame的列，然后沿着行一直向下广播：`frame - series`

若要匹配行且在列上广播，需要使用算术运算方法并规定 `axis`参数：`frame.sub(series3, axis='index'/0)`

#### 函数应用

`apply`方法，将函数应用到各列或行的一维数组上：` frame.apply(f)`可指定参数 `axis`，如 `frame.apply(f, axis='columns')`

`applymap`方法，函数应用到元素级

#### 排序和排名

对数据集排序 `sorting`

对行列索引排序 `sort_index(axis=,ascending=True/False)` ascending指升序

按值进行排序，`sort_value(by='')`by指定按照某列或多个列中的值进行排序。缺失值会被放到末尾。

排名 `rank`

#### 带有重复标签的轴索引

对于带有重复值的索引，如果某个索引对应多个值，返回Series；对应单个值的，返回标量值：

```python
In [225]: obj['a']
Out[225]: 
a    0
a    1
dtype: int64

In [226]: obj['c']
Out[226]: 4
```

### 汇总和计算描述统计
