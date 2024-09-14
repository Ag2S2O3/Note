# 核心数据结构 Tensor

注意：torch.Tensor是Python的一个类, torch.tensor是Python的一个函数

## 一、张量的作用

tensor之于pytorch等同于ndarray之于numpy,它是pytorch中最核心的数据结构,用于表达各类数据,如输入数据、模型的参数、模型的特征图、模型的输出等。这里边有一个很重要的数据,就是模型的参数。对于模型的参数,我们需要更新它们,而更新操作需要记录梯度,**梯度的记录功能**正是被张量所实现的

## 二、张量的结构

Tensor作为一个**类**，主要有以下八个主要属性

* data:多维数组,最核心的属性,其他属性都是为其服务的;
* dtype:多维数组的数据类型;  
* shape:多维数组的形状;  
* device: tensor所在的设备,cpu或cuda;  
* grad,grad_fn,is_leaf和requires_grad: 与Variable一样,都是梯度计算中所用到的

## 三、相关函数

### 3.1 张量的创建

#### 3.1.1 直接创建

``` python
torch.tensor(data, dtype=None, device=None, requires_grad=False, pin_memory=False)
//后面的参数都有默认值
```

* data(array_like) - tensor的**初始数据**,可以是list, tuple, numpy array, scalar或其他类型。  
* dtype(torch.dtype, optional) - tensor的**数据类型**,如torch.uint8, torch.float, torch.long等  
* device (torch.device, optional) – 决定tensor位于**cpu还是gpu**。如果为None,将会采用默认值,默认值在 `torch.set_default_tensor_type() `中设置,默认为cpu。  
* requires_grad (bool, optional) – 决定是否**需要计算梯度**。  
* pin_memory (bool, optional) – **是否将tensor存于锁页内存**。这与内存的存储方式有关,通常为False。

注意，使用 `torch.from_numpy()` 可以直接从numpy中创建张量，但是里面的数据会与numpy中的数组共享一块内存（即同时变化）

#### 3.1.2 给定值创建

```python
torch.zeros(*size, out=None, dtype=None, layout=torch.strided, device=None, requires_grad=False)
```

* 使用size指定大小（如3*3）
* out(tensor, optional) - 输出的tensor,即该函数返回的tensor**可以通过out进行赋值给别的张量**
* layout(torch.layout, optional) - 参数表明张量在内存中采用何种**布局方式**。常用的有torch.strided, torch.sparse_coo等

---

一个使用out赋值的例子：

```python
import torch 
o_t = torch.tensor([1]) 
t = torch.zeros((3, 3), out=o_t)
```

本来o_t中的值应该是1，但是经过后面的out赋值后变成了三阶全零张量

---

使用 `torch.zeros_like(input, dtype=None, layout=None, device=None, requires_grad=False)` 可以创建一个与给定input形状相同的张量

而使用 `torch.full(size, fill_value, out=None, dtype=None, layout=torch.strided, device=None, requires_grad=False)` 可以自定义填充的数值（fill_value项），相应的，它也有"like"创建方式

---

``` python
torch.linspace(start, end, steps=100, out=None, dtype=None, layout=torch.strided, device=None,  requires_grad=False)
```

创建均分的1维张量,长度为steps,区间为[start, end],均为数值

---

```python
torch.logspace(start, end, steps=100, base=10.0, out=None, dtype=None, layout=torch.strided, device=None,  requires_grad=False)
```

创建对数均分的1维张量,长度为steps, 底为base

---

```python
torch.eye(n, m=None, out=None, dtype=None, layout=torch.strided, device=None, requires_grad=False)
```

创建单位对角矩阵,n为矩阵的行数，m为矩阵的列数（默认值为n,即默认创建一个方阵）

---

```python
torch.rand(*size, out=None, dtype=None, layout=torch.strided, device=None, requires_grad=False)
```

在区间[0, 1)上,生成均匀分布

