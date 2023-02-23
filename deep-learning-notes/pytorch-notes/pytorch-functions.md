# PyTorch Functions

## Tensor Operation

### [torch.tensor.scatter\_](https://pytorch.org/docs/stable/generated/torch.Tensor.scatter\_.html#torch-tensor-scatter)

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

* The scatter function _dim_ is 0, so we know the _index_ input is specifying the new index in dimension 0 and the rest of dimensions follows the original dimension in _src_.&#x20;
* Look at _src\[0]\[1]=2_, the corresponding index value is _index\[0]\[1]=1_, so we know that _src\[0]\[1]_ will be placed into _self\[1]\[1]_.&#x20;
* Look at _src\[0]\[2]=3_, the corresponding index value is _index\[0]\[2] = 2,_ so we know that _src\[0]\[2]_ will be placed into self_\[2]\[2]_
* Because index tensor shape is \[1,4], so the second row of source tensors are not placed in the first scatte rcall.&#x20;

<pre class="language-python"><code class="lang-python"><strong># Using scatter_ to create one-hot encoding based on value of y 
</strong><strong>torch.zeros(10, dtype=torch.float).scatter_(0, torch.tensor(y), value=1)
</strong></code></pre>
