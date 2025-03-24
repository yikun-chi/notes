# PyTorch Functions

## Tensor Operation

### [torch.tensor.scatter\_](https://pytorch.org/docs/stable/generated/torch.Tensor.scatter_.html#torch-tensor-scatter)

Tensor.scatter\_(_dim_, _index_, _src_, _reduce=None_) → [Tensor](https://pytorch.org/docs/stable/tensors.html#torch.Tensor)

* **dim** ([_int_](https://docs.python.org/3/library/functions.html#int)) – the axis along which to index
* **index** (_LongTensor_) – the indices of elements to scatter, can be either empty or of the same dimensionality as `src`. When empty, the operation returns `self` unchanged.
* **src** ([_Tensor_](https://pytorch.org/docs/stable/tensors.html#torch.Tensor) _or_ [_float_](https://docs.python.org/3/library/functions.html#float)) – the source element(s) to scatter.
* **reduce** ([_str_](https://docs.python.org/3/library/stdtypes.html#str)_, optional_) – reduction operation to apply, can be either `'add'` or `'multiply'`.

The operation replaces values in _self_ by values in _src_ in ways specified by _index._&#x20;

```python
>>> src = torch.arange(1, 11).reshape((2, 5))
>>> src
tensor([[ 1,  2,  3,  4,  5],
        [ 6,  7,  8,  9, 10]])
>>> index = torch.tensor([[0, 1, 2, 0]])
>>> torch.zeros(3, 5, dtype=src.dtype).scatter_(0, index, src)
tensor([[1, 0, 0, 4, 0],
        [0, 2, 0, 0, 0],
        [0, 0, 3, 0, 0]])
>>> index = torch.tensor([[0, 1, 2], [0, 1, 4]])
>>> torch.zeros(3, 5, dtype=src.dtype).scatter_(1, index, src)
tensor([[1, 2, 3, 0, 0],
        [6, 7, 0, 0, 8],
        [0, 0, 0, 0, 0]])
```

* The scatter function dim is 0, so we know the index input is specifying the new index in dimension 0, and the rest of the dimensions follow the original dimension in src.&#x20;
* Look at _src\[0]\[1]=2_, the corresponding index value is _index\[0]\[1]=1_, so we know that _src\[0]\[1]_ will be placed into _self\[1]\[1]_.&#x20;
* Look at _src\[0]\[2]=3_, the corresponding index value is _index\[0]\[2] = 2,_ so we know that _src\[0]\[2]_ will be placed into sel&#x66;_\[2]\[2]_
* Because index tensor shape is \[1,4], so the second row of source tensors are not placed

<pre class="language-python"><code class="lang-python"><strong># Using scatter_ to create one-hot encoding based on value of y 
</strong><strong>torch.zeros(10, dtype=torch.float).scatter_(0, torch.tensor(y), value=1)
</strong></code></pre>

## [torch.mean](https://pytorch.org/docs/stable/generated/torch.mean.html#torch-mean)

torch.mean(_input_, _dim_, _keepdim=False_, _\*_, _dtype=None_, _out=None_) → [Tensor](https://pytorch.org/docs/stable/tensors.html#torch.Tensor)

* **input** ([_Tensor_](https://pytorch.org/docs/stable/tensors.html#torch.Tensor)) – the input tensor.
* **dim** ([_int_](https://docs.python.org/3/library/functions.html#int) _or tuple of ints_) – the dimension or dimensions to reduce.
* **keepdim** ([_bool_](https://docs.python.org/3/library/functions.html#bool)) – whether the output tensor has `dim` retained or not.

```python
a = torch.tensor([x for x in range(24)], dtype = float).reshape((2,3,4))

# 1
a.mean(dim = 0).shape
# (3,4)

# 2
a.mean(dim = 1).shape
# (2,4)

# 3
a.mean(dim = (0,2)).shape
# (3)

# think of a as an image with 2 channels, 3 by 4
```

1. Reducing over dimension 0. e.g.,: _Output\[0]\[0]_ is the mean of _Input\[0]\[0]\[0]_ and _input\[1]\[0]\[0]_
2. Reducing over dimension 1. e.g.,: _Output\[0]\[0]_ is the mean of _Input\[0]\[0]\[0]_, _Input\[0]\[1]\[0]_ and _Input\[0]\[2]\[0]_
3. Reducing over dimension 0 and 2. e.g.,: _Output\[0]_ is the mean of _\[0]\[0]\[0], \[0]\[0]\[1], \[0]\[0]\[2], \[0]\[0]\[3], \[1]\[0]\[0], \[1]\[0]\[1], \[1]\[0]\[2], \[1]\[0]\[3]_

## [transform](https://pytorch.org/vision/main/generated/torchvision.transforms.Normalize.html)&#x20;

torchvision.transforms.Normalize(_mean_, _std_, _inplace=False_)

* **mean** (_sequence_) – Sequence of means for each channel.
* **std** (_sequence_) – Sequence of standard deviations for each channel.
* **inplace** ([_bool_](https://docs.python.org/3/library/functions.html#bool)_,optional_) – Bool to make this operation in-place.

```python
# a is a batch of 2 image, each image is 2 channel 
a = torch.tensor([x for x in range(16)], dtype = float).reshape((2,2,2,2))

# mean sequence 
m = torch.tensor([0,1])

fn = torchvision.transforms.Normalize(mean = m, std = torch.ones(1))

```

* For every image, the transformer will subtract 0 from every cell in channel 0, and subtract 1 from every cell in channel 1
