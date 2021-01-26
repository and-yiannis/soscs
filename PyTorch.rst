#######
PyTorch
#######


General
#######

Create tensors
**************

.. code-block:: python

    #  Empty array
    x = torch.empty(5, 3)

    # Zeros, ones
    x = torch.zeros(5, 3)
    x = torch.ones(5, 3)

    # Random arrays
    x = torch.rand(5, 3) # Uniform
    x = torch.randn(5, 3)  # Gaussian

    #Create tensor directly from data
    x = torch.tensor([5, 3.4])

    #Create 'like' tensors
    x = torch.ones_like(y)
    x = torch.zeros_like(y)
    x = torch.rand_like(y)
    x = torch.randn_like(y)


**Types**

  * torch.long (64-bit integer)
  * torch.double (64-bit float)
  * torch.float (32-bit float)
  * :code:`x = torch.zeros(5, 3, dtype=torch.long)`

More types can be found in 
https://pytorch.org/docs/stable/tensors.html


Operations
**********

.. code-block:: python

    #Size
    x.size()
        
    #Reshaping
    x.view(-1, 1)
    
    #Addition
    x + y
    torch.add(x, y)
    torch.add(x, y, out=result)
    y.add_(x) # Addition in place
        
    #Average
    y = x.mean()
    
    #Get the value of a one element tensor
    y = x.item()


.. Note::

     Any operation that mutates a tensor is post-fixed with an :code:`_`. For example: :code:`x.copy_(y)`, :code:`x.t_()`, will change :code:`x`.


Numpy bridge
************

.. code-block:: python

    #Torch from numpy
    x = torch.from_numpy(y)
        
    #Numpy from torch
    y = x.numpy()
    y = x.detach().numpy()


GPU interaction
***************

.. code-block:: python

    #Is GPU available?
    torch.cuda.is_available()
    
    #Send tensor to GPU
    device = torch.device("cuda")
    x = x.to(device)
    # or
    x = x.to("cuda")
    
    #Bring back to cpu
    x = x.to("cpu")

The Neural network must also be sent to the same device where the tensors are, using the :code:`to` method. 

The methods :code:`is_cpu` and :code:`is_cuda` can check if a tensor is in the cpu or the gpu.
    

Automatic differentiation
#########################

Every tensor has a :code:`requires_grad` property. If this is set to :code:`True` all the operations on it are tracked.

Calling :code:`.backward()` calculates all the gradients automatically

The gradient for a tensor is stored into the :code:`grad` attribute. 

The gradient function for a tensor is stored into the :code:`grad_fn` attribute. 

An example of the automatic differentiation follows:

.. code-block:: python

    a = torch.randn(2,2) # `requires_grad` is false by default
    a.requires_grad_(True) # Set it to True
    # Alternatively
    a = torch.randn(2,2, requires_grad=True) # set `requires_grad` to true

If a tensor has :code:`requires_grad` set to true, then it also has a :code:`grad_fn`

.. code-block:: python

    a = torch.randn(2,2, requires_grad=True)
    a.grad_fn

The forward computation is 

.. code-block:: python

    x = torch.ones(2, 2, requires_grad=True)
    z = 3 * (x + 2) ** 2
    o = z.mean()

We calculate the gradients backward

.. code-block:: python

    o.backward()

We find the gradient of `o` w.r.t. `x`

.. code-block:: python

    x.grad

This is a 2 x 2 matrix with value 4.5

More on this example in https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html#sphx-glr-beginner-blitz-autograd-tutorial-py

To prevent tracking history you can wrap the code block in :code:`with torch.no_grad()`. This can be particularly helpful when evaluating a model, because the model may have trainable parameters with :code:`requires_grad=True`, for which we don't need the gradients.

To stop a tensor from tracking history, you can call :code:`.detach()` to detach it from the computation history and to prevent future computation from being tracked.

Layers and functionals
######################

**Layers**

.. code-block:: python

    import torch.nn as nn

    nn.Conv2d
    nn.Linear

