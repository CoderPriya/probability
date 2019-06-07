<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tfp.positive_semidefinite_kernels.FeatureTransformed" />
<meta itemprop="path" content="Stable" />
<meta itemprop="property" content="batch_shape"/>
<meta itemprop="property" content="dtype"/>
<meta itemprop="property" content="feature_ndims"/>
<meta itemprop="property" content="kernel"/>
<meta itemprop="property" content="name"/>
<meta itemprop="property" content="name_scope"/>
<meta itemprop="property" content="submodules"/>
<meta itemprop="property" content="trainable_variables"/>
<meta itemprop="property" content="transformation_fn"/>
<meta itemprop="property" content="variables"/>
<meta itemprop="property" content="__add__"/>
<meta itemprop="property" content="__init__"/>
<meta itemprop="property" content="__mul__"/>
<meta itemprop="property" content="apply"/>
<meta itemprop="property" content="batch_shape_tensor"/>
<meta itemprop="property" content="matrix"/>
<meta itemprop="property" content="with_name_scope"/>
</div>

# tfp.positive_semidefinite_kernels.FeatureTransformed

## Class `FeatureTransformed`

Input transformed kernel.

Inherits From: [`PositiveSemidefiniteKernel`](../../tfp/positive_semidefinite_kernels/PositiveSemidefiniteKernel.md)



