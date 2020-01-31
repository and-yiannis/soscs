#######
PyTorch
#######



# Create tensors
* Empty array
  * `x = torch.empty(5, 3)`
* Zeros, ones
  * `x = torch.zeros(5, 3)`
  * `x = torch.ones(5, 3)`
* Random arrays
  * `x = torch.rand(5, 3)`# Uniform
  * `x = torch.randn(5, 3)` # Gaussian
* Create 'like' tensors
  * `x = torch.ones_like(y)`
  * `x = torch.zeros_like(y)`
  * `x = torch.rand_like(y)`
  * `x = torch.randn_like(y)`
* directly from data
  * `x = torch.tensor([5, 3.4])`
* Types
  * `torch.long`
  * `torch.double`
  * `torch.float`
  * `x = torch.zeros(5, 3, dtype=torch.long)`

# Numpy bridge
## Torch from numpy
* `x = torch.from_numpy(y)`
## Numpy from torch
* `y = x.numpy()`
* `y = x.detach().numpy()`

# Reshaping 
* `x.view(-1, 1)`

# Addition
* `x + y`
* `torch.add(x, y)`
* `torch.add(x, y, out=result)`
* `y.add_(x)` # Addition in place

# Average
* `y = x.mean()`


# Automatic differentiation
Every tensor has a `requires_grad` property. If this is set to `True` then the gradient is stored and used in back propagation. Example:
`a = torch.randn(2,2)` # `requires_grad` is false by default
`a.requires_grad_(True)` # Set it to True
`a = torch.randn(2,2, requires_grad=True)` # set `requires_grad` to true

If a tensor has `requires_grad` set to true, then it also has a `grad_fn`
`a = torch.randn(2,2, requires_grad=True)` 
`a.grad_fn`

## Automatic differentiation example
The forward computation is the following
`x = torch.ones(2, 2, requires_grad=True)`
`z = 3 * (x + 2) ** 2`
`o = z.mean()`

We calculate the gradients backward
`o.backward()`

We find the gradient of `o` w.r.t. `x`
`x.grad`
This is a 2 x 2 matrix with value 4.5

# Layers and functionals
## `import torch.nn as nn`
* `nn.Conv2d` 
* `nn.Linear` 
## `import torch.nn.functional`
* `F.max_pool2d` 
* `F.relu` 
## Loss functions
* `criterion = nn.MSELoss()` 
* `criterion = nn.CrossEntropyLoss()` 
## Optimizers
`import torch.optim as optim`
* `optimizer = optim.SGD()`



`net.parameters()`
`net.zero_grad()`


--- clear the gradients of a net
net.zero_grad()

--- parameters
net.parameters()
returns a generator for all the network parameters

# Select 1 parameter
f = list(net.parameters())[0]
f.data # its values 
f.grad.data # its gradient

--- size 
x.size()