**Functionals**

.. code-block:: python

    import torch.nn.functional as F

    F.max_pool2d
    F.relu

**Loss functions**


.. code-block:: python

    import torch.nn as nn

    # Select a criterion
    criterion = nn.MSELoss()
    criterion = nn.CrossEntropyLoss()

    # Calculate the loss
    loss = criterion(output, target)


**Optimizers**

.. code-block:: python

    import torch.optim as optim
    optimizer = optim.SGD(net.parameters(), lr=0.001)
    

**Manual parameter update**

.. code-block:: python

    for p in net.parameters():
        p.data.sub_(p.grad.data * learning_rate)


* :code:`net.parameters()` returns a generator for all the network parameters
* :code:`p.data` is the parameter's value
* :code:`p.grad.data` is the parameter's gradient

* :code:`.grad_fn` shows the gradient function
* :code:`.grad_fn.next_functions` shows the gradient function of the previous computational step. 





Examples
########

Network example
***************

This is an example of a convolutional neural network. The input image is assumed to be 32x32 pixels. In order to feed it to the network, the tensor needs to have a shape [1, 1, 32, 32]. The first dimension is the batch dimension, the second is the number of channels and the third is the size of the image. 

.. code-block:: python


    class Net(nn.Module):
    
        def __init__(self):
            super(Net, self).__init__()
            # Convolutional layer, 1 input channel, 6 output 
            # 3 x 3 convolution kernel
            self.conv1 = nn.Conv2d(1, 6, 3)  
            # Convolutional layer, 6 input channel, 16 output 
            # 3 x 3 convolution kernel
            self.conv2 = nn.Conv2d(6, 16, 3)
            # Linear layer, 16 * 6 * 6 inputs (based on the image size)
            # 120 outputs
            self.fc1 = nn.Linear(16 * 6 * 6, 120)
            # Linear layer, 120 inputs 84 outputs
            self.fc2 = nn.Linear(120, 84)
            # Linear layer, 84 inputs 10 outputs. 
            self.fc3 = nn.Linear(84, 10)
    
        def forward(self, x):
    
            # The first conv layer. Output is 1, 6, 30, 30
            x = self.conv1(x)
            x = F.relu(x)
            # Max pool: output is 1, 6, 15, 15 
            x = F.max_pool2d(x, (2, 2))
            # Conv. layer. Output is 1, 16, 13, 13
            x = self.conv2(x)
            x = F.relu(x)
            # Max pool: output is 1, 16, 6, 6
            x = F.max_pool2d(x, (2, 2))
            # Flatten the features
            x = x.view(1, -1)
            # Linear layer, 120 outputs
            x = F.relu(self.fc1(x))
            # Linear layer, 84 outputs
            x = F.relu(self.fc2(x))
            # Linear layer, 10 outputs
            x = self.fc3(x)
            return x
    
    # Define the network
    net = Net()
    # Draw a random input tensor.
    x = torch.rand(1, 1, 32, 32)
    # Propagate through the network
    net(x).size()

Training Example
****************

This is a sample code that goes in the training loop

.. code-block:: python

    # zero the gradient buffers
    optimizer.zero_grad()   
    # propagate the input through the net  
    output = net(input)
    # calculate the loss
    loss = criterion(output, target)
    # back propagate the gradients
    loss.backward()
    # update the parameters
    optimizer.step()    

Manual weight training Example
******************************

The following is a sample code with manual weight update

.. code-block:: python

    # the gradients are zeroed out through the network, 
    # as there's no optimiser
    net.zero_grad()   
    # propagate the input through the net  
    output = net(input)
    # calculate the loss
    loss = criterion(output, target)
    # back propagate the gradients
    loss.backward()
    # update the parameters
    for p in net.parameters():
        p.data.sub_(p.grad.data*learning_rate)


* clear the gradients of a net
   :code:`net.zero_grad()`

--- size 

x.size()


:code:`torchvision.utils.make_grid(images)` creates a grid of images for plotting.