Defined in [`python/positive_semidefinite_kernels/feature_transformed.py`](https://github.com/tensorflow/probability/tree/master/tensorflow_probability/python/positive_semidefinite_kernels/feature_transformed.py).

<!-- Placeholder for "Used in" -->

Given a kernel `k` and function `f`, compute `k_{new}(x, y) = k(f(x), f(y))`.


### Examples

```python
import tensorflow as tf
import tensorflow_probability as tfp
from tensorflow_probability.positive_semidefinite_kernel.internal import util
tfpk = tfp.positive_semidefinite_kernels

base_kernel = tfpk.ExponentiatedQuadratic(amplitude=2., length_scale=1.)
```

- Identity function.

```python
# This is the same as base_kernel
same_kernel = tfpk.FeatureTransformed(
    base_kernel,
    transformation_fn=lambda x, _, _: x)
```

- Exponential transformation.

```python
exp_kernel = tfpk.FeatureTransformed(
    base_kernel,
    transformation_fn=lambda x, _, _: tf.exp(x))
```

- Transformation with broadcasting parameters.

```python
# Exponentiate inputs

p = np.random.uniform(low=2., high=3., size=[10, 2])
def inputs_to_power(x, feature_ndims, param_expansion_ndims):
  # Make sure we account for extra feature dimensions for
  # broadcasting purposes.
  power = util.pad_shape_with_ones(
      p,
      ndims=feature_ndims + param_expansion_ndims,
      start=-(feature_ndims + 1))
  return x ** power

power_kernel = tfpk.FeatureTransformed(
  base_kernel, transformation_fn=inputs_to_power)

<h2 id="__init__"><code>__init__</code></h2>

``` python
__init__(
    kernel,
    transformation_fn,
    validate_args=False,
    name='FeatureTransformed'
)
```

Construct an FeatureTransformed kernel instance.


#### Args:


* <b>`kernel`</b>: `PositiveSemidefiniteKernel` instance. Inputs are transformed and
  passed in to this kernel. Parameters to `kernel` must be broadcastable
  with parameters of `transformation_fn`.
* <b>`transformation_fn`</b>: Callable. `transformation_fn` takes in an input
  `Tensor`, a Python integer representing the number of feature
  dimensions, and a Python integer representing the
  `param_expansion_ndims` arg of `_apply`. Computations in
  `transformation_fn` must be broadcastable with parameters of `kernel`.
* <b>`validate_args`</b>: If `True`, parameters are checked for validity despite
  possibly degrading runtime performance
* <b>`name`</b>: Python `str` name prefixed to Ops created by this class.



## Properties

<h3 id="batch_shape"><code>batch_shape</code></h3>

The batch_shape property of a PositiveSemidefiniteKernel.

This property describes the fully broadcast shape of all kernel parameters.
For example, consider an ExponentiatedQuadratic kernel, which is
parameterized by an amplitude and length_scale:

```none
exp_quad(x, x') := amplitude * exp(||x - x'||**2 / length_scale**2)
```

The batch_shape of such a kernel is derived from broadcasting the shapes of
`amplitude` and `length_scale`. E.g., if their shapes were

```python
amplitude.shape = [2, 1, 1]
length_scale.shape = [1, 4, 3]
```

then `exp_quad`'s batch_shape would be `[2, 4, 3]`.

Note that this property defers to the private _batch_shape method, which
concrete implementation sub-classes are obliged to provide.

#### Returns:

`TensorShape` instance describing the fully broadcast shape of all
kernel parameters.


<h3 id="dtype"><code>dtype</code></h3>

DType over which the kernel operates.


<h3 id="feature_ndims"><code>feature_ndims</code></h3>

The number of feature dimensions.

Kernel functions generally act on pairs of inputs from some space like

```none
R^(d1 x ... x  dD)
```

or, in words: rank-`D` real-valued tensors of shape `[d1, ..., dD]`. Inputs
can be vectors in some `R^N`, but are not restricted to be. Indeed, one
might consider kernels over matrices, tensors, or even more general spaces,
like strings or graphs.

#### Returns:

The number of feature dimensions (feature rank) of this kernel.


<h3 id="kernel"><code>kernel</code></h3>

Base kernel to pass transformed inputs.


<h3 id="name"><code>name</code></h3>

Name prepended to all ops created by this class.


<h3 id="name_scope"><code>name_scope</code></h3>

Returns a `tf.name_scope` instance for this class.


<h3 id="submodules"><code>submodules</code></h3>

Sequence of all sub-modules.

Submodules are modules which are properties of this module, or found as
properties of modules which are properties of this module (and so on).

```
a = tf.Module()
b = tf.Module()
c = tf.Module()
a.b = b
b.c = c
assert list(a.submodules) == [b, c]
assert list(b.submodules) == [c]
assert list(c.submodules) == []
```

#### Returns:

A sequence of all submodules.


<h3 id="trainable_variables"><code>trainable_variables</code></h3>

Sequence of variables owned by this module and it's submodules.

Note: this method uses reflection to find variables on the current instance
and submodules. For performance reasons you may wish to cache the result
of calling this method if you don't expect the return value to change.

#### Returns:

A sequence of variables for the current module (sorted by attribute
name) followed by variables from all submodules recursively (breadth
first).


<h3 id="transformation_fn"><code>transformation_fn</code></h3>

Function that preprocesses inputs before handing them to `kernel`.


<h3 id="variables"><code>variables</code></h3>

Sequence of variables owned by this module and it's submodules.

Note: this method uses reflection to find variables on the current instance
and submodules. For performance reasons you may wish to cache the result
of calling this method if you don't expect the return value to change.

#### Returns:

A sequence of variables for the current module (sorted by attribute
name) followed by variables from all submodules recursively (breadth
first).




## Methods

<h3 id="__add__"><code>__add__</code></h3>

``` python
__add__(k)
```




<h3 id="__mul__"><code>__mul__</code></h3>

``` python
__mul__(k)
```




<h3 id="apply"><code>apply</code></h3>

``` python
apply(
    x1,
    x2
)
```

Apply the kernel function to a pair of (batches of) inputs.


#### Args:


* <b>`x1`</b>: `Tensor` input to the first positional parameter of the kernel, of
  shape `[b1, ..., bB, f1, ..., fF]`, where `B` may be zero (ie, no
  batching) and `F` (number of feature dimensions) must equal the kernel's
  `feature_ndims` property. Batch shape must broadcast with the batch
  shape of `x2` and with the kernel's parameters.
* <b>`x2`</b>: `Tensor` input to the second positional parameter of the kernel,
  shape `[c1, ..., cC, f1, ..., fF]`, where `C` may be zero (ie, no
  batching) and `F` (number of feature dimensions) must equal the kernel's
  `feature_ndims` property. Batch shape must broadcast with the batch
  shape of `x1` and with the kernel's parameters.


#### Returns:

`Tensor` containing the (batch of) results of applying the kernel function
to inputs `x1` and `x2`. If the kernel parameters' batch shape is
`[k1, ..., kK]` then the shape of the `Tensor` resulting from this method
call is `broadcast([b1, ..., bB], [c1, ..., cC], [k1, ..., kK])`.


Given an index set `S`, a kernel function is mathematically defined as a
real- or complex-valued function on `S` satisfying the
positive semi-definiteness constraint:

```none
sum_i sum_j (c[i]*) c[j] k(x[i], x[j]) >= 0
```

for any finite collections `{x[1], ..., x[N]}` in `S` and
`{c[1], ..., c[N]}` in the reals (or the complex plane). '*' is the complex
conjugate, in the complex case.

This method most closely resembles the function described in the
mathematical definition of a kernel. Given a PositiveSemidefiniteKernel `k`
with scalar parameters and inputs `x` and `y` in `S`, `apply(x, y)` yields a
single scalar value. Given the same kernel and, say, batched inputs of shape
`[b1, ..., bB, f1, ..., fF]`, it will yield a batch of scalars of shape
`[b1, ..., bB]`.

#### Examples

```python
import tensorflow_probability as tfp

# Suppose `SomeKernel` acts on vectors (rank-1 tensors)
scalar_kernel = tfp.positive_semidefinite_kernels.SomeKernel(param=.5)
scalar_kernel.batch_shape
# ==> []

# `x` and `y` are batches of five 3-D vectors:
x = np.ones([5, 3], np.float32)
y = np.ones([5, 3], np.float32)
scalar_kernel.apply(x, y).shape
# ==> [5]
```

The above output is the result of vectorized computation of the five values

```none
[k(x[0], y[0]), k(x[1], y[1]), ..., k(x[4], y[4])]
```

Now we can consider a kernel with batched parameters:

```python
batch_kernel = tfp.positive_semidefinite_kernels.SomeKernel(param=[.2, .5])
batch_kernel.batch_shape
# ==> [2]
batch_kernel.apply(x, y).shape
# ==> Error! [2] and [5] can't broadcast.
```

The parameter batch shape of `[2]` and the input batch shape of `[5]` can't
be broadcast together. We can fix this by giving the parameter a shape of
`[2, 1]` which will correctly broadcast with `[5]` to yield `[2, 5]`:

```python
batch_kernel = tfp.positive_semidefinite_kernels.SomeKernel(
    param=[[.2], [.5]])
batch_kernel.batch_shape
# ==> [2, 1]
batch_kernel.apply(x, y).shape
# ==> [2, 5]
```

<h3 id="batch_shape_tensor"><code>batch_shape_tensor</code></h3>

``` python
batch_shape_tensor()
```

The batch_shape property of a PositiveSemidefiniteKernel as a `Tensor`.


#### Returns:

`Tensor` which evaluates to a vector of integers which are the
fully-broadcast shapes of the kernel parameters.


<h3 id="matrix"><code>matrix</code></h3>

``` python
matrix(
    x1,
    x2
)
```

Construct (batched) matrices from (batches of) collections of inputs.


#### Args:


* <b>`x1`</b>: `Tensor` input to the first positional parameter of the kernel, of
  shape `[b1, ..., bB, e1, f1, ..., fF]`, where `B` may be zero (ie, no
  batching), e1 is an integer greater than zero, and `F` (number of
  feature dimensions) must equal the kernel's `feature_ndims` property.
  Batch shape must broadcast with the batch shape of `x2` and with the
  kernel's parameters *after* parameter expansion (see
  `param_expansion_ndims` argument).
* <b>`x2`</b>: `Tensor` input to the second positional parameter of the kernel,
  shape `[c1, ..., cC, e2, f1, ..., fF]`, where `C` may be zero (ie, no
  batching), e2 is an integer greater than zero,  and `F` (number of
  feature dimensions) must equal the kernel's `feature_ndims` property.
  Batch shape must broadcast with the batch shape of `x1` and with the
  kernel's parameters *after* parameter expansion (see
  `param_expansion_ndims` argument).


#### Returns:

`Tensor containing (batch of) matrices of kernel applications to pairs
from inputs `x1` and `x2`. If the kernel parameters' batch shape is
`[k1, ..., kK]`, then the shape of the resulting `Tensor` is
`broadcast([b1, ..., bB], [c1, ..., cC], [k1, ..., kK]) + [e1, e2]`.


Given inputs `x1` and `x2` of shapes

```none
[b1, ..., bB, e1, f1, ..., fF]
```

and

```none
[c1, ..., cC, e2, f1, ..., fF]
```

This method computes the batch of `e1 x e2` matrices resulting from applying
the kernel function to all pairs of inputs from `x1` and `x2`. The shape
of the batch of matrices is the result of broadcasting the batch shapes of
`x1`, `x2`, and the kernel parameters (see examples below). As such, it's
required that these shapes all be broadcast compatible. However, the kernel
parameter batch shapes need not broadcast against the 'example shapes' (`e1`
and `e2` above).

When the two inputs are the (batches of) identical collections, the
resulting matrix is the so-called Gram (or Gramian) matrix
(https://en.wikipedia.org/wiki/Gramian_matrix).

N.B., this method can only be used to compute the pairwise application of
the kernel function on rank-1 collections. E.g., it *does* support inputs of
shape `[e1, f]` and `[e2, f]`, yielding a matrix of shape `[e1, e2]`. It
*does not* support inputs of shape `[e1, e2, f]` and `[e3, e4, f]`, yielding
a `Tensor` of shape `[e1, e2, e3, e4]`. To do this, one should instead
reshape the inputs and pass them to `apply`, e.g.:

```python
k = tfpk.SomeKernel()
t1 = tf.placeholder([4, 4, 3], tf.float32)
t2 = tf.placeholder([5, 5, 3], tf.float32)
k.apply(
    tf.reshape(t1, [4, 4, 1, 1, 3]),
    tf.reshape(t2, [1, 1, 5, 5, 3])).shape
# ==> [4, 4, 5, 5, 3]
```

`matrix` is a special case of the above, where there is only one example
dimension; indeed, its implementation looks almost exactly like the above
(reshaped inputs passed to the private version of `_apply`).

#### Examples

First, consider a kernel with a single scalar parameter.

```python
import tensorflow_probability as tfp

scalar_kernel = tfp.positive_semidefinite_kernels.SomeKernel(param=.5)
scalar_kernel.batch_shape
# ==> []

# Our inputs are two lists of 3-D vectors
x = np.ones([5, 3], np.float32)
y = np.ones([4, 3], np.float32)
scalar_kernel.matrix(x, y).shape
# ==> [5, 4]
```

The result comes from applying the kernel to the entries in `x` and `y`
pairwise, across all pairs:

  ```none
  | k(x[0], y[0])    k(x[0], y[1])  ...  k(x[0], y[3]) |
  | k(x[1], y[0])    k(x[1], y[1])  ...  k(x[1], y[3]) |
  |      ...              ...                 ...      |
  | k(x[4], y[0])    k(x[4], y[1])  ...  k(x[4], y[3]) |
  ```

Now consider a kernel with batched parameters with the same inputs

```python
batch_kernel = tfp.positive_semidefinite_kernels.SomeKernel(param=[1., .5])
batch_kernel.batch_shape
# ==> [2]

batch_kernel.matrix(x, y).shape
# ==> [2, 5, 4]
```

This results in a batch of 2 matrices, one computed from the kernel with
`param = 1.` and the other with `param = .5`.

We also support batching of the inputs. First, let's look at that with
the scalar kernel again.

```python
# Batch of 10 lists of 5 vectors of dimension 3
x = np.ones([10, 5, 3], np.float32)

# Batch of 10 lists of 4 vectors of dimension 3
y = np.ones([10, 4, 3], np.float32)

scalar_kernel.matrix(x, y).shape
# ==> [10, 5, 4]
```

The result is a batch of 10 matrices built from the batch of 10 lists of
input vectors. These batch shapes have to be broadcastable. The following
will *not* work:

```python
x = np.ones([10, 5, 3], np.float32)
y = np.ones([20, 4, 3], np.float32)
scalar_kernel.matrix(x, y).shape
# ==> Error! [10] and [20] can't broadcast.
```

Now let's consider batches of inputs in conjunction with batches of kernel
parameters. We require that the input batch shapes be broadcastable with
the kernel parameter batch shapes, otherwise we get an error:

```python
x = np.ones([10, 5, 3], np.float32)
y = np.ones([10, 4, 3], np.float32)

batch_kernel = tfp.positive_semidefinite_kernels.SomeKernel(params=[1., .5])
batch_kernel.batch_shape
# ==> [2]
batch_kernel.matrix(x, y).shape
# ==> Error! [2] and [10] can't broadcast.
```

The fix is to make the kernel parameter shape broadcastable with `[10]` (or
reshape the inputs to be broadcastable!):

```python
x = np.ones([10, 5, 3], np.float32)
y = np.ones([10, 4, 3], np.float32)

batch_kernel = tfp.positive_semidefinite_kernels.SomeKernel(
    params=[[1.], [.5]])
batch_kernel.batch_shape
# ==> [2, 1]
batch_kernel.matrix(x, y).shape
# ==> [2, 10, 5, 4]

# Or, make the inputs broadcastable:
x = np.ones([10, 1, 5, 3], np.float32)
y = np.ones([10, 1, 4, 3], np.float32)

batch_kernel = tfp.positive_semidefinite_kernels.SomeKernel(
    params=[1., .5])
batch_kernel.batch_shape
# ==> [2]
batch_kernel.matrix(x, y).shape
# ==> [10, 2, 5, 4]

```

Here, we have the result of applying the kernel, with 2 different
parameters, to each of a batch of 10 pairs of input lists.

<h3 id="with_name_scope"><code>with_name_scope</code></h3>

``` python
with_name_scope(
    cls,
    method
)
```

Decorator to automatically enter the module name scope.

```
class MyModule(tf.Module):
  @tf.Module.with_name_scope
  def __call__(self, x):
    if not hasattr(self, 'w'):
      self.w = tf.Variable(tf.random.normal([x.shape[1], 64]))
    return tf.matmul(x, self.w)
```

Using the above module would produce `tf.Variable`s and `tf.Tensor`s whose
names included the module name:

```
mod = MyModule()
mod(tf.ones([8, 32]))
# ==> <tf.Tensor: ...>
mod.w
# ==> <tf.Variable ...'my_module/w:0'>
```

#### Args:


* <b>`method`</b>: The method to wrap.


#### Returns:

The original method wrapped such that it enters the module's name scope.



